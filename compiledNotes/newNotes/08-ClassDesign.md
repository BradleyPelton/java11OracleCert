# Chapter 8 - Class Design

### Inheritance
- Defn: Inheritance - the process by which a subclass automatically includes any public or protected members of the parent
- Inheritance is transitive
- private members are never inherited.
- Java supports single inheritance, not multiple inheritance(diamond problem)
- Java supports multiple-levels of inheritance (class chains)
- Cosmic SuperClass - All classes inherit from Object

### Class modifiers
- Two access modifiers available for Top-Level types (classes, interface, enum)
    * public
    * default
- Two optional specifiers for Top Level types
    * final
    * abstract
    * strictfp(not on the exam)
    * native (not on the exam)

### this
- defn: this - refers to the current instance of the class.
- this() calls another constructor of the same class
- this() must be the first statement in the constructor (excluding comments)
- by effect, there can only be one this() call per constructor
- this() can't cause an infinite recursion. Compiler error.
- `this` can't be accessed from a static context

### this vs this()
- this refers to the current instance of a class
- this() refers to a constructor of the current class

### super
- super is a reference to the parent class instance(not class!)
- Only members defined at the parent level are accessible via super
- `super` can't be accessed from a static context
- you can't call super to refer to an interface that this class implements.

### super()
- calls parent class constructor
- must be first line of constructor if used
- super() always refers to the most direct parent (one level up)

### super vs super()
- super is used to reference parent class members
- super() calls parent constructor

### Constructors
- Defn: Constructor - a special method with the same name as the class but has no return type.
- constructors can be private
- no var params 
- constructor overloading - multiple constructors with different signatures.
- The first line of every constructor is either
    1. explicit call to this() 
    2. explicit call to super() 
    3. implicit call to super() // compiler enhancement

### Default no-arg Constructor
- `public Rabbit() {}`
- Every class has a constructor. If no constructor is coded, default no-arg constructor is generated at compile time.
- default construct - aka default no-arg constructors
- default no arg constructors only created if no other constructors exist
- if Parent class has no no-arg constructor, all child classes must have constructors. Compiler can't create default no-arg since super() isn't defined.

### Compiler Enhancement
- Java compiler automatically inserts a call to the no arg super() if you don't call this() or super() yourself
- creates default no-arg constructor if you don't create any constructors
- If a parent does not define a no-arg constructor but does define 1 arg constructor, child class can't use default no-arg constructor since it would implicitly call super() which doesn't exist.

### Constructors and final Fields
- local final variables don't have to be assigned. Not defaulted either.
- final instance/static variables MUST be assigned a value exactly once. Not defaulted.
- After initializer and constructors finish, final instance/static fields must have been assigned a value.

### Order of initialization
- NOTE: A class may never be loaded if it is not used
- Class is initialized at most once, right before it is used.
- Initializing a class is different than initializing an instance
- Order:
    1. Recursively initialize parent class
    2. Static variable and static initializers in order
    3. Instance variables and instance initializers in order
    4. Constructor in top-down order (resolving stack naturally)
- Edge case: 
    - The class containing main is initialized before the main method is executed just like any other method
     
### Overriding
- Defn : Overriding - when a subclass declares a new implementation for an inherited method with the same signature.
- this and super allow you to select between the current and parent versions of a method.
- rules:
    1. same signature
    2. access modifier same or less restrictive
    3. same or more specific checked exception
    4. covariant return type
- covariant = same or subtype
- covariant iff you can assign the type from smaller to larger type
- Generic overriding
    * signature must be exactly the same, including the generic type
    * covariant return type
- You can't override a private method since private methods aren't inherited. It's just a new method.

### Hiding Methods
- defn: hidden method = when a child class defines a static method with the same name and signature as an inherited static method defined in a parent class.
- Same 4 override rules apply.
- new rule: child static iff parent static

### Hiding Variables
- defn: hidden variable = when a child class defines a variable with the same name as an inherited variable.
- No such thing as variable overriding
- Two distinct copies of the variable are created.
- hiding a variable replaces the member only if a child reference is used
- The type of the reference determines which variable is used.

### Polymorphism
- Defn: Polymorphism - A Java object may be accessed using a reference that is same or supertype (without a case)
- a Java object may be accessed using a reference of 
    * same type
    * super class
    * interface
- Polymorphic references don't create additional objects. Same object, just different references.
- Only the methods and variables available to the reference type are callable, regardless of the underlying object
- The type of the object determines which properties EXIST
- The type of the reference determines which properties are accessible.
- Overriding a method replaces the parent method on all reference variables
- hiding a method or variable replaces the member ONLY if a child reference type is used.

### Hiding vs Polymorphism 
- static method hiding is like the opposite of polymorphism 
    * parent static -> child static = hiding
    * parent static -> child instance = compiler error
    * parent instance -> child static = compiler error
    * parent instance -> child instance = overriding 

### Variables
- Variables can't be overridden
- Variables can be hidden though
- Child classes can hide parent instance variables with child static variables
- Child classes can hide parent static variables with child instance variables
- defn: Hidden Variable - Child class defined variables with the same name as inherited variable.
- When hiding, two distinct copies of the variable exist
- The type of the reference variable determines which variable is used

### Explicit vs Implicit Cast
- `Employee frank = new Boss(); // Implicit Cast`
- `Boss jill = (Boss) empObject; // Explicit Cast`
- If the current reference is a subtype, you do not need a cast operator.
- defn: cast conversion - implicit cast to a subtype
- Rules
    1. Casting a reference from a child to a parent doesn't require an explicit cast
    2. Casting a reference from a parent to a child does require an explicit cast
    3. The compiler disallows casts to an unrelated type
    4. The compiler can't check for interface casts at compile-time (unless the class is final)

### Polymorphism vs hiding
- Polymorphism states that when you override a method, you replace all calls to it, even those defined in the parent class
- unlike method overriding, method hiding is very sensitive to the reference type and location where the member is being used.
- changing the reference type may grant access to new members, but the rule of polymorphism states that it doesn't change the logic of existing members





---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------

