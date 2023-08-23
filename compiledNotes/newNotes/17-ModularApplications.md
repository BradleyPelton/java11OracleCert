# Chapter 17 - Modular Applications
- module-info.java file.
- classpath vs Module path

### Reviewing Module Directives
- `exports <package>`                 - Allows all modules to access the package
- `exports <package> to <module>`     - Allows a specific module to access the package
- `requires <module>`                 - Indicates module is dependent on another module
- `requires transitive <module>`      - Indicates the module and all downstream modules are dependent on module
- `uses <interface>`                  - Indicates that a module uses a service
- `provides <interface> with <class>` - Indicates that a module provides an implementation of a service

### Comparing Types of Modules
- Three types of modules:
    1. Named modules
    2. Unnamed modules
    3. Automatic modules

### Named Module
- defn: named module - a module containing a module-info file.
- Named modules appear on the module path rather than the classpath.
-  A named module has the name defined inside the module-info file.

### Automatic Module
- defn: automatic module - a module on the module path but does not contain a module-info file.
- Simply a regular JAR file that is placed on the module path and is treated as a module
- Java "automatically" determines the module name if `Automatic-Module-Name` field is not added MANIFEST.MF
- the code referencing an automatic module treats it as if there is a module-info file present.
- It automatically exports all packages
- It also determines the module name. Either MANIFEST.MF has the value stated, or JAR file name is used (after stripping .jar, version, and converting special characters to "." periods).

### Unnamed Modules
- defn: unnamed module - module that appears on the classpath
- Like automatic modules, unnamed modules are regular JARs
- Unlike automatic modules, unnamed modules appear on the classpath (rather than the module path)
- unnamed modules are treated like old code and as second-class citizens to modules.
- An unnamed module does not USUALLY contain a module-info file. If it does, it is ignored
- Unnamed modules do not export any packages to named or automatic modules. 
- Unnamed module can read from any JARs on the classpath or module path.
- Unnamed modules are similar to the way "java worked before modules".

### Comparing Module Types
- Key Point: code on the classpath can access the module path, but code on the module path can't read from the class path
- Chart 17.3 on page 810

---------------------------------------------------------------------------------------------------

# JDK Dependencies
### Built-in Modules
- java.base is the most import module. It contains most of the packages
- java.base is available without using the requires directive
- other famous modules `java.logging, java.sql, java.xml, java.desktop`
- most core modules start with `java.` but jdk modules start with `jdk.` (e.g. `jdk.javadoc, jdk.charsets, jdk.jdpes`)

### jdeps
- defn: jdeps - gives you information about dependencies
- jdeps can be used to assist in identifying dependencies and problems for projects not yet modularized.
- `jdeps zoo.dino.jar` 
- jdeps against a JAR
- jdeps prints the modules that we would need to use the require directives on.
- jdeps also prints what packages are used and their corresponding modules 
- summary mode
    * `jdeps -s` or `-summary`
    * if we run jdeps in summary mode, we only see the modules that we would need to use the require directive on.
- jdk internals
    * `jdeps --jdk-internals zoo.dino.jar`
    * `jdeps -jdkinternals   zoo.dino.jar`
    * lists any classes that you are using that call an internal API

---------------------------------------------------------------------------------------------------

# Migration Applications
- ordering modules
- bottom-up migration
- top-down migration
- how to split up an existing project

### Determining the order
- Before you can migrate an application to use modules, you need to know how the libraries in the existing app are structured
- dependency graph
- BOTTOM = Projects that do not have any dependencies
- TOP = Projects with (the most) dependencies
- Arrow points FROM X -> Y. X depends on Y. x requires y. y exports packages. 
```
chicken
 ↓
nest
 ↓
egg
```

### Bottom-Up migration
- easiest migration approach
- works best when you have the power to convert any JAR files that aren't already modules.
- Steps:
    1. Pick the lowest-level project that has not yet been migrated
    2. Add a module-info.java (including exports and requires directives)
    3. Move this newly migrated named module from the classpath to the module path
    4. Ensure any projects that have not yet been migrated stay as unnamed modules on the class path (instead of automatic)
    5. Repeat with the next-lowest-level project 
- in effect, we are moving (unnamed) -> (named)

### Top-down migration
- most useful when you don't have control of every JAR file used by your app.
- Steps:
    1. Place all projects on the module path (making all modules automatic)
    2. Pick the highest-level project that has not yet been migrated to modules
    3. Add a module-info file to convert this module from automatic -> named. (remember exports and requires directives)
    4. Repeat with the next-lowest-level project
- in affect, converts everything to automatic
- then, adding module-info files one-by-one making each package (automatic) -> (named)

### Cyclic dependencies
- defn: cyclic dependencies - aka circular dependency - when two things directly or indirectly depend on each other.
- if you run into cyclic dependency, just merge the two modules together.
- java will not allow you to compile modules with circular dependencies. 
- Cyclic dependencies are allowed between packages, but not modules.

### Creating a Service
- defn: service - composed of an interface, any classes that interface references, and a way of looking up implementations of the interface
- Implementations are not part of the service
- defn: service provider interface - "interface/abstract class" that specifies what behavior this service has.
- service provider "interface" can be either an interface or abstract class. 
- defn: service locator - finds any classes that implement a service provider interface
- Java provides ServiceLocator class to be a service locator. 
- `ServiceLocator<tour> loader = ServiceLoader.load(Tour.class)` where Tour is the interface being `uses`
- ServiceLoader was added to java.util in JDK6, prior to that the basic technology was used in the Service class.


### Using a service as a Consumer
- defn: Consumer - aka client - is a module that obtains and uses a service
- defn: service provider - implementation of a service provider "interface"
- Recall it's possible to have multiple implementation classes or modules
- `provides zoo.tours.api.Tour with zoo.tours.agency.TourImpl`













---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
