# Chapter 4 - Making Decisions (Control Flow)
- defn: statement               - a complete unit of execution, terminated with a semicolon
- defn: control flow statements - statements to break up the flow of executing by using decision making
- defn: block                   - zero or more java statements between balanced braces
- the target of a decision-making statement can be either a block or a single statement. (excluding try)
- Exam Trick: Using indentation to fool single statement blocks
- `goto` is a reserved keyword, but unused in java

### if statement
- parentheses always required
- if statements must use conditionals(evaluate to boolean). `if (1)` compiler error, no truthy-falsy

### switch statement
- defn: switch statement - decision-making structure where a single value is evaluated and branched to matching case.
- if no default option is available and no case matches, nothing happens
- parenthesis required around switch expression
- `case 15:` // colon, not semicolon after case
- braces required around entire switch
- `switch (month) {}` // valid switch statement. case statements are optional
- `switch () {}`      // compiler error, "expression expected"
- allowed switch values:
    * byte/Byte
    * short/Short
    * int/Integer
    * char/Character
    * String
    * Enum
    * var (if the type resolves to one of above)
- boolean/long/float/double notable not allowed. Either too few options or too many
- `break;` or //fall through
- default doesn't need to be the last branch
- a case can fall through into a default below (and a default can fall through into a case below it)
- Exam Trick: missing break statements causing fall-through. 
- !!!! case statements must be compile-time constants
- defn: compile-time constant : constants and trivial combinations of constants (+,concat, etc.)
- Constant expressions of type String are always "interned".
- switch statements support numeric promotion (upcast and downcast to fit switch type)

### While loops
- parentheses required around conditional
- `while(conditional) {}`
- `do {} while (conditional);`
- semicolon required at the end of a do-while

### For loops
- for loop questions consume a lot of time on the exam
- `for (initialization; conditional; updateStatement) {}`
- parentheses required
- semicolon separated
- variables are only in scope inside the for loop
- forward and backward loops important for the exam
- `for(;;)` // valid
- Multi-term for loops
    * `for (int x = 0, y = 10; x < 20 && y >= 0, x++, y--) {}`
    * x,y,... must be the same type in the initialization
- You can't declare an already declared variable
- var is supported in initialization `for (var i = 0;;) {}`

### For each
- `for (datatype instance : collection) {}`
- colon separated
- parentheses required
- Either array, or must implement `Iterable<T>`
- Map, String, StringBuilder not supported.
- var is supported `for (var s : args) {}`
- You can't modify Collection while looping, `ConcurrentModificationException`

### Branching Statements
- defn: label - optional pointer to the head of a statement
- labels can be added almost anywhere, but almost always before loops
- `OUTER_LOOP:`
- labels follow same rules as identifiers. Usually SCREAMING_SNAKE case
- `break;,break OUTER_LOOP;,continue;,continue OUTER_LOOP;,return;,return val;`
- any code directly after break,continue,return is "Unreachable" and will throw compiler error
- dead code != unreachable code






---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
