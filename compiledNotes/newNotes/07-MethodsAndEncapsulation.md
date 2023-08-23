# Chapter 7 - Methods and Encapsulation
### Methods
- defn: method declaration - specifics all the info need to call the method. Everything.
- defn: method signature   - method name + ordered parameter list
- access modifier vs optional specifier

### Access Modifiers
- public
- protected
- default
- private

### Optional Specifiers
- static
- abstract
- final
- synchronized
- native       (not on exam)
- strictfp     (not on exam)
- transient

### Order of method declaration
- return type                      HAS TO appear right before method name
- formal type parameter (Generics) HAS TO appear right before return type `<T> T`
- [access_mods + optional_specifiers] + type parameter + return_type + methodName(params) throws ExceptionList {}
- order of access modifiers and optional specifiers doesn't matter, but bad practice
    * `final public void run(){}` this is valid

### Return type
- return type is always required
- if void, return statement is optional in body. `return;`
- All branches of logic have to return a value. Compiler error if else.

### Method Names
- Method names are "identifiers" as well. 
- Method Names follow the same rules as identifiers. 

### Parameter list
- comma separated
- No multi-declaration
- No var

## Exception list
- comma separated 
- only one Throws
- Don't have to be exclusive at all (Intellij grays it out as unnecessary)
- You can `throws` Checked or unChecked, but throws unchecked is pointless
- `throws IOException, FileNotFoundException, TimeoutException, IllegalStateException`

### Varargs
- must be last parameter
- only one vararg allowed (since it must be last)
- varargs can be passed as array (new int[]{1,2,3}) or as values (1, 2, 3) (e.g. `Stream.of(V...)` )
- vararg param can be left out entirely. sending two params instead of 3. Java will send empty array
- null can be passed as vararg value

### Access modifiers
- public vs protected vs default vs private
- access modifiers for class determine where the class reference can be used
- access modifiers for methods determine where the method can be used, regardless of the reference.
- protected edge cases:
    * Even if a child class inherits, if you reference a protected method from a parent reference, no access allowed.

### Static keyword
- (Static Context) A static member cannot call an instance member (without referencing an instance of the class)
- Tricksy: The compiler checks the type of the reference and uses that instead of the object.
- instance.staticMethod() - instance of an object to call a static member.
- ((Koala) null).staticMethod() - compiles fine.

### Misc modifier/specifier
- final != immutable
- final instance/static fields must always be initialized by the time the constructor finishes.
- final instance/static fields must be set "exactly once" (not more, not less)
- final local variables don't need to be initialized if never used.
- In general, avoid static&instance initializers, just use constructors
- defn: static import - used to import static members (as opposed to classes)
- `import static java.util.Arrays.asList;`

### Passing Data among Methods
- defn pass-by-value - a copy of the variable is made and the method receives the copy.

### Overloaded
- defn: Overloaded - methods have the same name but different signature
- Overloaded methods have very few rules. Same method name, different parameters.
- Edge case:
    * Java treats varargs as an array, so the signature is the same
    * void fly(int[] e)
    * void fly(int... e) // compiler error
- Edge case:
    * void fly(int e)
    * void fly(Integer e) // fine, promotion/boxing wont ever be called.
- Edge case:
    * `void walk(List<String> strings) {}`
    * `void walk(List<Integers> integs) {}` // Type erasure with generics make these two the same signature

### Method resolution
1. Exact
2. Widening
3. Autoboxing
4. Varargs
- NOTE: No combinations, only one conversion





---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
