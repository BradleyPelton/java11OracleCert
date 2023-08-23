# Chapter 15 - Functional Programming
- Java i/o streams and the Streams API are nothing alike, even though they use the same name.
- Recall functional interfaces have exactly one abstract method

### Built-in Functional interfaces
- UnaryOperator and BinaryOperator are special cases of Function. T,T -> T
- UnaryOperator extends Function, just with different formal type parameters
- The generic declarations on UnaryOperator force the functionality on Function.
- Predicate returns boolean, not Boolean. If Boolean returned, think Function not Predicate.

### Convenience Methods on Functional Interfaces
- All of these help modify and combine functional interfaces 
- Must be the same functional interface type
- Consumer  :::: .andThen()
- Function  :::: .andThen() , .compose(), 
- PREDICATE :::: .and(), .or(), .negate()
- remember f o g , f.compose(g), is equivalent to f(g(x)).

# Optional
- Optional objects are used to express "not applicable" or "we don't know"
- Optional is created using a factory method

### Optional methods
- Factory methods
    * .of()    - creates an optional of `<T>` type
    * .ofNullable() - factory method to return Optional.empty() if value==null, or Optional.of(value) else.
    * .empty() - creates an empty Optional
- Methods
    * .isPresent()  - returns true if Optional is not empty
    * .ifPresent() - Consumer
    * .get() on an optional without checking is empty could throw `NoSuchElementException`
    * .orElse() - maxOpt.orElse(0);
    * .orElseGet() - Supplier
    * .orElseThrow() - zero param or one param with `Supplier<Exception>`
    * .getAsInt() vs .get() only available for OptionalInt/OptionalDouble/OptionalLong, not Optional

# Streams
- defn: stream - a sequence of data of elements from a source that supports aggregate operations.
- defn: stream pipeline - operations that run on a stream to produce a result
- defn: stream operations - operations (intermediate or terminal) that act on the elements of a stream.
- stream does not implement `Iterable<T>` and thus can't be used in for-each loops
- finite streams have a limit, infinite streams do not
- lazy evaluation - delays execution until necessary
- Three parts of stream pipeline:
    1. Source                  - where the stream comes from
    2. Intermediate operations - transforms the stream and returns another stream
    3. Terminal operations     - Ends the stream and returns a result
- There is no limit to the number of intermediate operations
- Intermediate operations do not run until the terminal operation runs
- Any intermediate operation after a terminal operation will throw SymbolError 
- Special streams exist for primitives
- You can't reuse the same stream

### Stream Sources
- finite stream constructors
    - Stream.empty()
    - Stream.of(T...)
    - coll.stream()
    - coll.parallelStream()
    - Arrays.stream()
- infinite stream constructors
    - Stream.generate(Math::random);
    - Stream.iterate(1, n -> n + 2);
    - Stream.iterate (identity, Predicate, UnaryOperator) - (1, n < 100, n -> n + 2);
- you can limit infinite streams with .limit()
- `Stream.<String>builder()` - returns a builder for a stream.cf

### Terminal Operations
- Reductions are special type of terminal operations where all of the contents are combined into a single primitive or Object
- .count() - Reduction operator. return long number of elements.
- .min() and .max() - Reduction operator. Takes a Comparator and returns `Optional<T>`.
    * Required Comparator param, no "natural order" (unlike .sorted())
- .findAny() and .findFirst() - Terminal but not reduction. returns Optional. "short circuit" not all elements are seen.
- .allMatch(), anyMatch(), noneMatch() - Takes a `Predicate<T>` and returns boolean
- .forEach() - Takes a `Consumer<T>` and returns void.
    - collection.forEach() is different than myStream.forEach()
- .reduce() - combines a stream into a single object
    * Three different signatures 
        1. `T           reduce(T identity, BinaryOperator<T> accumulator)`
        2. `Optional<T> reduce(BinaryOperator<T> accumulator)`
        3. `<U> U       reduce(U identity, BiFunction<U, ? super T,U> accumulator, BinaryOperator<U> combiner)`
    * identity is the initial value of the reduction (e.g. "", or new ArrayList)
    * accumulator combines the current result with current value
    * combiner combines any intermediate totals.
    * `Stream.of("A", "B", "C").reduce("", String::concat);` // "ABC"
- .collect() - mutable reduction
    * Use a prebuilt Collector in `Collectors` class or create one yourself
    * More efficient than a regular reduction because we reuse the same mutable object while accumulating
    * Two different signatures
        1. `<R>   R collect(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator, BiConsumer<R,R> combiner)`
        2. `<R,A> R collect(Collector<? super T,A,R> collector)`
    * `stream.collect(StringBuilder::new, StringBuilder::append, StringBuilder::append)`
    * supplier creates the object that will store the results. Similar to identity for .reduce()
    * accumulator BiConsumer that adds one more element 
    * combiner    BiConsumer merges two intermediate/subsection results together
    * `stream.collect(TreeSet::new, TreeSet::add, TreeSet::addAll)`
    * `stream.collect(Collectors.toCollection(TreeSet::new)`
    * `stream.collect(Collectors.toList()`
    * `stream.collect(Collectors.toSet()`

### Intermediate Operations
- returns another stream
- .filter() - Takes a Predicate and returns only the elements where predicate is true
- .distinct() - No params and returns a stream without duplicates. (calls .equals() under the hood)
- .limit() - returns a stream with a smaller source
- .skip()  - skips the first n elements
- .map()   - maps elements from old stream to a new stream
    * `<R> Stream<R> map(Function<? super T, ? extends R> mapper)`
    * can return a stream of a different type than the original Stream (e.g. .map(String::length))
- .flatMap() - returns a stream of the elements of the 
    * `<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper)`
    * Helpful for combining a stream of lists.
    * `Stream<ArrayList<Integer>> -> Stream<Integer>`
- .sorted() - returns a stream with the elements sorted
    * uses natural ordering unless a comparator is provided
    * `stream.sorted(String::compareTo)`
- .peek()  - apply a Consumer without altering or terminating the Stream
    * `Stream<T> peek(Consumer<? super T> action)`
    * Not intended to change results. If you want to change, use a map

# Primitive Streams
- Convert `Stream<Integer>` -> IntStream with `stream.mapToInt(x -> x)`
- Primitive streams have many of the same intermediate and terminal methods as Stream
- Primitive streams also include specialized methods for working with numeric data.

### Creating Primitive Streams
- All primitives don't have their own Streams class. There are only three.
- Three Types of primitive streams
    1. IntStream - used for byte,short,int, and char
    2. LongStream - used for long
    3. DoubleStream - used for float and double
- same factory methods, .of(), .empty(), .generate(), .iterate()

### Primitive Stream methods
- .boxed() returns a `Stream<T>` where T is the boxed version of prim in PrimStream
- .min(), .max(), .average(), .sum
- .range() , .rangedClosed()
    * `IntStream.range(1,11)` // 1-10, `[a,b)`
    * `IntStream.rangeClosed(1,10)` // 1-10, `[a,b]`
- .summaryStatistics()
- flatMapToInt, flatMapToDouble, flatMapToLong

### Mapping Streams
- `Stream<T>` -> IntStream 
    * stream.mapToInt()
- mapToInt(), mapToDouble(), mapToLong(), mapToObj()
- IntStream -> `Stream<T>`
    * iStream.mapToObj()
- The parameters to these mappers are actually custom interfaces
    * `ToDoubleFunction<T>`
    * `ToIntFunction<T>`
    * `LongToDoubleFunction`
- charts on page 711 and 712

### Primitive Options
- primitive streams return OptionalDouble, instead of `Optional<T>`
- `Optional<Double>` holds wrapper  Doubles
- OptionalDouble    holds primitive doubles
- `OptionalDouble opt = stream.average()`
- sum() notably returns int/double/long, not OptionalInt/OptionalDouble/OptionalLong

### Summarizing Statistics
- `IntSummaryStatistics stats = iStream.summaryStatistics()`
    - `iStream.summaryStatistics().getMax()`
    - `iStream.summaryStatistics().getMin()`
    - `iStream.summaryStatistics().getSum()`
    - `iStream.summaryStatistics().getAverage()`
    - `iStream.summaryStatistics().getCount()`

### Special Functional Interfaces for Primitives
- `BooleanSupplier.getBoolean()          returns boolean`
- `IntConsumer.accept()                  void`
- `DoublePredicate.test()                returns boolean`
- `LongFunction<R>.apply()               returns <R>`
- `IntUnaryOperator.applyAsInt()         returns int`
- `DoubleBinaryOperator.applyAsDouble()  returns double`
- `ToIntFunction<T>                      returns int`
- `ToDoubleBiFunction<T,U>               returns double`
- `LongToDoubleFunction                  returns double`
- `ObjIntConsumer                        returns void`

---------------------------------------------------------------------------------------------------

# Advanced Streams
### Collection backed Streams
- coll.stream() creates a stream BACKED by a collection. Altering the underlying collection also changes the stream
- Recall streams can only be traversed once. Altering the backed collection after doesn't do anything. Only before.

### Collecting Results
- Collector is an interface
- Collectors is a helper class
- Collectors interface has static factory methods for Collector objects.
- Collect is passed as a parameter to the `stream.collect()` method
- Collectors.averagingDouble(), averagingInt, averagingLong - calculates the average and returns Double
- .counting() - counts the number of elements, returns Long
- .averagingDouble()
    * `stream.collect(Collectors.averagingInt(String::length))`
- groupingBy
- joining
    * `Stream.of("lions", "tigers", "bears").collect(Collectors.joining(", "));` // lions, tigers, bears
    * .joining delimiter
- maxBy, minBy
- mapping
- partitioningBy
- summarizingDouble
- summingInt
- toList
- toSet
- toCollection
    * `.collect(Collectors.toCollection(TreeSet::new))`
- toMap

### Collecting into Maps
- There are many different ways to collect into maps, some being complicated
- Simple:
    * `stream.collect(Collectors.toMap(Function::identity, String::length))`
- When creating a map, you need to specify two functions. One for key creation, the other for value creation.
- `IllegalStateException: Duplicate key 42`
- collector will freak out if duplicate keys are created.
- groupingBy() and partitioningBy() appear on the exam a lot
- .groupingBy()
    * return type = Map
    * `.collect(Collectors.groupingBy(String::length))`
    * groupingBy tells collect() to group all of the elements of the stream into a map
    * The function determines the keys in the map.
    * Each value is a List of entries
    * does not allow null keys
    * downstream collector - A second collector that does something special with the values
    * `stream.collect(Collectors.groupingBy(String::length, Collectors.toSet()))` // two Collectors references!
- .partitionBy()
    * Partitioning is a special case of grouping
    * Partitioning groups values into key=True and key=False
    * `Map<Boolean, List<String>> myMap = stream.collect(Collectors.partitioningBy(s -> s.length() <= 5))`
    * passing a Predicate
    * We can change the collection that holds the values. Defaulted to List, but we can make it a set, etc.
    * `Map<Boolean, Set<String>> myMap = `
                `stream.collect(Collectors.partitioningBy(s -> s.length() <= 5), Collectors.toSet())`
    * 

### OCC MAP 
- Creating an occurence map
```
 Map<Integer, Long> occMap = stream.collect(Collectors.groupingBy(String::length, Collectors.counting()))
 Map<Integer, Long> occMap = stream.collect(Collectors.groupingBy(Function::identity, Collectors.counting()))
        Map<Integer, Integer> occMap  = Arrays.stream(nums).boxed().collect(
                Collectors.groupingBy(
                        x -> x,
                        Collectors.summingInt(x -> 1)
                )
        );
```








---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
