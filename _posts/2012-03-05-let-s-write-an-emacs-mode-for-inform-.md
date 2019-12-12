---
title: Let's write an emacs mode for Inform 7!
layout: post
published: true
categories: [emacs, inform]
---

So, my love affair with [Inform][i7] [continues][fun]. Whatever love I
housed for the in-built editor however, has withered and died. The
keybindings all felt wrong, I kept accidentally narrowing the scope of
the code, and had to use the mouse way too much for it to be a
pleasant writing experience. So I thought, Hey, why not write an emacs
mode for Inform? I've been meaning to learn about major modes and
syntax highlighting anyway, and this is as good an excuse as
any. After a bit of research, it turned out that creating a new major
mode was pretty straightforward.

First, since we want headings in the source code to stand out, we'll
define a new font face, based on
`font-lock-preprocessor-face`. It could be inherited from any face,
really, but let's pick one that's likely to have been included in most
colour themes.

``` cl
(defface inform7-heading-face
  '((t (:inherit font-lock-preprocessor-face :weight bold :height 1.2)))
  "Face for Inform 7 headings"
  :group 'font-lock-highlighting-faces)
```

Then some keybindings for our mode. There's not much for us to do
here, but let's make RET automatically jump to the correct indentation
level; that's always nice:

``` cl
(defvar inform7-mode-map
  (let ((map (make-sparse-keymap)))
    (define-key map (kbd "RET") 'newline-and-indent)
    (define-key map "\C-j" 'newline-and-indent)
    map)
  "Keymap for inform7 major mode")
```

The easiest way to add syntax highlighting for our mode is to use
regexp-based matchers. Inform 7 hasn't got that much of a formal
syntax, but it would be nice to highlight keywords like _if_, _then_
and _otherwise_ and for section headings in the text. We should also
find some way of supporting indexed text (i.e. anthing inside a string
surrounded by \[]).

The last one is a bit tricky, as the same delimiters are also used for
comments (see below), and it's not actually possible to distinguishing
between them using only regexps. The solution here, prepending a '.'
to the matcher, is a workaround that's good enough for most cases
though.

One thing to watch out for is that that the names of the faces must be
symbols. For the built-in faces there are variables defined for each
of these, but we must be careful to quote the name of the face we
defined above.

``` cl
(defconst inform7-font-lock-keywords
  `(( ,(regexp-opt '("Include" "Use" "let" "say" "if" "otherwise") 'words) . font-lock-keyword-face)
    ("^\\(\\(?:Book\\|Chapter\\|Part\\|Section\\|Volume\\) - .*\\)" . 'inform7-heading-face)
    (".\\(\\[.*?\\]\\)." 0 font-lock-variable-name-face t)
    )
  "Highlighting expressions for inform7-mode")
```

We now have everything we need to define our new mode. Current wisdom
holds that it's a good idea to extend an existing mode using
`define-derived-mode`, and have someone else do all the heavy
lifting. In our case, we want to extend [sws-mode], which handles
significant whitespace and provides functions for intelligent
indentation of lines and blocks of code. It is part of
[jade-mode][sws-mode].

Defining our major mode is deceptively simple; we just pass in a mode
name, the name of the mode we're deriving from, a printable name that
will be displayed in the status line, and a body of code that will be
executed when the mode is initialized. `define-derived-mode` will take
care of the rest and set up a keymap, syntax table, mode hooks and
ensure that the parent mode and the hooks are called on init. In our
case, we've already created the keymap manually, so
`define-derived-mode` will use that one instead.

```cl
(define-derived-mode inform7-mode sws-mode "Inform7"
  "Major mode for editing inform 7 story files."
  (visual-line-mode)
  (set (make-local-variable 'font-lock-defaults) '(inform7-font-lock-keywords nil t)))
```

Two things remain, though. First, the line indent function does always
increases the indentation of new/empty lines by one step. For inform
code, we only want that if the preceding line ends with a ':',
otherwise we'd like the new line to have the same indentation level as
the previous one.

It turns out, that can be accomplished by adding some advice around
`sws-do-indent-line`, which is the function in sws-mode that actually
carries out the indentation:

```cl
(defadvice sws-do-indent-line (around inform7-indent-blank () activate)
  "Ensure proper indentation of blank lines according to Inform7 conventions."
  (if (and (eq major-mode 'inform7-mode) (sws-empty-line-p))
    (if (save-excursion
          (previous-line)
          (looking-at "^.*:[ \t]*$"))
        (indent-to (sws-max-indent))
      (indent-to (sws-previous-indentation)))
    ad-do-it))
```

The other thing we need to fix is highlighting of comments in the
text. We don't want to do this with regexps, as it's tricky to get
this working reliably for multi-line comments. Instead, we'll take
advantage of the built-in facilities for syntactic
highlighting. Incidentally, you may have noticed that we didn't have
to do anything to get highlighting of strings in the code; the
syntactic parser gives us that functionality for free.

These two lines modifies the syntax table, telling the parser to treat
\[ and \] as comment delimiters.

```cl
(modify-syntax-entry ?\[ "<]" inform7-mode-syntax-table)
(modify-syntax-entry ?\] ">[" inform7-mode-syntax-table)
```

So there we are, [41 lines of code][el] all in all, not bad for a start.

[el]:https://github.com/fred-o/inform7-mode/blob/master/inform7-mode.el
[sws-mode]:https://github.com/brianc/jade-mode
[fun]:http://localhost:4000/2011/02/11/fun-with-inform7.html
[i7]:http://inform7.com

