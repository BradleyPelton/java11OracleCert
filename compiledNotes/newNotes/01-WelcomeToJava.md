# Chapter 1 - Welcome To Java
# Basic commands
`javac Zoo.java`
`java Zoo`

# Compiling 
### Compiling with wildcards
- `javac packaged/*.java`
- wildcard expansion only works for depth=1, you can't just run `javac *.java`
- classpath - the preferred way of setting classpath is with `-cp` flag

### javac command
- defn: javac - read source files and compiles them into class files
- `-cp <classpath>`                -   
- `-classpath <classpath>`         - Location of classes
- `--class-path <classpath>`       -
- `-d <dir>`                       - Directory to place generated .class files

### java command
- defn: java - command to start a Java application
- `-cp <classpath>`                -   
- `-classpath <classpath>`         - Location of .class files that are already compiled
- `--class-path <classpath>`        -

### jar command
- java archive file
- similar to a zip file of mainly .class files
- `-c / --create`                 - creates a new JAR file
- `-v / --verbose`               - Prints details of creation process
- `-f / --file <fileName>`        - JAR filename
- `-C <directory>`                - Directory containing files to be used to create the JAR

### single-file source-code programs
- new variant: In Java 11, it's also possible to run a program with just `java Zoo.java` without compiling first
- only works for one-file programs
- single-file programs can only use core-package imports

# Importing 
### Wildcard import
- `import java.util.*;`
- wildcard imports don't import child packages, fields, or methods. Just imports classes
- depth = 1
- invalid : `import java.nio.*.*;` (only one wildcard allowed)
- java.lang is automatically imported 

# Order of class files
- P.I.C.
- package declaration
- imports
- class declaration 
- NOTE: Only one top level public class/interface/enum allowed

---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
