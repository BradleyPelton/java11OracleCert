# Chapter 3 - Operators
- defn: java operator - special symbol that can be applied to a set of operands and that returns a result
- defn:       operand - the value or variable the operator is being applied to.

### Types of Operators
- unary
- binary
- ternary
- arithmetic
- logical
- java operators are sometimes right-to-left(e.g. assignment), but mostly left-to-right evaluated.

### Operator Precedence
- defn: operator precedence - order in which operators are evaluated
- assignment operator has the lowest order of precedence
- parentheses override the order of precedence
- ORDER
    0. UMARE LTA
    1. Unary                      (in order: ++a, a++, (-,!,~,+,(cast)))
    2. Multiplication/division/mod (*, / , %)
    3. Add/subtract               (+,-)
    4. Relation                   (<,>,<=,>=, instanceof)
    5. Equality                   (==,!=)
    6. Logical Operators          (in order: (&,^,!) , (&&,||)) // regular before short circuit
    7. Ternary                    (a ? b : c)
    8. Assignment                 (=,+=,-=,*=)

### Unary Operators
- one operand
- pre  increment (returns a+1 and stores a+1)
- post increment (returns a   and stores a+1)
- increment and decrement operators will overflow, but stay the same type(126,127,-128,...)
- logical compliment (!) not to be confused with number negation (-)

### Binary Operators
- two operands
- Arithmetic operators (also include unary operators)
- balanced parentheses
- defn : modulus - aka remainder operator - remainder left after dividing value.
- Integer division edge case: integer division returns the floor value of the division.

### Numeric Promotion
-
- Rules:
    1. If two different sizes, smaller data type is widened
    2. bye,short,int automatically promoted to int when used in binary arithmetic operators.
    3. Promotions are retained after evaluation

### Assigning Values
- defn: assignment operator - binary operator that modifies, or assigns, a value to a variable
- java will automatically promote from smaller to larger data types(widening)
- java requires cast operator to convert larger to smaller data type (narrowing)
- defn: casting: unary operator where one data type is explicitly interpreted as another data type.
- Unary means you have to be extra careful with parentheses around casting
- The assignment operator returns the value assigned to the left variable.

### Overflow and Underflow
- defn: overflow - when a number is so large that it will no longer fit, so the system "wraps around" to the lowest negative value.
- defn: underflow - when a number is so small that it no longer fits, wraps back to positive MAX
- 2147483647 + 1 == -2147483648 // true

### Compound Assignment Operators
- int a += b;
- equivalent to int a = (int)(a + b);
- compound assignment operator implicitly casts the right side back to left side, even if overflow. 

### Assignment Operator Return value
- The result of an assignment operator is an expression in and of itself.
- Java assignment operator is python walrus operator
- The result assigned to the left operand is also returned (a = 10 also returns 10)
- Exam Trick: boolean questions `(healthy = false)` // both assignment and returning the value false;

### Comparing Values
- equality operator
- "reference equality operator"
- Three types of equality operators:
    1. Comparing two number or character primitives. Upcasted for comparison. `5 == 5.0` // true
    2. Comparing two boolean
    3. Comparing two object references (reference equality)
- All other uses of equality operator throw an exception
- null == null // true, same "location"
- reference equality doesn't have to be the same type ("bob".equals(bossObj) compiles but is false))
- reference equality has an `instanceof` check which immediately returns false if the objects are different types

### Relation Operators
- defn: compare two expressions and return a boolean value
- <, >, <=, >=, instanceof
- less than and greater than (or equal) only applies to numeric values.
- less than and greater than follow same Numeric Promotion (to int!) rules

### instanceof Operator
- `a instanceof b`
- defn: instanceof - return true if the reference that "a" points to is an instance of a class, subclass, or class that implements a particular interface "b"
- best practice to check instanceof before casting to a narrower type
- instanceof throws compiler error if attempting to compare incompatible types (A !<= B and B !<= A)
- instanceof struggles with interface instanceof. If class is final, instanceof can catch bad interface type comparisons, else not.
- `null instanceof Object` is always false
- `anything instanceof Object` is always true
- `anything instanceof null` // compiler error
- instanceof supports arrays
    - `new int[]{1} instanceof int[]`        // true
    - `new Integer[]{1} instanceof Number[]` // true
    - `new Boss[]{1} instanceof Employee[]` // true
- instanceof can't be used with primitives

### Logical Operators
- bitwise operators not on the exam
- ^ is XOR operator (a && !b || !a && b) (different values for both)
- short circuit operators
    * right operand may not evaluate if the left side is deterministic 
- "underperformed side affect" because of short circuiting
- short circuit is good at preventing NPE ` a != null && a.length() > 0`

### Ternary
- `a ? b : c`
- just a condensed form of if/else. 
- conditional ? expression_1 : expression_2 
- `int e = a ? b : c`
- if used with assignment operator, both `b` and `c` must evaluate to covariant subtype of `e`, even if always true
    `int animal = (stripes < 9) ? 3 : "Horse"` // does not compile
- if used alone, no requirement on type 
    `System.out.println((stripes < 9) ? 3 : "Horse")` // fine
- ternary can also have "underperformed side affect". false condition is never evaluated if true is true.






---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
