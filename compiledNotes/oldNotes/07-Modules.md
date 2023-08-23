
# Modules
- Defn: A module groups related code under a module name and is self contained
- Modules are a new level of abstraction in java
- modules groups related packages
- modules control access
- modules are still 100% optional
- jar file != modules
- JEP 261: Module System

### Module Syntax
- best practice: exports on top
- best practice: group exports, opens, requires
```
import greeter.api.messageservice

[open] module <module_name> {
    exports <package>;
    exports <package> to <module_name>;
    opens <package>;
    requires <module_name>;

    uses <package>.<type_name>;

    provides <package>.<type_name> with <package>.<type_name>

    uses MessageService
}
```

### Pros
- Understanndability - explicit boundaries and dependencies
- maintainability - hiding implementation details
- Flexibility - decoupling of parts of your system
- Strong Encapsulation - Hide internals
- Well Defined Interface - Don't break downstream dependencies
- Explicit dependiecies - naming dependiecies 

### Creating Modules
- module-info.java
```
module myfirstmodule {
}
```
- Not a real java class because java files can't have dashes
- Should be in the top level directory of module (above the packages)
- module names live in a separate namespace, no clashes with class or packages
- module names use dots like package names
- module names shouldn't use digits
- root package is a common moduile name (e.g. "com.pluralsight")
- A jar file that contains a module-info.class is called a "modular JAR file"
- modular JAR files behave differently than regular JAR files

### Module Compilation
- Modules require a module-info.java file
- classPath != modulePath
- module path knows how to work with module declaration

### How to compile & run Java module
1. Create module-info.java next to main class
2. Compile (javac -d {all source files})
3. Run the program using `java -p {module_path}`
- Alternatively `javac -d out --module-source-path src -m modulename`
- `java modulepath`

- `java -p {module_path} -m {modu;es}/{fully qualified main class}`
- `java --module-path mods --module module/className`

### Exporting Modules
- syntax `exports {package_name}`
- other modules can now use the exported package
- Doesn't give recursive access, jsut depth = 1
```
module mymodule {
    exports com.pluralsight;
}
```

### Requires and Importing
- syntax `requires {module_name}`
- NOTE: "require" uses module name, but "exports" uses package name
- all modules implicitly require/depend on `java.base`
- `java.base` is part of the modular jdk
- Optional to expresss this dependency
- Cyclical dependencies are not allowed

```
module mymodile {
    requires util;
    requires java.base;
}
module util {
    exports com.pluralsight.util;
}
```

### Open Package
- "reflective access"
- Bypass access restrictions
- Export AND open
- an open module implicitly opens all packages
```
module mymodule {
    exports com.pluralsight;
    opens com.pluralsight;
    opens com.pluralsight.util;
}
open module mymodule {
    exports com.pluralsight;
}
```

### Qualified Exports and Opens
- Strictly limit access using qualified exports
- some exports qualified, some not
```
module util {
    exports com.pluralsight.util to mymodule;
}
module util {
    opens com.pluralsight.util to mymodule,anothermodule;
}
```

### Modular JDK
- The first modular app was the JDK itself
- Around 71 modules in JDK
- Two types of modules (java.* and jdk.*)
- all modules that start with java.* are part of the Java specification
- jdk modules are implementation dependent

###
- modular resolution
- a lot of sanity checks will be applied
- this guarantees no name collisions,cycles, invalid dependencies, etc.

### Requires Transitive
- similar to dependenciy transitives in maven
```
module mymodule {
    requires transitive java.logging;
}
```
### Packaging a module
- first compile to the out/ folder, then package with jar command
- `jar --create --file mods/pluralsight.jar -C out/ .`
- New packaging format `jmod`
- Not executable
- can handle Legal notices
- intended for use with JLINK

### Services
- Syntax `provides {shared_contract} with {implementation_type}`
```
module greeter.hello {
    requires greeter.api
    prvides greer.api.MessageService with greeter.hello.service.HelloMessageService;
}
module greeter.api {
    exports greeter.api
}
public interface MessageService {
    String getMessage();
}
module.greeter.cli {
    requires greeter.api

    uses greeter.api.MessageService
}
```
- Service Loader
- `Iterable<MessageService> messageService = ServiceLoader.load(MessageService.class);`
- Services allow us to compare modules at runtime in a flexible manner

### Unnamed module
- Backwards compatability for people who don't use modules
- doesn't list any dependencies
- can read everything from the modular JDK at runtime
- WarningL AN illegal reflective access operation has occured
- JDK 16 will throw errors if you don't use modules
- Bottom-up Migration (create moduiles, create modules-info.java)
- Top-doiwn Migration

### JDEPS
- `jdeps -java Dependencies`
- jdeps tells which module a jar depends on
- jdeps can even generate `module-info.jar` file

### Automatic Modules
- Top down migration
- automatic modules try to create a name
- an automatic module opens and exports everything (all packages)
- an automatic module required all resolved module because there is no way to know what it requires
- code on auitomatic modules can access code on the class path (named modules can't)
- Automatic moduiles area "necessary evil" until full migration






























