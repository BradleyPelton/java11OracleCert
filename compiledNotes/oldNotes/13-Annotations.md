### Annotations
- @Override
- @NonNull annontation on parameters
- Spring boot uses A LOT of annotations

### Built-in ANnotation Types
- @Override
- @Deprecated
- @SuppressWarnings
    * @SuppressWarnings("Unused")
    * @SuppressWarnings("deprecated")

### Creating annotations
- use @interface top level type
- you can provide default values
- In Intellij, when creating a file, choose "annotation"

```
public @interface Version {
    int value;
}
@Version(value = 12)
```

###
- Normal Annotations - Optional arguments
- Marker Annotation - do no accept arguments. (e.g. @Deprecated)
- Single-element Annotation - Single Argument (e.g. @Platform, @Languages({"Java", "Kotlin"})) 
- Single argument mean can mean a single array

### Default Values
- Methods, not fields
```
public @interface Version {
    int value() default 1;
    String author();
    String license() default "MIT";
}
```

### Array Arguments
- Must be compile-time constant values
- can't use array type(new String[] {}), must be anonymous array ({})
- `@Version(environment={"prod","dev"})`
```
public @interface Version {
    String[] environments();
}
```

### Annotating Annotations
- Annotations for Java annotations
- annotations "targets" -control where annotations can be used
    * field
    * method
    * constructor
    * types (aka classes)
    * other annotations (annotation annotations)

```
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD})
public @interface Version {
    ...
}
```
- @Target annotation takes an array of type ElementType
- apply the @Target annotation above the annotation definition

### Retention
- @Retention
- apply @Retention above the custom annotation defintiion
- select policy using RetentionPolicy enum
    * CLASS
    * RUNTIME
    * SOURCE
- all other retention policies will cause the JVM to ignore the annotations

### Reflection
- `.getAnnotations()`
- `Annotation[] annots = engineer.getClass().getAnnotations()`
- if RetentionPolicy RUNTIME is set, our new custom annotation will appear in `getAnnotations()` above. Else, not.

### Repeatable Annotations
- add @Repeatable annotation to our custom annotation declaration
- @Repeatable needs a container
- create a new, second, custom annotation to serve as a container
- the new, second container annotation must share the same Retention Policy, Target, etc.

### Inherited Annotation
- @Inherited
- Adding an inherrited annotation to a person class means subclasses, such as Employee, inherit the annotation

### Pluggable Type System
- ANnotation processing
- Lombok is a library to analyze code
- Checker framwork - pluggable static analysis
- lombok @ToString annotation
- adding methods that are not explicitly created by programmer
- spotting bugs with @NonNull























