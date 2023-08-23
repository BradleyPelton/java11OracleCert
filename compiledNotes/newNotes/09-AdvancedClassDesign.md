# Chapter 9 - Advanced Class Design

# Abstract Classes
- defn: Abstract Class - a class that cannot be instantiated (and may have abstract methods).
- Abstract classes can include variables, methods, inner classes, and even constructors.
- defn: Abstract Method - a method that does not have a body.
- implementing an abstract method is a form of @Override
- Abstract classes are not required to have any abstract methods
- if abstract method exists, then class must be Abstract
- abstract keyword is mandatory when (not implicit) (empty body iff abstract keyword) (abstract keyword is implicit for interfaces, but not for abstract classes)
- defn: Concrete Class - a class that is non-abstract
- The first concrete subclass that extends an abstract class is required to implement all inherited abstract methods
- abstract classes can extend concrete classes and vice versa and abstract classes.

### Rules
- Abstract class constructors can call abstract methods
- Abstract methods can't provide an implementation
- Abstract classes can't be final
- Abstract methods can't be final
- Abstract methods can't be private
- Abstract methods can't be static
- Abstract methods must still follow @Override rules
- (NOTE: private + final is fine)

### Interfaces
- defn: Interfaces: an interface is an abstract data type that declares a list of abstract methods that any implementing class must implement.
- Interfaces can also include variables, but they must be constants. (public static final)
- abstract methods are implicitly public
- variables are implicitly public
- variables are implicitly static
- variables are implicitly final
- interfaces are implicitly abstract
- interfaces can't be final since implicitly abstract
- interfaces can extend multiple other interfaces
- interface methods w/o a body are implicitly abstract and final
- Interfaces extend, not implement, other interfaces
- Interfaces can extend multiple other interfaces at once
- Interfaces can be thought of as being like specialized abstract classes.

### Implicit modifiers
- implicit means the compiler will automatically add the modifier
- Okay to add implicit modifiers manually instead of letting compiler do it automatically
- Conflicting modifiers will throw compiler error

### default for interfaces
- Since methods are implicitly public, if omit the access modifier, it will be made public (not default!)
- lack of access modifier for classes = default
- lack of access modifier for interfaces = public

### Interfaces vs Abstract Classes
- Interfaces have implicit modifiers, Abstract Classes do not
- extends vs implements
- "abstract" keyword is required for Abstract Classes and Abstract methods in Abstract classes
- "abstract" keyword is optional for Interfaces and Interface methods(implicit)
- Abstract classes isn't forced into public implicit modifier for all methods

### Name Collisions with Interfaces
- If class implements two interfaces with
    * same signature -> fine. compiler understands
    * different signature -> fine. This is just overloading
    * same signature & different return type -> Depends. Must follow @Override rules. Covariant else compiler error

### Casting Interfaces
- Casting to interface is fine. 
- Compiler struggles with validating casts at compile time. 
- Compiler allows bad interface casts, but ClassCastException thrown a run-time
- The compiler does not allow a cast from an interface reference to an object reference if the object type is final and does not implement the interface. 
- The compiler can check for unrelated interfaces if the reference is a class that is marked final.
- instanceof   
    - The compiler will only report an unrelated type error for an instanceof operation with an interface on the right side if the reference on the left side is a final class that does not inherit the interface.

### Inner Classes
- inner aka nested class aka member inner class
- defn: A class defined at the member level of another class
- private inner interfaces are possible
- inner classes can have any access modifier(public/protected/default/private)
- No static members in inner classes
  



---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
