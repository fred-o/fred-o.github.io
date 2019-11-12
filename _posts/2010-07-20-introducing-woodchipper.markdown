---
layout: post
title: Introducing Woodchipper
---

Well, my [last post][nolog] may have been a bit of a rant, but it did
spur my creativity. The more I thought about automatically replacing
logging statements with `System.out.println()`, the more the idea
appealed to me. A couple of days later I had a day off with beautiful
weather and nothing to do, and I spent it out on the balcony, hacking
frantically. In the end I had a rough proof of concept that I've now
refined to the point that I'm not embarrased to show it to people.

Ladies and gentlemen, I give you [WoodChipper][wc]. It is a command
line utility that will automatically remove all [Log4j][log4j] and
[Commons Logging][commonslog] statements from a given .jar file or a
classpath directory. This happens through bytecode manipulation, so
the dependencies are removed completely. There is no classpath
trickery like with [Slf4j][slf4j] (which supplies its own replacement
jars for the underlying logging system), no fiddling with your library
path, no more need to write exclusion filters in maven. You can run it
on your own code, but it's really designed to work on 3rd party .jar
files where you can't influence the choice of logging framework.

Depending on your opinion on the state of Java logging frameworks,
this is either wonderful or a terrible idea. Anyway, let me know what
you think.

## Demo

You need to have git and maven installed. Now get a bash prompt and
download and install WoodChipper:

    > git clone http://github.com/fred-o/woodchipper
    > cd woodchipper
    > mvn package
    
    [lots and lots of maven output]

This builds `woodchipper` and `woodchipper-test-jar`. The latter is a
small project with trivial classes you can use for testing. Let's
start by trying to run one of the classes directly:

    > java -cp woodchipper-test-jar/target/woodchipper-test-jar-1.0-SNAPSHOT.jar woodchipper.TestClass

    Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/log4j/Priority
    Caused by: java.lang.ClassNotFoundException: org.apache.log4j.Priority
        at java.net.URLClassLoader$1.run(URLClassLoader.java:202)
        at java.security.AccessController.doPrivileged(Native Method)
        at java.net.URLClassLoader.findClass(URLClassLoader.java:190)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:307)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:301)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:248)

Ok, so we get `NoClassDefFoundError`. That's actually expected, as I
didn't include any log4j .jar file on the classpath. Anyway, let's run
woodchipper on the sucker:

    > java -jar woodchipper/target/woodchipper.jar -i \
        woodchipper-test-jar/target/woodchipper-test-jar-1.0-SNAPSHOT.jar 

    Removing Log4J references from woodchipper/TestClass.class
    Removing Commons Logging references from woodchipper/TestClass4.class

Looks like it's working. Let's try running it again:

    > java -cp woodchipper-test-jar/target/woodchipper-test-jar-1.0-SNAPSHOT.jar woodchipper.TestClass

    first message!
    second message!
    third message!
    fourth message!
    fifth message (with exception!)
    java.lang.IllegalArgumentException
        at woodchipper.TestClass.main(TestClass.java:29)
	
Yay! No log4j dependency anymore! (Note that the exception at the end
isn't acutally an error with WoodChipper, just a logged exception).

[nolog]:{% post_url 2010-06-22-nolog %}
[wc]:http://github.com/fred-o/woodchipper
[log4j]:http://logging.apache.org/log4j/1.2/
[jdklog]:http://java.sun.com/j2se/1.4.2/docs/guide/util/logging/overview.html
[commonslog]:http://commons.apache.org/logging/
[slf4j]:http://slf4j.org
