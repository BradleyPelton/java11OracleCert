# Second Half of Book

# Chapter 12 - Java Fundamentals
### Final Modifier
- final can be applied  to variables, methods and classes
- final local variables can be declared and never initialized (unlike final instance/static variables)
- final != immutable. final objects can still be modified with method calls
- final classes cannot be extended

# Enum
- defn: enumeration - top level type fixed set of constants
- enum values are considered constants and should be in SCREAMING_SNAKE_CASE
- list of enums values is required to be declared first
- list of enums are comma separated with a semicolon after the last enum.
- semicolon is optional if only enum constants, but if enum contains fields/constructors/methods, semicolon is required
- enums can be compared using == or .equals() because there is only one created by the JVM
- .values() method provided. returns an array of all of the values, e.g.  returns `Season[]`
- .valueOf() method provided. Factory method turning String -> Enum
- .valueOf() must match the enum value exactly, case sensitive. runtime exception IllegalArgumentException thrown.
- enums can't be extended
- enums can implement interfaces however
- enum is a type, not a primitive

```
public enum Season {
    WINTER, SPRING, SUMMER, FALL
}
```

### Enum switch statements 
- enums can be used in switch statements. 
- The case statement doesn't compile if you use the redundant `Season.FALL` instead of just `FALL`

### Enum constructors, fields, and methods
- instance variables of enum can be mutable, but bad practice. Usually fields should be final
- All enum constructors are implicitly private.
- This makes sense since you can't extend an enum and you can't call constructors of enums. 
- The first time the enum class is loaded, Java constructs all of the enums. 

### Enum methods
- enums can have simple methods that apply to all enum values
- enums can have abstract methods that are overridden by each enum
- "Creating a bunch of tiny subclasses"
```
public enum Season {
    WINTER {
        public String getHours() { return "10am-3pm"; }
    },
    SUMMER {
        public String getHours() { return "9am-7pm"; }
    },
    SPRING,FALL {
        public String getHours() { return "9am-5pm"; }
    },
    public abstract getHours();
}
```

# Nested Classes
- defn: Nested class - a class that is defined within another class.
- Four types of nested classes:
    * Inner Class - A non-static type defined at the member level
    * Static nested class - A static type defined at the member level
    * Local class - A class defined within a method body
    * Anonymous class - A special case of a local class that does not have a name
- Nested classes help enforce encapsulation better. Although, they make the code more tightly coupled.
- interfaces and enums can be declared as both inner classes and static nested classes, but not as local or anonymous.
- It's possible to have inner classes within inner classes (bad practice)

# Inner classes
- defn: Inner Class - A non-static type defined at the member level
- aka member inner class
- An inner object is associated with a specific outer object.
- properties
    * any access modifier (just like any member)
    * can extend any class and implement any interface
    * can be abstract or final
    * cannot declare static methods
    * all static fields must be static final
    * can access members of the outer class, including private members
- An inner class declaration looks like a stand-alone class declaration.
- Since an inner class is not static, it has to be used with an instance of the outer class. 
- Multiple class files are created at compile time . Outer.class , Outer$Inner.class . ($ syntax not required on exam)
- Inner classes can have the same variable names as outer classes, complicating scope.
- static members of the outer class cannot instantiate the inner class without first instantiating the outer. The inner instance has to be associated with an instance of the outer. 

### Instantiating inner classes
- A method of the outer class can instantiate the inner class
- An inner object must be associated with a specific outer object.
- We can't just call `new Inner()` because Java won't know which instance of Outer it is associated with.
```
Outer outer = new Outer();
Inner inner = outer.new Inner()  // "new Inner()" is being used like a method name
```

### Scope
- Inner classes can have the same variable names as outer classes, complicating scope.
- pre-qualifying "this" with a class reference. `Outer.this.x`
- `Outer.this.x` // inner vs `this.x` // depends on where

# Static Nested Class
- defn: Static nested class - A static type defined at the member level
- Unlike inner classes, static nested classes can be instantiated without an instance of the enclosing class. 
- Unlike inner classes, it can't access instance variables or methods from the outer class directly (but indirectly, by creating a dummy instance, yes)
- properties:
    * creates a namespace (like a new package `outer.inner.add`)
    * can be made private 
    * Outer class can refer to the fields and methods of the static nested class
- You can instantiate a static nested class using `Outer.Inner inner = new Outer.Inner()`
- You can import static nested classes using regular imports or static imports
    - import bird.Outer.Inner;
    - import static bird.Outer.Inner;

# Local Class
- Local class - A class defined within a method body
- Like local variables, local class declaration does not exist until the method is invoked
- Like local variables, local classes go out of scope when the method returns
- You can return instances of the local class as the return type of the method
- Local classes can be declared inside methods, constructors, and initializers
- properties:
    * No access modifiers
    * can't be static
    * can't declare static methods
    * static fields must be final
    * Full access to outer class fields and methods(*when defined in an instance method, otherwise static context)
    * Can access local variables iff final or effectively final
    * Only allowed to extend or implement one ??? property??? research
- a new .class file is generated for local classes.
- Local classes share the same rules as lambdas because lambdas are effectively local classes

# Anonymous Class
- defn: Anonymous class - A special case of a local class that does not have a name
- declared and instantiated in the same one statement
- uses the new keyword, looks like calling constructor but with extra braces
- a type name
- braces
- ends with semicolon ***
- Since Anonymous classes are local classes, they can only access local variables iff final or effectively final
- Anonymous classes are required to extend an existing class or implement an existing interface
- Best used when you have a short implementation that will not be used anywhere else (e.g. sorting)
- You can "extend" abstract classes and interfaces with anonymous classes. The anonymous class provides the implementation of any abstract methods. The reference type will be the abstract class or interface you are implementing.
- Anonymous class is just an unnamed local class. 
- Anonymous classes can be defined where they are needed, even if that is an argument to another method (e.g. PQ sort)
- You can define anonymous classes outside a method body. Tricky example:
```
public class Gorilla {
    interface Climb {}                  // nested interface
    Climb climbing = new Climb() {};    // import to remember semi-colon     
    // braces makes this an anonymous class implementing the Climb interface
}
```

---------------------------------------------------------------------------------------------------

# Interfaces
### Interface members
- 6 types of interface members
    1. Constant Variables (public static final)
    2. Abstract method   (public abstract)
    3. Default method    (public default)
    4. static method     (public) 
    5. private method    (private)
    6. private static    (private static)

### default method
- defn: default method - a default method in an interface is a method with a method body.
- "default implementation"
- default methods are treated as part of the instance, can't be used like static methods 
- default methods must have the modifier "default". Not implicitly added
- May be overridden by a class implementing the interface, may not.
- not to be confused with the default keyword in switch statements
- not to be confused with the package-private access modifier that added if no access modifier

### Rules for default interface methods
1. Only in interfaces
2. Must be marked "default" and have body
3. Implicitly public
4. Can't be abstract, final, or static
5. May be overridden by a class that implements the interface
6. If a class inherits two default methods with the same signature from different interfaces, then the class must override the method

### Overridden Default Methods
- You can access overridden default methods by calling `Myinterface.super.defaultMethod()`

### Static interface methods
- defined explicitly with the static keyword
- behave almost identically to static class methods
- Static interface methods are not inherited (this solves the diamond problem)
- called just like static class methods but with Interface name first, `MyInteface.staticMethod()`
- Since static methods are not inherited, there are no collisions with same method signatures from different Interfaces.
- Rules
1. must be marked "static"
2. must include a body
3. Either public or private
4. cannot be abstract or final
5. Static methods are not inherited and cannot be accessed in a class implementing the interface without a reference to the interface name.

### Private Interface methods
- Not inherited
- mainly used to reduce code duplication inside of interface methods
- private vs private static 
- private methods can't be referenced from static interface methods (static context)
- private interface methods behave a lot like instance methods within a class.
- Rules
    * Must be marked "private"
    * Must include a method body
    * Can only be called by default and private(non-static) methods within the interface.

### Private Static Interface Methods
- mainly used to reduce code duplication
- Rules:
1. must be marked "private" and "static"
2. Must include a method body
3. May only be called by other methods within the interface.

---------------------------------------------------------------------------------------------------

# Functional Programming
(bunch of repeated stuff from chapter 6)
- Functional Interfaces are used as the basis for lambda expressions
- Any functional interface can be implemented as a lambda expression
- defn: functional interface: - an interface that contains a single abstract method (SAM rule).
- defn: lambda expression: a block of code that gets passed around, sort of like an anonymous class that defines one method
- Weird edge case: if a functional interface includes an abstract method with the same signature as Object.toString or Object.equals() or Object.hashCode(), the interface is not considered a functional interface. 
- behind the scenes, anonymous classes are used for lambda expressions (hence they share the final/effectively final rule)










---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
