# Chapter 10 - Exceptions
### Exceptions
- Three types of exceptions: Checked, Unchecked, Errors
- Exceptions are objects
- Exceptions can be stored in variables
- Don't forget the "new" in the `throw new Exception();` statement
- errors should not be recovered from (e.g. OutOfMemoryError)
- Throwable is the parent of all Errors&Exceptions

### Checked Exception
- defn: Checked Exception: an exception that must be handled or declared
- All Checked Exceptions inherit Exception but not RuntimeException.
- An exception that "might reasonably be expected to recover from" - Oracle
- "Handle or Declare" rule
    * Checked exceptions must be Handled (try-catch) or declared (throws in method declaration)

### Unchecked Exception
- runtime != Runtime
- Unchecked a.k.a. runtime exception, but notably also includes Errors subclasses, not just RuntimeException subclasses.
- defn: Unchecked Exception - an exception that doesn't need to be handled or declared
- It is optional to handle/declare. You can throw & throws & catch runtime Exceptions
- Adding `throws IllegalStateException;` or any runtime exception to a method declaration doesn't do anything. "documentation"
- Technically errors are a subset of Unchecked Exceptions
- declaring runtime exceptions is redundant

### Errors
- Defn: a form of unchecked exception that extends the Error class
- They are thrown by the JVM
- should not be handled, declared, or thrown
- E.g. StackOverflowError OutOfMemoryError
- ExceptionInInitializerError- thrown when a static initializer throws an exception and doesn't handle it
- NoClassDefFoundError - thrown when a class that the code uses is available at compile time but not run time (could have been deleted)

# Try statements
- curly braces are always required, even for single statements in a try-statement
- either (catch or finally) blocks required. try statement can't be alone (unlike try-with-resource)
- only one catch block can execute
- unreachable exception catch
- catch has to come before finally if both appear
- declaring/"throws" an unused exception isn't considered unreachable code.
- catch blocks that catch unrelated Checked exceptions are unreachable code and do not compile
- catch blocks for unchecked exceptions are always "reachable", even if they have nothing to do with the code.
- "swallowing"/suppressing/ignoring exceptions is bad practice. At least log something.

### Chaining catch blocks
- at most one catch block will run
- each catch block much be "reachable"
- more specific exceptions first
- the exception variable is only in scope for that specific catch block
- Unrelated checked exceptions (not thrown by any of the statements) are unreachable and thus compiler error

### Multi-catch block
- `} catch( ArrayIndexOutOfBoundsException | NumberFormatException e) {`
- allows multiple exception types to be caught by the same catch block
- only one variable (usually `e` or `ex`)
- pipe separated `|`
- No redundant catches regardless of order.
- Must be absolutely exclusive, no subtypes, regardless of order
- you can chain multi-catch and "single-catch" blocks together

### finally block
- finally block always executes, regardless of Exceptions or returns
- catch is optional if finally block exists. One or the other or both
- finally can't come before catch
- if finally block has a return statement, this return statement will override all others. Finally always executes even if another block calls return. Finally "interrupts" the other return statements.
- finally blocks can also themselves be interrupted by exceptions
- finally blocks always execute, but aren't guaranteed to finish
- System.exit() is the only thing that will stop the finally block from executing. Stops everything, hard stop.

### try-with-resource
- aka automatic resource management
- compiler replaces (try-with-resources) -> (try + implicit finally) block under the hood.
- compiler implicitly creates a finally block to close resource
- try-with-resource can have a finally block. It runs after implicit finally block
- when multiple resources are opened, they are closed in REVERSE ORDER.
- `try(in = null; out = null;){ `
- similar to for loop syntax
- semicolons separated. Last semicolon is optional
- No multi-declaration.
- catch is optional, finally is optional
- try-with-resources is only try that can omit both optional blocks
- try-with-resources still responsible for checked exceptions (e.g. IOExceptions)
- Resources must implement AutoCloseable
- var allowed since a resource is a local variable
- resource is only in scope with try and implicit finally, not catch or explicit finally
- (NEW) resources can be declared before try-with-resource if the resource is final/effectively final. See Chapter 16.

### Overriding methods with exceptions
- Override rule 3 : "An overridden method is not allowed to add new checked exceptions. Only same, fewer, or more specific"
- Put another way, an overridden method must throw the same, fewer, or covariant checked exceptions.
- Overridden method can throws new or covariant UncheckedExceptions at will
- Overridden method can declare fewer exceptions. (similar to unnecessary throws clause)
- If Interface/Abstract abstract method throws `Exception` (e.g. Callable), all implementations are free to throw any Exception

### Output
- `System.out.println(e)`                      - "java.lang.RuntimeException: cannot hop"
- `System.out.println(e.getMessage())`         - "cannot hop"
- `e.printStackTrace()`          = "java.lang.RuntimeException: cannot hop /n at Handling.hop(Handling.java:15) /n ..."
- `e.getMessage()` returns the message from the Exception Constructor
- `System.out.println(e)` or `e.toString()` returns just the first line of the stacktrace (exception type and message)






---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
