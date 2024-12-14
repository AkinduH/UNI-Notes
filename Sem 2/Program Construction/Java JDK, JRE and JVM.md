## What is JVM?

JVM (Java Virtual Machine) is an abstract machine that enables your computer to run a Java program.

When you run the Java program, Java compiler first compiles your Java code to bytecode. Then, the JVM translates bytecode into native machine code (set of instructions that a computer's CPU executes directly).

Java is a platform-independent language. It's because when you write Java code, it's ultimately written for JVM but not your physical machine (computer). Since JVM ​executes the Java bytecode which is platform-independent, Java is platform-independent.

![How does Java program work?](https://cdn.programiz.com/sites/tutorial2program/files/how-java-program-runs.jpg "Working of Java Program")

Working of Java Program

If you are interested in learning about JVM Architecture, visit [The JVM Architecture Explained](https://dzone.com/articles/jvm-architecture-explained).

---

## What is JRE?

JRE (Java Runtime Environment) is a software package that provides Java class libraries, Java Virtual Machine (JVM), and other components that are required to run Java applications.

JRE is the superset of JVM.

![JRE contains JVM and other Java class libraries.](https://cdn.programiz.com/sites/tutorial2program/files/java-realtime-enviornment_0.jpg "Java Runtime Environment")

Java Runtime Environment

If you need to run Java programs, but not develop them, JRE is what you need. You can download JRE from [Java SE Runtime Environment 8 Downloads](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) page.

---

## What is JDK?

JDK (Java Development Kit) is a software development kit required to develop applications in Java. When you download JDK, JRE is also downloaded with it.

In addition to JRE, JDK also contains a number of development tools (compilers, JavaDoc, Java Debugger, etc).

![JDK contains JRE and other tools to develop Java applications.](https://cdn.programiz.com/sites/tutorial2program/files/java-development-kit.jpg "Java development kit")

Java Development Kit

If you want to develop Java applications, [download JDK](http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html).

---

## Relationship between JVM, JRE, and JDK.

![JRE contains JVM and class libraries and JDK contains JRE, compilers, debuggers, and JavaDoc](https://cdn.programiz.com/sites/tutorial2program/files/jdk-jre-jvm.jpg "Relationship between JVM, JRE, and JDK")