# Chapter 14 - Generics and Collections
- Review of Chapter 5 as well as more in depth

### Method References
- `System.out::println`
- `::` operator tells Java to call the println method later.
- `::` is like a lambda because it is used for deferred execution with functional interfaces
- A method reference can look the same but behave differently based on the surrounding context.
- If a method is overloaded, both lambdas and method references infer which version of a method to use from the context.
- If no methods or multiple methods match from context, compiler error thrown (ambiguous type error)
- Java knows to pass the parameter into the method.
- You can pretend the compiler turns your method references into lambdas for you
- Four formats:
    1. Static methods
    2. Instance methods on a particular instance
    3. Instance methods on a parameter to be determined at runtime
    4. Constructors

### Calling Static Methods
- Collections::sort
- ClassName::staticMethodName

### Calling Instance Methods on a Particular Object
- s::startsWith
- variableName::methodName

### Calling Instance Methods on a parameter
- String::isEmpty;
- ClassName::instanceMethodName

### Constructors References
- Constructor reference is a special type of method reference that uses new instead of a method name. 
- It is common for a constructor reference to use a Supplier reference
- ArrayList::new
- ClassName::new
- `Functional<Integer, List<String>> methodRef = ArrayList::new`  // Instantiate ArrayList with capacity integer param.
- Inferring the type of the constructor from the context

---------------------------------------------------------------------------------------------------

# Wrapper Classes (review)
- defn: autoboxing - compiler automatically converts primitive to wrapper
- defn: unboxing   - compiler automatically converts wrapper to primitive
- `valueOf()` vs `parseInt()`
- wrappers can be null, but attempting to unbox a null reference throws NPE
- autounboxing isn't a word. It's autoboxing and unboxing. 

# Diamond Operator
- defn: diamond operator - shorthand notation that allows you to omit the generic type from the right side of a statement when the type can be inferred from the left side
- Can only be used on the right side of an assignment operator. Fails to compile altogether, not just raw type.
- `<>` looks like a diamond
- `List<String> = new ArrayList<>();`
- Var edge cases:
    * `var list = new ArrayList<Integer>();` // `list instanceof ArrayList<Integer>`
    * `var list = new ArrayList<>();`        // `list instanceof ArrayList<Object>`
    * var can be used with generics, but the type defaults to Object if not specified. 

# Lists, Sets, Maps, and Queues
- defn: collection - a group of objects contained in a single Object.
- The Java Collections Framework is a set of classes in java.util for storing collections.
- Four main Interfaces in Java Collections Framework:
    * List  - ordered collection of elements accessible by index
    * Set   - unordered collection that does't allow duplicates
    * Map   - collection that maps keys->values with no duplicate keys allowed
    * Queue - collection that orders elements in a specific order for processing. 
- List, Queue, Set instanceof Collection, but not Map
- Map is considered part of the Java Collection Framework but not a sub-interface of Collection.

### Common Collections Methods
- Many of the common collections methods are convenience methods that could be implemented in other ways.
- .add()      - inerts a new element into the collection and returns whether it was successful
- adding to a list always returns true, but adding to a set may return false.
- remove()    - removes a single matching value and returns true if the element was found and removed.
- notably only removes a single value, not all values.
- remove() can also be called with an index. If index is too larger, `IndexOutOfBoundsException`
- NOTE: ConcurrenceModificationException if iterating and removing
- .isEmpty() and .size()
- .clear()    - discard all elements of the collection. void return type.
- .contains() - returns true if a certain value is in the collection.
- .contains() method calls .equals() on each element in the collection
- .removeIf() - `Predicate<? super E>` that removes an element if condition is true. 
- .forEach()  - `Consumer<? super T>` does something with each value and doesn't return anything, void.
- (ArrayList only) .replaceAll()  - `UnaryOperator<E> operator` replace each value with the output (e.g. x -> x*2)

---------------------------------------------------------------------------------------------------

# List Interface
- ordered (can access by index), can contain duplicates
- Unlike an array, many List implementations can change in size after they are declared
- ArrayList is a resizable array. Constant time lookup by index. Adding or removing an element is slow.
- ArrayList is good for reading when you know index. Bad for writing large amounts of data. 
- LinkedList is a special list because it implements both List and Queue.
- LinkedList allows constant time add/removing from the beginning and end of the LinkedList.
- LinkedList is bad at index. Linear time to access element by index. 

### Factory Methods of Lists
- Arrays.asList  -> returns fixed size list backed by an array. You can replace, but not delete or add. 
- List.of        -> returns an immutable list. You can't add, delete, or replace.
- List.copyOf    -> returns an immutable copy. You can't add, delete, or replace.
- ArrayList::new -> Solution. Constructor with one argument that takes the other Collection and uses it as a template.

### Extra list methods
- add(index, element)
- get(index)
- remove(index)
- replaceAll(UnaryOperator) - nums.replaceAll(x -> x*2)
- set(index, element)
- .iterator()

# Set Interface
- HashSet stores elements in a hash table.
- The keys are a hash and the values are an Object. .hashCode()
- adding and checking if contains element are both constant time
- TreeSet stores its elements in order
- TreeSet uses a heap 
- TreeSet is much slower at adding and checking if contains element. 
- TreeSet does not allow null values

### Set Methods
- .add() returns false if element already exists in set
- Set.of() - Immutable set
- Set.copyOf() - Immutable clone
- (other Collection methods)

### Queue Interface
- Queues are used when order is important
- Unless stated otherwise, a queue is FIFO (first-in, first-out)
- Some queue implementations change this to use LIFO(Last-in, first-out)
- A stack is LIFO. A stack still implements the Queue interface, but LIFO vs FIFO.
- Stack is older code. Use LinkedList instead
- LinkedList is a double ended queue that is also a List
- defn: double-ended queue - you can insert and remove elements from both the front and the back of the queue
- LinkedList is not as efficient as "pure" queues.

### Queue Methods
- add()     - Adds an element to the back of the queue and returns true or throws and exception 
- element() - Returns next element or throws an exception if empty queue
- offer()   - Adds an element to the back of the queue and returns true if successful
- remove()  - Removes and returns next element or throws exception if empty
- poll()    - Removes and returns next element or returns null if empty
- peek()    - Returns(but don't remove) the next element or returns null if empty
- Basically two sets of methods
    * One set throws an exception when something goes wrong --- (add, remove, element)
    * The other set returns null                            --- (offer, poll, peek)
- offer/poll/peek are more common. 

### Map Interface
- Map has a static interface inside the Map class called Entry
- Map.Entry provides two methods Map.Entry::getKey and Map.Entry::getValue
- Map.of     -
- Map.copyOf -
- Map.ofEntries(Map.entry...)
- HashMap stores the keys in a hash table. (using .hashCode() method). Adding and getting elements both are constant time
- TreeMap stores the keys in a order using a heap. Adding and getting elements takes longer time
- TreeMap does not allow null values

### Map Methods
- boolean    .containsKey    - returns true if key   in keySet
- boolean    .containsValue  - returns true if value in values
- Set        .entrySet - returns keyValue pairs      as a `Set<Map.Entry<K,V>> `
- Set        .keySet   - returns keys                as a `Set<K>`
- Collection .values   - returns values              as a `Collection<V>`
- int        .size     - returns the number of key/value pairs (.entrySet().size())
- boolean    .isEmpty  - returns true if the size == 0
- V          .get          - Returns the value mapped by key OR NULL 
- V          .getOrDefault - Returns the value mapped by key or DEFAULT value provided
- V          .put          - Adds or replaces key/value pair. Returns previous value or null.
- V          .putIfAbsent  - sets a value but skips if the value is already set to a non-null value.
- V          .remove       - Removes and returns value mapped to key. Returns null if key doesn't exist
- void       .clear        - Removes all keys and values from the map
- V          .replace   - map.replace(2,10) // replace(k, v)
- void       .replaceAll - map.replaceAll((k,v) -> (k, v*2))
- void       .forEach
- V          .merge

---------------------------------------------------------------------------------------------------

# Sorting Data
- numbers < ALPHA < alpha  - (ASCII table order)
- Collections.sort() returns void, in place sorting
- Comparable - Interface that defines .compareTo()
- If you class implements Comparable, it can be used in these data structures that allow sorting.
- Comparator - class that is used to specify that you want to use a different order than the object provides by default
- "natural order" vs "custom order"

# Comparable
- java.lang (automatically imported)
- Comparable Interface has only one method, `int compareTo(T o)`
- Comparable is an Functional interface since the only method is an abstract method
```
public class Duck implements Comparable<Duck> {
    public int compareTo(Duck d) {
        return name.compareTo(d.name);
    }
}
Collections.sort(ducks)
```
- Any object can be comparable. 
- Three rules for compareTo()
    1. Zero when equal
    2. Negative when object is smaller than the parameter in compareTo
    2. Positive when object is greater than the parameter in compareTo
- .compareTo() and .equals need to be consistent on when two objects are equal else collections may misbehave. 
- If Comparable is implemented with raw type, then `compareTo(Object obj)` method takes Object.
- If Comparable is implemented with raw type, then cast is needed in the implementation of compareTo method
- Be careful of null comparisons: myObj.compareTo(null)

# Comparator
- java.util.Comparator (different package than Comparable, not automatically imported)
- We can define a different way than the existing way by creating a Comparator
- Comparator is a functional interface since there is only one abstract method. 
- `Comparator<Duck> byWeight = Comparator.comparing(Duck::getWeight);`
- `Comparator::comparing` is a static factory interface method that creates a Comparator given a lambda or method reference.

### Comparable vs Comparator
- They are both functional interfaces
- The point of Comparable is to implement it inside the object being compared
- The point of Comparator is to be passed as a lambda for custom sorting on demand
- Different packages (java.lang vs java.util)
- Different method names : Comparable::compareTo vs Comparator::compare
- Comparable::compareTo takes 1 argument, Comparator::compare takes 2 arguments

### Comparing Multiple Fields (/tie breakers)
- `.thenComparing()` for tie breakers
- `Comparator<Squirrel> c = Comparator.comparing(Squirrel::getSpecies).thenComparingInt(Squirrel::getWeight)`
- `var c = Comparator.comparing(Squirrel::getSpecies).reversed()`

### Comparator methods
- static methods for building a Comparator
- comparing()
- comparingDouble
- comparingInt
- comparingLong
- naturalOrder
- reverseOrder
- reversed
- thenComparing
- thenComparingDouble
- thenComparingInt
- thenComparingLong

### Sorting and Searching
- Collections.sort method uses the compareTo() method to sort
- Collections.sort() expects the collection passed as an argument to implement the Comparable interface
- `Collections.sort(Comparable<T>)`                 - if collection implements the Comparable interface
- `Collections.sort(Collection<T>, Comparator<T>)`   - if not, then pass a Comparator lambda/method reference
- Compiler will throw error if collection passed that doesn't implement Comparable and no second argument.
- .sort() and .binarySearch() methods allow you to pass in a Comparator object when you don't want to use the natural order.
- three param binary search `Collections.binarySearch(names, "Hoppy", myComparator)` //binary search in descending order.

---------------------------------------------------------------------------------------------------

# Generics
- Generics(Parameterized types) vs Raw Types
- Raw Types are a nightmare to deal with. Error prone.
- Generics are rarely used in custom classes you write, but they are everywhere in Java core packages, such as Java Collections Framework.
- Reifiable types aren't on the exam

### Generic Classes
- defn: Generic class is a class that declares a formal type parameter in angle brackets.
- The generic type T is available anywhere within the Crate class.
- When you instantiate the class, you tell the compiler what T should be for each particular instance
- `Crate<Integer>` vs `Crate<String>` vs `Crate<Date>`
```
public class Crate<T> {
    private T contents;
    public T emptyCrate() {
        return contents;
    }
    public void packCrate(T contents) {
        this.contents = contents
    }
}
```

### Naming Conventions for Generics
- A type parameter can be named anything.
- The convention is to use single uppercase letters to make it obvious that they aren't real class names
    * E for element
    * K for map key
    * V for map value
    * N for a number
    * T for a generic data type
    * S,U,V,... for multiple generic types

### Type Erasure
- Generic types help enforce rules at compile time, but not a runtime
- At runtime, the compiler replaces all references to `T` with `Object`
- This is called Type Erasure
- Type Erasure allows your code to be compatible with older versions of Java that don't have generics.
- The compiler adds the relevant casts from `Object` -> `T` 

### Generic Interfaces
- Just like classes, interfaces can declare a formal type parameter
``` 
public interface Shippable<T> {
    void ship(T t)
}
```
- If a class implements a generic interface:
    1. The class specifies the generic type in the Class declaration
        `class ShippableRobotCrate implements Shippable<Robot> {}`
    2. The class and the interface share a generic type
        `class ShippableAbstractCrate<U> implements Shippable<U> {}`
    3. The class uses raw types

### Raw Types
- This is the old way of writing code before generics. 
- Generates compiler warnings (which can be suppressed with `@SuppressWarning("unchecked")`)

### Generic Methods
- Methods also allow formal type parameters
- Method and class formal parameter types are entirely independent if both declare a formal type parameter, even if the same letter/name `<T>`.
-  If same letter without `<>`, it's not a formal type parameter being declared.
- This is often useful for static methods since they aren't part of an instance.
- Before the return type, we declare the formal parameter of `<T>`
- Unless a method is obtaining the generic formal type parameter from the class/interface, it is specified immediately before the return type of the method.
```
public class Handler {
    public static <T> void prepare (T t) {
        System.out.println("preparing " + t);
    }
    public static <T> T doNothing(T t) {return t;}
}
```
- You can specify the type explicitly on method calls with this weird syntax. Otherwise, compile will try to infer.
- `Box.<String>ship("Package")`

### Bounded Generic Types
- defn: bounded parameter type - a generic type that specifies a bound for the generic.
- defn: wildcard generic type - an unknown generic type represented with a `?`
- Bounded Generics are only allowed on the left side of an assignment operator
- You can use generic wildcards in three ways:
    1. Unbounded `?`
    2. Wildcard with an upper bound `? extends type`
    3. Wildcard with a lower bound  `? super type`

### Unbounded Wildcards
- An unbounded wildcard,`?`, represents any data type.
- Unbounded generics are immutable 
- A compiler error thrown if attempting to add an item to a list with an unbounded or upper-bounded wildcard.
- `List<String>` cannot be assigned to `List<Object>`
- `List<String>` can be assigned to `List<?>` however
- Review casting for generics
- `var` and `List<?>` are not the same thing. Both return type Object when calling `get()` though

### Upper-Bounded Wildcards
- `List<? extends Number> list = new ArrayList<Integer>();`
- A compiler error thrown if attempting to add an item to a list with an unbounded or upper-bounded wildcard.
- The upper-bound wildcard says that any class that extends Number or Number itself can be used as the formal parameter type
- Upper bounds are like anonymous classes in that they use extends regardless of whether we are working with a class or an interface

### Lower-Bounded Wildcards
- `List<? super String>`
- We are telling Java that the list will be a list of String objects or a list of objects that are a superclass of String. Either way, it is safe to add a String to the list.
- Lower-bounded objects are mutable (unlike Upper-bounded or wildcard boundless)







---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
