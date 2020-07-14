---
layout: post
title:  Quering MS SQL from Babel
date:   2020-07-14 14:21:57 +0200
category: [emacs, org-mode]
---

I finally managed to hook up `org-babel` to an MS SQL server, albeit
in a roundabout way. The key is to use the `dbi` engine and let Perl
do the heavy lifting. I'm documenting my work here for posterity.

## Install DBI

You need a working Perl installation, but the good news is that the
one shipped with OSX is sufficient, so you don't need to install and
maintain an entire Perl environment just for this. You do however need
to install the `DBI::Shell` module:

```bash
> perl -MCPAN -e shell

cpan shell -- CPAN exploration and modules installation (v2.00)
Enter 'h' for help.

> install DBI::Shell
```

## Install Microsofts ODBC drivers

These can be installed via homebrew:

```bash
> brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
> brew update
> brew install microsoft/mssql-release/msodbcsql17
```

This creates a driver entry called `ODBC Driver 17 for SQL Server` in
`/usr/local/etc/odbcinst.ini`. 

You should now be able to connect to your MS SQL Server from the command line:

```bash
> dbish "dbi:ODBC:Driver={ODBC Driver 17 for SQL Server};Server=<HOSTNAME>;UID=<USERNAME>;PWD=<PASSWORD>"
```

In theory you should be able to use the same string as an argument to
the org-babel dbi enginge, but I couldn't get it to work. Instead I
configured a name datasource in `~/.odbc.ini`

```ini
[my-data-source]
Driver = ODBC Driver 17 for SQL Server
Server = HOSTNAME
```

## Execute SQL queries from Babel

Finally, in my org-mode document I can now put this source block,
execute it and get the results right in the buffer:

```
#+name: select-users
#+header: :engine dbi
#+header: :cmdline dbi:ODBC:my-data-source <USERNAME> <PASSWORD>
#+begin_src sql
select top 20 userid, username from usertable;
#+end_src

+RESULTS: select-users
| userid | username    |
|--------+-------------|
| ...    | ...         | 
```


