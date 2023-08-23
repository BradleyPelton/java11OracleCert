# Chapter 11 - Modules
- JPMS - Java Platform Module System  - introduced in Java 9
- Defn: Module - group of one or more packages(plus static resources) plus a module-info.Java file.
- Module dependencies where one module relies on code in another
- module-info.java is required to be inside all modules.
- module names and package names are closely related. Generally, packages are "module_name" + "other_stuff"

### Benefits of Modules
- Better access control -  Allows targeted "public" access. Fifth level of access control. Expose packages to only specific packages.
- Clearer dependency management - Forcing users to specify which packages are dependent on others
- Smaller Java distributions - Smaller JDKs which only use some packages.
- Improved performance - Java only needs to look at subset of package at class loading time.
- Unique package enforcement - same packages, different versions. 

### Creating module-info
- module-info must be in the root directory of your module. 
- top level type in the module-info is the `module` keyword
- same naming rules for package names. 
- access modifier not allowed for top-level type of "module" in module-info.java

### Compiling Modules
- Compiling modules uses the same `javac` command but with different arguments as the classpath arguments.
```
javac 
--module-path mods
-d feeding
feeding/zoo/animal/feeding/*.java feeding/module-info.java
```
- replacing --class-path with --module-path
- "--module-path" indicates the location of any custom module files. Optional if no dependencies.
- "-d" option specifies the directory to place the class files
- the last two paths in the command is a list of the .java files to compile
- New option for `javac` command     `--module-path` short form is `-p`

### Running Modules
- `java --module-path feeding --module zoo.animal.feeding/zoo.animal.feeding.Task`
- `java --module-path mods --module book.module/com.sybex.OCP`
- Location of modules after --module-path
- module name after --module
- Forward slash between module name and class name `module.name/fully.qualified.class.Name`
- New option for `java` command        `--module` short form is `-m`
    * takes argument of module containing class which contains the main method
- New option for `java` command     `--module-path` short form is `-p`

### Packaging Module
- jar'ing modules is identical to jar'ing packages.
- Same arguments as before (-cvf -C)
- `jar -cvf mods/zoo.animal.feeding.jar -C feeding/ .`

### Keywords
- defn: Module Directives - "statements" that appear in module-info.java files (e.g. exports, requires, etc.)
- Directives can appear in any order in the module-info file
- module "keywords" are only keywords inside a module-info.java file. In other files, you are free to use these names elsewhere (e.g. provides, open, exports, etc.)

### Exports
- defn: Exports - used to indicate that a module intends for those packages to be used by Java outside the module.
- Exporting makes public types (and protected) types visible. Not private or package-private. Same access rules after exported.
- Without an export, the module is only available internally
- `exports zoo.animal.feeding;`
- Qualified export: `exports zoo.animal.talks to zoo.staff;`

### Requires
- defn: Requires - a module is needed.
- `requires zoo.animal.feeding;`
- `requires transitive zoo.animal.feeding`
- `requires static zoo.animal.feeding`
- requires transitive = any module that requires this module will also transitively depend on the modules this module marks as "requires transitive"
- requires transitive is like "requires" + "extra behavior"
- Compiler error if `require x` and `requires transitive x` in the same module-info.
- all modules implicitly require `java.base`

### provides, uses, and opens
- provides keyword specifies that a class provides an implementation of a service
- uses keyword specifies that a module is relying on a service

### New java commands
- `java --describe-module` or `-d`
- `java --list-modules`
- `java --show-module-resolution`

### New jar Commands
- `jar --describe-module` // similar to java describe module

### jdeps
- jdeps gives you information about dependencies within a module.
- tells you what dependencies are actually used rather than simply declared
- `jdeps -s`/ `jdeps -summary` 
- specific module path

### jmod
- only for working with JMOD files
- Don't have to learn the syntax











---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
