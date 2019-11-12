---
layout: post
title: Generating java code with maven, QDox and StringTemplate
published: true
categories: [java]
---

## Introduction: why would you ever want to generate Java code?

In my current project at work we have quite a few value objects with
giant constructor methods; these behemoths take 20 to 30 arguments
each and it's crucial that you get all of them in the right order,
which is a pain in those cases when you're only really interested in
setting one or two of them. Just as an example, let's just say that
these classes looks something like this:

``` java
public class ValueObj {
   private int value1;
   private int value2;
   private int value3;
   ...
   private Object obj1;
   private Object obj1;
   ...
   public ValueObj(int value1, int value2, int value2, ... Object
       obj1, Object obj2, ...) {
      this.value1 = value1;
      this.value2 = value2;
      this.value2 = value3;
      ...
      this.obj1 = obj1;
      this.obj2 = obj2;
      ...
   }

   public int getValue1() {
      return this.value1;
   }
   ... (other getter methods follow)
}
```

About a year ago or so we started using the
[Builder pattern][pattern], creating a separate *Builder class* for
each value object class. The builder has one setter method for each
parameter, and one `create()` method that instantiates the new object.

``` java
public class ValueObjBuilder {
   private int value1;
   private int value2;
   private int value3;
   ...
   private Object obj1;
   private Object obj1;
   ...

   public ValueObjBuilder value1(int value1) {
       this.value1 = value1;
       return this;
   }
   public ValueObjBuilder value2(int value2) {
       this.value2 = value2;
       return this;
   }
   ...

   public ValueObj create() {
       return new ValueObj(value1, value2, ...);
   }
}
```

Since the setter methods return a reference to the builder itself it
is possible to chain invocations:

``` java
ValueObj obj = new ValueObjBuilder().value1(23).value2(47).create();
```

However convenient these builder classes are to use, I very quickly
tired of maintaining them. Every time you add or remove a property to
a value object, you also need to make the corresponding change to the
builder. Keeping two classes in sync doesn't seem like too much work,
but it is unnecessary, and it occurred to me that I could probably
write something to generate the builders automatically.

## Writing a Maven plugin

We use [Maven][mvn] for our builds. Ok, ok, I know. Maven is horrible,
maven downloads the whole universe, maven is braindead. Yes, I know
all these things. But here's the ting: *Maven is like democracy: It's
not perfect, but it's the best we have*.

One of the nice things about maven is that it has a well-defined
standard build lifecycle. The build process is divided into steps, and
during one of the steps, `generate-sources`, a maven plugin has the
opportunity to generate source that will then be compiled during the
`compile` step.

When invoked, we'd like the plugin to:

 1. Loop over our java classes, looking for constructors marked with
 our custom `@builder` javadoc comment.

 2. For each matching constructor, generate a new source file under
 `target/generated-sources/builderbuilder/` containing the Builder
 class.

The plugin is invoked *before* the compilation step, meaning that all
the generated code will be included in the final output.

As an example, this code (in `src/main/java/example/ValueObj.java`):

``` java
package example;

public class Cat {
   private String name;
   private int age;

   /**
    * @builder
    */
   public Cat(String name, int age) {
      this.name = name;
      this.age = age;
   }
}
```

Will generate the following (in
`target/generated-sources/builderbuilder/example/CatBuilder.java)` when run through our plugin:

``` java
package example;

public class CatBuilder {
   private String name;
   private int age;

   public CatBuilder name(String name) {
      this.name = name;
      return this;
   }

   public CatBuilder age(int age) {
      this.age = age;
      return this;
   }

   public Cat create() {
      return new Cat(name, age);
   }
}
```

For those of you who like to read along, the code for the whole plugin
is [on github][bb]. The finished version has some extra bells and
whistles, like the ability to generate abstract builder classes and
explicitly the name of the `create()` method.

## Parsing Java code

Okay, so let's say we want to pick apart the java class files and for
each constructor found we want to generate a helper class as discussed
above. Now, normally we'd use the introspection facilities already
built into java, but now we have one problem: our code-generating code
will run during the `generate-sources` step, *before* any compilation
has actually taken places.

This is a problem. What we need is a java parser that can pull apart
the relevant bits of the source files and give us enough information
that so we can generate the new classes. Preferably the level of
detail should be on par with with that of the `java.util.reflect.*`
functions, and it should also be fast. I actually started writing
[something like that][timjan], but that's another story. Instead,
let's check out [QDox][qdox], an amazing little library used
internally by maven.

Using it is pretty simple. First, create a new `JavaDocBuilder` object
and tell it where the source code you want to parse lives:

``` java
this.docBuilder = new JavaDocBuilder();
for (String r : sources) {
    docBuilder.addSourceTree(new File(r));
}
```

Then loop over all known classes and generate the builder:

``` java
for (JavaClass jc : docBuilder.getClasses()) {
    generateBuilderFor(jc);
}
```

## Generating the output

This is the `generateBuilderFor()` method:

{% highlight java linenos %}
public void generateBuilderFor(JavaClass jc) throws IOException {
    for(JavaMethod m: jc.getMethods()) {
        if (m.isConstructor()) {
            DocletTag dc = m.getTagByName("builder");
            if (dc != null) {
                String builderName = dc.getNamedParameter("name");
                if (builderName == null)
                    builderName = jc.getName() + "Builder";

                String packageName = dc.getNamedParameter("package");
                if (packageName == null)
                    packageName = jc.getPackageName();

                StringTemplate st = templates.getInstanceOf("builder");
                st.setAttribute("packageName", packageName);
                st.setAttribute("builderName", builderName);
                st.setAttribute("resultClass", jc.asType().toString());

                List<Param> ps = new LinkedList<Param>();
                for(JavaParameter p: m.getParameters()) {
                       ps.add(new Param(p.getType().toGenericString(), p.getName()));
                }
                st.setAttribute("parameters", ps);

                File pd = new File(outputDirectory, packageName.replaceAll("\\.", "/"));
                pd.mkdirs();

                FileWriter out = new FileWriter(new File(pd, builderName + ".java"));
                try {
                    out.append(st.toString());
                } finally {
                    out.flush();
                    out.close();
                }
            }
        }
    }
}

public static class Param {
    public final String type;
    public final String name;
        public Param(String type, String name) {
        this.type = type;
        this.name = name;
    }
}
{% endhighlight %}

This is all pretty straightforward; we loop through the methods for
the class, looking for a constructor that has the `@builder` javadoc
tag.

Lines 6-12 checks if the tag has any values specified for `name`
and `package`, reverting to default values if not.

Lines 14-23 instantiates a new `StringTemplate` object and supplies it
with values for the `packageName`, `builderName` and `resultClass`
attributes (the latter being the class that we want the builder to
actually build). Lines 19-23 loops through the constructor arguments
and creates value object holding the name & type of each.

Finally, a new file is created (lines 25-26) and written (lines
28-34).

## Templating

There are an abundance of templating frameworks out there. I choose to
go with [StringTemplate][st] for this project, since it makes it easy
to use subtemplates in a functional way, and generally fits my way of
thinking. I could just as easily have used something like
[FreeMarker][fm] or [Velocity][velocity], though.

The templates are not that interesting, so I won't go into it
here. You can check them out [on github][stg] if you are curious.

## Conclusion

Automatic code generation in java projects is not only feasible, but
also quite convenient with maven. We've been using [BuilderBuilder][bb]
internally in production for a few months now, and it greatly cuts
down on code maintenance.

It would be nice, though, if there was a more general approach to code
generation. Just for [BuilderBuilder][bb], I had to dive into [Maven][mvn]
internals (which are surprisingly poorly documented), learn
[QDox][qdox] (ditto) and write all the java code that solved this
particular problem. Imagine instead if there was some kind of
transformation language that operated on java sources, kind of like
what [XSLT][xslt] does for XML? Now, that would be cool.

[pattern]:http://c2.org
[timjan]:http://github.com/fred-o/timjan
[bb]:http://github.com/fred-o/BuilderBuilder
[stg]:https://github.com/fred-o/BuilderBuilder/blob/master/src/main/resources/builderbuilder.stg
[qdox]:http://qdox.codehaus.org
[st]:http://stringtemplate.org
[fm]:http://freemarker.sourceforge.net/
[velocity]:http://velocity.apache.org/
[xslt]:http://www.w3.org/Style/XSL/
[mvn]:http://maven.apache.org/
