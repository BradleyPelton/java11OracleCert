

# Interface
- defn: interface - An interface is like a conctract for the classes that implement it.
- Rules:
    * Interfacess can't be instantiated
    * Interface can't contain constructors
    * Usually, interface methods do not have a body
- default and static methods were introduced for compatability reasons


### Why Interfaces
- great at abstraction
- Interfaces allow multiple inheritance

###
- Interface methods are by default abstract and public
- Interface methods must be overridden and must be public
- Interfaces can also contain constants
- fields in interfaces must be public static final

### default methods
- defn: default method - a method defined in an interface that has an implementation(i.e. must have a body)
- default methods can be overridden, but don't have to be

### static interface methods
- defn: static interface method - a method that belongs to the interface
- `InterfaceName.staticMethodName();`


---------------------------------------------------------------------------------------------------

# Functional Interface
- Defn: Functional Interface - A functional interface is an interface with a single abstract method
- functional interfaces from java.util.function (e.g comparator)
- Can have any number of other default methods, only limited to one abstract method
- @FunctionalInterface annotation is used to describe Interface top-level type
- @FunctionalInterface is optional
- @FunctionalInterface acts like @Override with compiler safety (compiler will throw error if more than one abstract method)


### Famous Examples
- Function      T -> R              - BiFunction      T,U -> R
- Consumer      T ->                - BiConsumer      T,U ->
- Supplier        -> R              
- Predicate     T -> boolean        - BiPredicate     T,U -> boolean
- UnaryOperator T -> T              - BinaryOperator  T,T -> T

### Composition
- built in support
- firstFunction.andThen(secondFunction) // f o g
- firstFunction.compose(secondFunction) // g o f
- firstPredicate.and(secondPredicate)

### Special Primitive Functional Interface
- Primitives have special special functional interfaces pre-built
- e.g.
    * IntFunction
    * LongToDoubleFunction
    * ObjIntConsumer
- SYNTAX: `PrefixToSuffixInterface`

---------------------------------------------------------------------------------------------------

# Lambda Expressions
- defn: An anonymous method
- Lambda expressions implement functional interfaces.
- Lambda expressions are "functional programming" and not OOP
- functional interfaces from java.util.function (e.g comparator)
- replacing comparator with a lambda express
- Recall Functional Interface := Interface with a single abstract method

### Syntax
- `(a,b) -> a + b`
- params   body
- You can optionally specify the parameter types
- `() -> true`     - Zero parameter lambda
- `file -> file.isHidden()`
- Lambda expressions can have either a single expression or an entire block as a body
- Variables used in Lambda expressions must be final or effectively final
- "captured local variable"
- "this" in Lambda expression refers to the enclosing object
- "super"
- Internal Iteration - streams iterate automatically w/o loops
- Collections use External Iteration; streams use Internal Iteration

# Method references
- System.out::println  // static
- Student::new         // constructor
- aStudent::getName    // instance
- Method references implement Functional interfaces

### Downsides
- Lambda expressions have a hard time dealing with Exceptions

---------------------------------------------------------------------------------------------------

# Streams
- Defn: Stream - a sequence of elements from a source that supports aggregate operations
- Streams let you process data in a declarative way
- Streams can leverage multi-core architecture easily using `.parallelStream()`
- Streams don't actually store elements. They are computed on demand
- Streams consume data from a source, such as collections, arrays, or I/O resources
- interface java.util.stream.Stream
- Intermediate Operations
- Terminal operations
- Stream processing is lazy (nothing happens until terminal operators)
- stream without terminal operations won't do anything
- streams have several factories
- eager vs lazy operations
- Streams don't modify the source (collection, arrays, etc.)
- you can't iterate over the same stream more than once
- streams can be truly infinite

### Pipeline
- Pipelining - Many stream operators return a stream object
- This allows operators to be chained together to form larger pipelines
- This enables certain optimizations such as laziness and short-circuiting

### Factory Methods
- Arrays.stream(new String[]{"a", "b", "c"})
- Stream.of("one", "two", "three");
- Stream.ofNullable("four");
- Stream.empty();
- Stream.generate(UUID::randomUUIID); // infiniteStream
- Stream.iterate(BigInteger,ONE, n ->n.multiply(BigInteger.TWO));
- Stream.builder<String> builder = Stream.builder().add("one").add("two")
- Stream<String> s = builder.build()

- BufferedReader br.lines() generates a stream of lines

###
- .filter() takes Predicate lambdas
- .map()    takes Function  lambdas
- .flatMap() - returns a stream of values
- .findFirst() requires a filter to proceed it
- all search operations are terminal
- .noneMatch()
- .allMatch()
- .anyMatch()
- .findAny()
- .findFirst
- .findFirst and .findAny are "short-circuiting"
- .allMatch() is non-"short-circuiting"

### Reducing and Collecting Streams
- .collect(Collectors.toList())
- Collectors is a util class from streams package
- .collect(Collectors.joining(",")) - join on delimiter ","

### Famous operations
- Famous intermediate
    * .filter()
    * .map()
- Famous terminal
    * .forEach()
    * .collect()

### Reduction
- .reduce((a,b) -> a+b)
- .reduce(identity, accumulator, combiner)
- .reduce(BigDecimal.ZERO, BigDecimal::add);
- .reduce(BigDecimal.ZERO, (r,p) -> r.add(p.getPrice()), BigDecimal::add)

### Collect
- .collect(supplier, accumulator, combiner)
- .collect(ArrayList::new, (list, person) -> list.add(person.getName()), ArrayList::addAll);

### Stream -> HashMap
- products.stream()
    .collect(Collectors.toMap(
        Product::getCategory,      // Key mapper function
        Product::getPrice,         // value mapper
        BigDecimal::add
    ));

### Grouping Stream Elements
- `Map<Category, List<Product>` productsByCategory = products.stream()
    .collect(Collectors.groupingBy(Product::getCategory));

### Partitioning Streams
- Big Decimal priceLimit = new BigDecimal("5.0");
- `Map<boolean, List<Product>>` partitionedProducts = products.stream()
    .collect(Collectors.partitionBy(
        product -> product.getPrice.compareTo(priceLimit) < 0
    ));
- List<Product> cheapProducts = partitionedProducts.get(true);
- List<Product> expensiveProducts = partitionedProducts.get(false);

### Parallel Streams
- multi-threaded streams
- multiple cores, much faster
- sequential streams vs parallel stream
- If bottle-necked by database, parallel streams might not be faster
- Collectors.groupingByConcurrent instead of Collections.groupingBy

### Specialized Streams
- IntStream, LongStream, DoubleStream
- DoubleStream prices = products.stream()
    .maptoDouble(p -> p.getPrice.doubleValue());

### Summary Statistics
DoubleSummaryStatistics stats = product.stream()
    .mapToDouble(p -> p.getPrice().doubleValue())
    .summarystatistics();
- stats.getCount(), stats.getSum(), stats.getMax(), stats.getAverage()

---------------------------------------------------------------------------------------------------

# Comparable and Comparator
- Comparable Interface - An interface that imposes total ordering
- Comparator - A comparison method
- Comparators are built and defined OUTSIDE the class(lambdas)
- Comparable is implemented         INSIDE the class 
- FunctionalInterface
- int compare(T o1, T o2)
- When creating classes, such as Person class, implement Comparable Interface
- By implementing Comparable Interface, you must implement `compareTo` method
- Collections.sort(people) // No Comparator function needed because Person implements Comparable
- Comparator is also overridden in lambda functions often (PriorityQueue)





























