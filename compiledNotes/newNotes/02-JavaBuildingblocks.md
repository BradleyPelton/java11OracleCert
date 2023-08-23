# Chapter 2 - Java Building Blocks

### Constructors
- The name of the constructors must match the name of the class (case-sensitive)
- No return type
- Tricky exam questions: reusing class name as a method name but including a return type. Compiles fine.
- `(new Date()).Date()`

### Initializer Blocks
- defn: code block - code between braces
- defn: instance initializer - floating code blocks at the member level. 
- instance and static initializer blocks

### Order of initialization
- See chapter 8. Initializer blocks run before constructors always.

# Data Types
### Primitive Types
- Java has 8 built in data types, called primitives.
- All Java objects are just complex collection of primitive data types

### Primitive sizes
- (not required for exam, just relative size)
- boolean - (depends on VM)(small)
- byte    -  8-bit integral value           - [-128                       to 127]
- short   - 16-bit integral value           - [-32_768                    to 32,767]
- int     - 32-bit integral value           - [-2_147_483_648             to 2_147_483_647]
- long    - 64-bit integral value           - [-9_223_372_036_854_775_808 to 9_223_372_036_854_775_807]
- float   - 32-bit floating-point value     - [6 to 7 decimal digits]
- double  - 64-bit floating-point value     - [15 decimal digits]
- char    - 16-bit Unicode value            - [0                          to 65_535]      
- ASCII values range from 0 to 255, but char ranges from \u0000 (0) to \uffff (65,535)
- special relationship exists between `char` and `short` since they are both 16-bit integral values. `short` is signed but `char` is unsigned. No such thing as a negative char. 
- each numeric type uses twice as many bits as the smaller similar type
- All numeric types are "signed"(include negative/"parity") in Java
- Java stored floating-point values in scientific notation. `a x 10**b` up to # of decimal precision. Not accurate.
- Java allocates memory based on primitive type. (int -> 32 bits). primitive references aren't the same size (but obj are)

### Literals
- defn: literal - when a number is present in the code
- literals without a suffix are assumed to be int if number, double if decimal. 
- `long max = 3_123_456_789` won't compile because it exceeds int size. Adding `L` suffix fixes
- `byte b = 200;`        won't compile because it exceeds byte size.
- literal is int, but then down casted to left side without an explicit case if it fits
- Three bases
    1. Octal       - prefix = `0`           , example = `017`
    2. Hexadecimal - prefix = `0x` or `0X`  , example = `0xFF`   (NOTE: Hex is case-insensitive, [0-9, a-f/A-F])
    3. Binary      - prefix = `0b` or `0B`  , example = `0b1011100`

### Underscores
- Underscores are allowed anywhere except:
    1. Beginning/End of literal
    2. Before/After decimal
    3. Before/After prefix (`0b`)

### Reference Type
- defn: reference type - type that refers to an object
- references do not hold the value of the object
- references "point" to an object by storing the memory address where the object is located.
- defn: pointer - an address in memory
- An object in memory can only be accessed via a reference, not directly
- references can be assigned null, meaning they do not currently refer to an object
- primitive types can't be assigned null

### Variables
- defn: variable               - name for a piece of memory that stores data
- defn: variable declaration   - stating the variable type
- defn: variable initialization - giving a variable a value using assignment operator (=)
- variables are declared to be a certain type `int e;`
- variables are initialized to a certain value `e = 10;`
- variables can be declared and initialized in one line `int e = 10;`
- three types of variables: 1. local 2. instance 3. static

### Identifiers
- defn: identifier - name of a variable, method, class, interface, or package
- identifier charset = `[0-9, A-Z, a-z, _, $]`
- Identifier rules:
    1. Can't start with number 
    2. Can't be equal to `_`
    3. Can't be a Java reserved word (case-sensitive)

### Multi-declaration (Declaring Multiple Variables)
- Declaring multiple variables in one statement
- `int i, j, k = 10` // i j k all declared, but only k initialized
- Rules:
    1. Only one type allowed (duplicate/redundant types also not allowed!)
    2. Comma separated, semicolon end.
    3. Some values can be initialized, not required
    4. var not allowed
 
### Local Variables
- defn: local variable - a variable defined within a method/constructor/initializer_block
- local variables do not have a default value and don't have to be initialized ever
- local variables must be initialized before used
- "x might not have been initialized" compiler error thrown if used before initialized
- compiler is smart enough to know if initialized in "branching" (if, if else, else)

### Method/Constructor Parameters
- special type of local variables that have been pre-initialized

### Instance/Class Variables
- defn: instance variable - aka field, is a value defined within a specific instance of an object.
- defn:    class variable - static field, is a value defined on the class level and shared among all instances.
- instance and class variables do not have to be initialized
- instance and class variables are initialized to default values (except when final!)
- final instance/class variables must be initialized before constructor finishes or else compiler error
- default values = {boolean -> false, byte+short+int+long -> 0, float+double -> 0.0, char -> `\u0000`, obj -> null}

### Var
- defn: var = local variable type inference
- Golden Rule: var can only be used when the compiler can infer the type from the context.
- `var e = 0;` var infers raw literals as int or double
- Rules for Var
    1. var can't be used as instance/static fields, method/constructor params
    2. var must be declared and initialized in the same statement
    3. var type can't change (but value can)
    4. var can't be initialized to null (without a cast), but can be reassigned to null.
    5. var not allowed in multi-declaration
    6. var is not a reserved word, but it is a reserved type!(can't declare class/interface/enum with name = var)
    7. var in lambda parameter list only if all parameters are var type (no mixing)

### Scope
- local variables can never have a scope larger than the method they are defined in.

# Garbage Collection
- defn: heap - aka free store - represents a large pool of unused memory allocated to Java
- defn: garbage collection - the process of automatically freeing memory on the heap by deleting objects that are no longer reachable.
- eligible for garbage collection != garbage collected. JVM does what it wants
- `System.gc()` merely suggests that the JVM kicks off garbage collection. No guarantee
- Just before `OutOfMemoryError` is thrown, JVM will attempt to garbage collect.
- GarbageCollection is the process of deleting unused objects, NOT REFERENCES.
- Reference vs Object itself



---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
