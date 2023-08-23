# Chapter 5 - Core Java APIs
- String, StringBuilder, arrays [], Arrays, ArrayList, Math

# Strings
### String Concatenation
- `String::concat` == `+` == `s += "a"`

### 3 Concatenation Rules
1. If both operands are numeric, `+` means numeric addition
2. If either operand is a String, `+` means String concatenation
3. The expression is evaluated left to right
    - Example: `3 + 5 + "c";` // "8c"

### String Methods
- int     length()
- char    charAt(int index)
- int     indexOf(int ch) // note: "int ch" char promoted to int
- int     indexOf(int ch, int fromIndex)
- int     indexOf(String str)
- int     indexOf(String str, int fromIndex) // offset "fromIndex"
- String  substring(int beginIndex)
- String  substring(int beginIndex, int endIndex)
- String  toLowerCase()
- String  toUpperCase()
- boolean equals(Object obj)
- boolean equalsIgnoreCase(String str)
- boolean startsWith(String prefix)
- boolean endsWith(String suffix)
- String  replace(char oldChar, char newChar) - replaces all occurrences (multiple occurrences)
- String  replace(CharSequence target, CharSequence replacement)
- boolean contains(CharSequence charSeq)
- String  strip() - trim() plus unicodes. 
- String  stripLeading()
- String  stripTrailing()
- String  trim() - simple version of strip
- String  intern() - returns the reference from the string pool (or otherwise place it there and return reference)

# StringBuilder
### Constructors
- new StringBuilder()
- new StringBuilder("animal");
- new StringBuilder(10); // capacity

### StringBuilder Methods
- "chaining StringBuilder methods" because most methods returns a reference to the original object. Builder pattern.
- char          charAt()
- int           indexOf()
- int           length()
- String        substring()
- StringBuilder append(String str)             - appends and return a reference to the current StringBuilder
- StringBuilder insert(int offset, String str) - adds characters to the StringBuilder at the requested index
- StringBuilder delete(int startIndex, int endIndex)
- StringBuilder deleteCharAt(int index)
- StringBuilder replace(int startIndex, int endIndex, String newString)
- StringBuilder reverse()
- String        toString()

### Equality Operator == vs .equals()
- reference equality (==) - same pointer
- `.equals()` is inherited from Object class. Some classes override the method(e.g. String) but others do not (array, StringBuilder)

### String Pool
- `"a" != new String("a")`          // false
- `"a" == new String("a").intern()` // true
- Constant expressions of type String are always "interned".
- `"a" + "b"` is interned automatically since it's a constant expression

# Java Arrays
### Java Arrays methods
- arr1.equals(arr2)    // BAD!, reference equality/same reference
- Arrays.equals(arr1, arr2)
- Arrays.deepEquals(arr1, arr2)
- Arrays.sort(myArr)
- Arrays.sort(myArr, `Comparator<E>`)
- Arrays.binarySearch(myArr, val)   
    * if .contains(), return index; else, return -index -1;
    * Array must be sorted before binarySearching
- Arrays.compare(arr1, arr2)
    * compare is effectively compareTo(). positive, zero when equal, negative.
- Arrays.mismatch(arr1, arr2)
    *  return first index that is different , else (if equal) return -1;

# ArrayList
- Generics vs raw type

### Creating an ArrayList
- `new ArrayList<>();`
- `new ArrayList<>(10);`    // initial capacity
- `new ArrayList<>(list2);` // Construct from Collection

### ArrayList Methods
- boolean add(E element)
- void    add(int index, E element)      - effectively an insert statement. ArrayList doesn't define a .insert() method
- boolean remove(Object obj)
- E       remove(int index)
- E       set(int index, E newElement)
- boolean isEmpty()
- int     size()
- void    clear()                       - discard all elements of the ArrayList
- boolean contains(E element)
- boolean equals(E element)            - ArrayList implements a custom .equals() method. Same elements, same order.

### ArrayList --> Array
- `list.toArray()`          // PROBLEM: returns Object[]
- `list.toArray(String[0])` // Safe: returns String[]. Simply a copy, no linking

### Array --> ArrayList
- `Arrays.asList(arr1)`      // PROBLEM: Fixed size. Linked to the original array. 
- `new ArrayList<>(Arrays.asList(arr1))`      // Safe: passing List as argument
- `List.of(arr1)`            // PROBLEM: Immutable
- `(ArrayList) Arrays.stream(nums).boxed().collect(Collectors.toList());` // int[]
- `            Arrays.stream(nums).boxed().collect(Collectors.toCollection(ArrayList::new));`

### Collections Utility Class
- `Collections.sort(coll)`
- `Collections.binarySearch(coll, ele)`
- `Collections.min(coll)`
- `Collections.max(coll)`

# Set
- defn: Unordered collection that does not allow duplicate elements
- HashSet vs TreeSet

# Map
### Map Methods
- V             get(Object key)
- V             getOrDefault(Object key, V other)
- V             put(K key, V value)
- V             remove(Object key)
- boolean       containsKey(Object key)
- boolean       containsValue(Object value)
- `Set<K>`        keySet()
- `Collection<V>` values()

# Math API
- double min(double a, double b)
- float  min(float a, float b)
- int    min(int a, int b)
- long   min(long a, long b)
- double max(double a, double b)
- float  max(float a, float b)
- int    max(int a, int b)
- long   max(long a, long b)
- long   round(double num) - traditional round (.5 ? up : down)
- int    round(float num)
- double pow(double number, double exponent)
- double random() - random double 0<= x < 1







---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
