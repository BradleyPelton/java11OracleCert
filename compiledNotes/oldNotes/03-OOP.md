

#
Defn: OOP - 


### 4 Pillars of OOP
- APIE
- Abstraction
- Polymorphism
- Inheritance
- Encapuslation

- this() calls constructor inside of other constructors
- this() must appear on the first line of constructors
- super()

- constructors can be private. 

### Instance Initializer 
- runs before constructor 
- random floading block

### Static Initializer 
- multiple static blocks can exist
- run order

### GC Roots
- garbage collector roots. Find unused references


### Package
defn: Package - a namespace that organizes a group of related classes or interfaces

- package names are always lowercase
- reverse domain = com.pluralsight.courses

# Field Shadowing
defn:

#
- Pass by reference vs pass by value
- passing primitives and objects are different
- passing the address in memory to the object
- Java is pass-by-value


# Access control
- Two levels of access control
    * class level
    * member level

- public
- protected
- default (package-private)
- private


# Encapsulation
- Hides info
- Hides inner workings
- fields should almost always be private
- getters and setters should be public

# Inheritance
- defn:
- subclasses don't have access to private members of parent
- Invoke parent constructors with super()


# Overriding
- Defn: method overriding

# Overloading
- defn: method overloading

# Polymorphism
- defn: polymorphism - The version of the method used is determined at runtime
- final methods can't be overridden
- super.methodName() to call parent method
- a common reference type
- polymorphism allows us to change behavior at runtime
- polymorphism reduces coupling because we can depend on abstractions, not concrete types
- we can use a single variable type to store many types

# Abstract class
- Defn: Abstract Class - A Class that can't be instantiated. Must use "abstract" optional specifiers
- Abstract classes can have both abstract and concrete methods

# Abstract Method
- Defn: 
- has to be implemented(overridden) by the first concrete class

### interface vs abstract classes
### Use interfaces when:
- highest level of abstraction
- highest level of loose coupling
- you need to inherit/extend multiple parents

### Use abstract classes when:
- you want to offer some base functionality to subclasses
- provide a template for future classes
- create abstract methods that are not public


# Enum
- defn: Enum is a data type that enables a variable to hold only certain predefined values
- a fixed set of constants
- enum tyoe can have a body which can include fields and methods
- enums are much more powerful than at first glancve
- The compiler also adds some special static methods like values() or valuesOf()
- An enum type can have properties assigned to each constant value
- An enum type can have a constructor, which can be used to assign properties to enum constants
- constants must be declared at the top of the enum body
- everything else must be declared below
- the constructor of the enum type must be private
- you can not call the enum type constructor directly
- enums support == comparison
- switch statements support enum case values

### Example:
public enum FlightRules {
    INSTRUMENT_RULES(10),
    VISUAL_RULES(20),
    SPECIAL_RULES(15);

    private int minSeparation;

    FlightRules(int minSeparation) {
        this.minSeparation = minSeparation;
    }

    public int getMinSeparation() {
        return minSeparation;
    }
}

# Inner Classes
- In Java there are two types of nested classes
    * static nested classes
    * inner classes

- defn: static nested class
- defn: Inner class -  a class associated with an instance of its outer class

characteristics
- has direct access to outer class object's fields and methods
- it can't contain static members of the enclosing class (since it is associated with an instance)
- to instantiate an inner clkass, you must first instantiate the outer class

#
- two special types of Inner Classes
    * Local classes
    * Anonymous classes

# Local Class
- defn: local class -  a class defined within a block of code, usually within a method
- can be created in 1. a method 2. a for loop 3. if clause
- local class can still access enclosing class methods and variables
- local classes can only be instantiated within the sameblock where they were declared
- just like lambdas, local classes can only access local variables that are final or effectively final
- local classes can access method parameters if it is declared inside a method
- local classes can't contain any static members, except  constants (final static fields or primitive types or strings)
- you can't declare interfaces in a block, just classes
- local classes do not have access modifiers since they are only defined within a block and used within the same block (there is no need for access control)

# Anonymous Classes
- defn:
- Anonymous classes are treated as expressions
- An anonymous class can access all the instance members of its enclosing class
- can access local variables, but they must be final/effectively final
- can access method params if it is defined within a method
- Anonymous classes can't have a constrcutor

### Example












