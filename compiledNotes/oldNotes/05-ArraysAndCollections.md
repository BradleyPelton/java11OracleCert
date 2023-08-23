


defn: Colllection -  A group of elements with a relationship. Usuually ordered by index. Usually backed by an array under-the-hood
- List, Sets, Queues, and Maps are the 4 main types of Collections
- performance of lists degrades as size grows
- Sets have unique elements
- Queues
- FIFO
- FILO
- Maps
- performance doesn't change as map size grows
- keys are unique, not values

## Basic Collection Type Attributes and Characterists
You should be able to answer the following questions about each core Collection type

|     | Elements Ordered?   | Allows Duplicates? | Elements Added/Removed in a Certain Order? | Allows Key/Values? |
|-----|---------------------|--------------------|--------------------------------------------|--------------------|
|List | Yes - Numbered Index| Yes                | No                                         | No                 |
|Set  | No                  | No                 | No                                         | No                 |
|Queue| Yes - Placed Order  | Yes                | Yes                                        | No                 |
|Map  | No                  | Yes - Values only  | No                                         | Yes                |


## Specific Collection Type Attributes and Characterists
You should be able to answer the following questions about each Collection implementation provided by Java

| Java Type| Parent Interface Type | Allows null? | Elements Sorted? | Uses hashCode? | Uses compareTo? |
|----------|-----------------------|--------------|------------------|----------------|-----------------|
|ArrayList | List                  | Yes          | No               | No             | No              |
|LinkedList| List and Queue        | Yes          | No               | No             | No              |
|HashSet   | Set                   | Yes          | No               | Yes            | No              |
|TreeSet   | Set                   | No           | Yes              | No             | Yes             |
|HashMap   | Map                   | Yes          | No               | Yes            | No              |
|TreeMap   | Map                   | No           | Yes              | No             | Yes             |


# Arrays
- primitive arrays vs object arrays
- `int[] arr = new int[10];`
- Arrays must be instantiated with size or with elements
- zero indexed
- defn: Anonymous array - An array without a type `int[] ids = {1,2,3}; `
- Address space
- new String[3] generates {null, null, null}
- the array class doesn't have many useful methods
- The helper class, Arrays, holds most useful methods
- Arrays.sort(arr) // default sort
- Arrays.sort(arr, comparator)
- sort in place
- Arrays.toString()

### Arrays search
- Arrays,binarySearch(arr, 3)
- Must be sorted first
- Efficient searching technique.
- binarySearch {
    if (.contains) {
        return index
    } else {
        return -index - 1;
    }
}

### Array comparison
- BAD: arr1.equals(arr2) is object equals (reference equality)
- GOOD: Arrays.equals(arr1, arr2) same elements in same order
- Arrays.compare()
- Arrays.mismatch() - find the first index where arrays are different
- must be the same array type or exception thrown

### Varargs
- main(String[] args) is the most famous example
- main(String... args)

### 2d array
- int[][] multiArray
- int[][] multiArray = new int[3][2]
- Arrays.deepToString()
- Arrays.deepEquals()

---------------------------------------------------------------------------------------------------

# List
- Collection interface at java.util.List
- Famous implementation is java.util.ArrayList

### ArrayList
- Improved version of basic array
- backed by array under-the-hood
- new ArrayList()
- new ArrayList<>()
- new ArrayList(100)           - Initial capacitiy optimization 
- new ArrayList(arrList2)
- When using collections, it's often best practice to reference the class by it's parent interface rather than the implementation
- This helps prevent tightly coupled code
- List<String> = new ArrayList<>();
- Collection<String> = new ArrayList<>();
- .add("yellow")
- .add(1) // 1 is autoboxed to Integer
- .add (13, "orange") // insert orange to index 13 
- .get(i)
- `colors.forEach(color -> System.out.println(color));`
- ConcurrentModificationgException

### ArrayList -> Array
- Object[] = myArrayList.toArray();
- String[] = myArrayList.toArray(new String[0]);

### Array -> ArrayList 
- Arrays.asList(new int[]{1,2,3}) // mutable
- List.of()                       // immutable

### LinkedList
- implements both List and Queue interface
- LinkedList<String> orders = new LinkedList<>();
- orders.add("order1")
- orders.add("order2")
- orders.addFirst("order5");
- orders.addLast("order4");
- orders.removeFirst();
- orders.removeLast();

### Collections
- Collection vs Collections
- Collections is a helper class similar to Arrays
- Collections.sort(myArrList)  // in place sorting
- Collections.max();

---------------------------------------------------------------------------------------------------

# Sets
- Set is an interface
- hashcode()  (uniqueness factor for set)
- sets implement collection
- .add()  // returns boolean. True if added, false else
- .size()
- .isEmpty()

### HashSet
- HashSet - "hashes" elements and creates a "mapping" from hash to element
- When adding an element to a hashset, it checks if the hash value already exists. 
- Since this is done in a key-value pairing, adding elements to HashSets is very efficient 

### TreeSet
- TreeSet - orders elements as they are added to the set
- TreeSet doesn't check hashcodes like HashSet
- TreeSet uses a heap under the hood
- Comparable compareTo() for constructing the heap

---------------------------------------------------------------------------------------------------

# Maps
- Map doesn't implement Collection
- Two main Maps, HashMap and TreeMap
- HashMap, like HashSet, is unordered
- TreeMap, like TreeSet, is ordered
- .size()
- .isEmpty()
- .containsKey()
- .containsValue()
- .get()
- .put()
- .keySet()  // returns Set type
- .remove()
- duplicate key put's will overwrite existing key-value
- myMap.put(null, null) is valid . map allows a single null element to be mapped to a key

### TreeMap
- TreeeMap is sorting and ordering keys as they are added
- TreeMap and TreeSet are built off "red-black trees"
- TreeMaps sort and rebalance the tree as keys are added
- TreeMaps do not allow null key

---------------------------------------------------------------------------------------------------

# Generics


### Before Generics
- List without generic allows any type
- List num = new ArrayList();
- nums.add(1);
- nums.add(2L);
- nums.add("3");
- nums.add(4.0);
- This compiles fine, but is a nightmare to handle

### Generics
- Generics are defined and declared with the diamond syntax <>
- determine types at compil;e time
- if generic Type is not added, Java will set T type to Object

### Naming Convention
- E - Element (used frequently by Collections package)
- K - Key
- V - Value
- T - Type
- S,U,V,... second, third, ... types after T is used

### Method declaration
- `public static <T> T myMethod(T[] t) {return t[0]};`

### Wildcards
- Wildcard generics are used when you don't know what the type will be
- List<?> list - Unbounded Wildcard
- List<? extends Number> - Bounded wildcard. ? must be a subclass of Number
- List<? super Number>   - Bounded wildcard. ? must be a superclass of Number
- AutoBoxing - int -> Integer


























