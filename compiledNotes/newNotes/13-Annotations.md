# Chapter 13 - Annotations
- Annotations are all about metadata.
- The purpose of an annotation is to assign metadata attributes to classes, methods, variables, etc.
- @ syntax
- can contain attribute/value pairs called "elements"
- annotations function a lot like interfaces
- annotations establish relationships that make it easier to manage data about our application.
- annotations ascribes custom information on the declaration where it is defined.
- annotations are optional metadata and by themselves don't do anything.
- new keyword is never used to create an annotation, just an "annotation literal"(`@Exercise(hoursPerDay=2)`)

### Creating Custom Annotations
- @interface annotation to declare an annotation.
- Usually declared in their own file as a top-level type, but can be defined inside a class like inner classes.
- `public @interface Exercise{}`
- defn: Marker annotation - annotation that does not contain any elements, "zero arg"
- Parentheses are optional if no elements(marker annotations), but required for non marker annotations
- Annotations can be declared above or inline
- if an annotation is declared on a line by itself, then it applies to the next non-annotation type.
- Annotations are case sensitive
- Some annotations are lowercase, but PascalCase preferred

### Elements
- defn: annotation element - "annotation param" - an attribute that stores values about the particular usage of an annotation.
- `int hoursPerDay();`
- When defined in the annotation class, it looks like an abstract method although it's an "element"
- parentheses required in "annotation literal" if annotation element exist
- When more than one element value, comma separated.
- syntax: `elementName=value` (similar to a Map)
- order of each element does not matter
- compiler checks for incompatible types
- Valid Element types = {primitives, String, "Clazz"(not a class), enum, another annotation, or an array of one of these types}
- Element types can't be {Primitive Wrapper classes, String[][] 2d arrays, Object, etc.}
```
public @interface Exercise {
    int hoursPerDay();
}
@Exercise(hoursPerDay=3) public class Cheetah {}
```

### Optional Elements
- When declaring an annotation, any element without a default value is considered required.
- `int startHour() default 6;`
- Similar to case statement values, the default value of an annotation must be a non-null constant expression (compile time constant)
```
public @interface Exercise {
    int hoursPerDay();
    int startHour() default 6;
}
@Exercise(startHour=5, hoursPerDay=3) public class Cheetah {}
@Exercise(hoursPerDay=0) public class Sloth {}
```

### Element Modifiers
- annotation elements are implicitly abstract and public (like abstract interface methods)
- since abstract, can't be final
- since public, can't be protected or private.

### Constant variables
- Annotation can include constant variables
- can be accessed by other classes without creating the annotation. `@Test(timeout=TEST_TIMEOUT)`
- annotation variables are implicitly public, static, final (just like interface variables)
- constant variables are not considered annotation elements 
- Marker annotations can contain constants

### Annotation locations
- @Target annotation limits the types the annotation can be applied to.
- @Target takes a param of `ElementType[]`
- Annotations can be used to any Java declaration, including:
    * Cast Expressions (interesting!)
    * Classes, interfaces, enums, and modules
    * Variables (local, instance, static)
    * Methods and constructors
    * Method, constructor, and lambda parameters
    * Other annotations

### value() element (Keyless Element/Argument)
- shorthand notation for one argument annotation where the one argument is unambiguous
- `@Injured("Broken Tail")`
- annotation with a value without the elementName/"key"
- Rules
    * annotation declaration must include `value()` method with exact name(with either a default value or required)
    * No required elements (except optionally `value()`)
    * Only one param, the value() param, can be passed to usage("annotation literal")
- you can optionally specify the key name, namely `value="foot"`
- `Injured("Legs")`
- `Injured(value="Legs")`
- Typically, the value() of an annotation should be related to the name of the annotation

### Passing an array of values
- shorthand exists for singleton arrays
- If array type, and singleton array, braces are optional (e.g. testng `@Test(groups={"Regression"})`)
- Compiler provides the missing braces for you
- If multiple elements, compiler error (type checking)
- `@Music(genres={"Classical"})` === `@Music(genres="Classical")`
- NOTE `@Music(genres={})` is valid, `genres=null` or `genres=` are invalid
- Doesn't work for List or Collection since they are not supported element types
- value() shorthand can be combined with singleton array shorthand
- `@Music(value={"Swing"})` === `@Music("Swing")`

### Built-in annotations
- java.lang.annotation package is not imported automatically.
- @Target - limits the types the annotation can be applied to. Takes an array of ElementType enum value
    * `@Target({ElementType.METHOD, ElementType.CONSTRUCTOR})`
    * Default behavior doesn't allow lambda expression or generic declarations to be annotated. Must be overridden with @Target
    * `ElementType.TYPE_USE` can be used anywhere there is a Java type (90% of everywhere)
- @Retention - Annotations may be discarded by the compiler or at runtime. @Retention overrides the retention policy
    * `@Retention(RetentionPolicy.RUNTIME)`
- @Documented - Marker annotation that tells generated javadoc to include annotation information on Java Types.
- @Inherited - Marker annotation that forces subclasses to inherit the annotation information from parent class. This annotation is applied to other annotations, not to classes. 

### Duplicate annotations (@Repeatable)
- Without the @Repeatable annotation, an annotation can be applied only once
- Generally, you use repeatable annotations when you want to apply the same annotation with different values
- @Repeatable - used when you want to specify an annotation more than once on a type
- requires creating two annotations, "containing annotation" and "repeatable annotation"
- To declare a @Repeatable annotation , you must define a containing annotation type value
- A containing annotation type is a separate annotation that defines a `value()` array element. The type of this array is the type of the Annotation you want to repeat
- By convention, the name of the annotation is often the plural form of the repeatable annotation (Risk and Risks)
- Rules
    * Repeatable annotation must be declared with @Repeatable and contain a value that refers to the containing type annotation
    * containing type annotation must include an element named value(), which is a primitive array of the repeatable annotation type.

```
public @interface Risks {
    Risk[] value();
}
@Repeatable(Risks.class)
public @interface Risk {
    String danger();
    int level() default 1;
}
```

### Javadoc vs java annotation
- @param, @return, @author are all javadoc annotations included in the javadoc(/** */)
- javadoc annotations are usually all lowercase
- Java annotations usually start with an uppercase
- @deprecated (javadoc) vs @Deprecated (java)

### Common Annotations
- @Override - optional marker annotation to indicate a method is overriding another. 
- incorrect usage of @Override will throw compiler error
- @FunctionalInterface - optional marker annotation to state a Interface is a FunctionalInterface
- incorrect usage of @FunctionalInterface will throw compiler error. (more than one default method)
- @Deprecated - notates that a Java type is scheduled for deprecation
- optional arguments: `String since()` and `boolean forRemoval()`
- not to be confused with `@deprecated` which is a javadoc annotation. Best practice to use both.
- @SuppressWarnings - Tells the compiler to stop spamming logs with warnings. Apply to class, method, or type
- `@SuppressWarnings({"deprecation", "unchecked"})`
- Common suppressed warnings: deprecated methods, unchecked raw types (e.g. List)
- @SafeVarargs - Market annotation that indicates that a method does not perform any potential unsafe operations on its varargs parameter. Can only be applied to constructors and methods that can't be overridden (e.g. private, static, final)
- Example of unsafe operations: Generic array casting varargs, heap pollution, 








---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
