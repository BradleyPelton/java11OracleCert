# Exception Hierarchy
- (insert picture)

# Exceptions
- The compiler will check that you check Checked Exceptions
- Unchecked Exceptions, aka runtime Exceptions
- Both checked & unchecked will crash the program if left unhandled
- All unchecked exceptions subclass Runtime Exception
- When a class overrides a method, it is not allowed to add a higher-level or ne Checked Exceptions to the method signature (because of Polymorphism)
- Overloading: different "throw x" doesnt make a new signature
- To create a custom Exception, you must extend Exception or RuntimeException or any child
- `class MyCustomException extends IOException {}`
- Never `catch Throwable` because you will catch errors
- Prefer catching specific exceptions
- Don't leave catch Exceptions blocks empty
- super(message,ex)
- Wrapping Exceptions
- e.getCause()
- exception stack trace
- e.printStackTrace()
-

# Checked Exceptions
- FileNotFoundException extends IOException
- IOException is very General
### Common Checked Exceptions
- IOException                       -
- FIleNotFoundException             -

# Unchecked/Runtime Exceptions
- The compiler does not require, nor is it recommended, to check runtime exceptions. Just let them happen
- NUmberFormat extends IllegalArgumentException
### Common Unchecked/Runtime Exceptions
- ArithmeticException               - Divide by zero
- ArrayIndexOutOfBoundsException    - `arr[arr.length]`
- ClassCastException                -
- IllegalArgumentException          -
- IllegalStateException             -
- NullPointerException              - null.toString()
- NumberFormatException             - Double.parseDouble("b")


# Error
- errors should not be catch, handled, or try to recover from
- errors are runtime/unchecked exceptions
- all errors extend `Error` class

### Common Errors
- ExceptionInInitializerError       - error in static or instance block
- StackOverFlowError                - infinite recursion
- NoClassDefFoundError              - jar file missing at runtime(perhaps was deleted)
- OutOfMemoryError                  - read a file too big

# Rethrowing
- throw vs throws
- throws is used in method declaration
- throw is used to manually throw new Exception();


# try
- catch or finally is mandatory if you try
- curly brackets are required for try-catch, even if it is a single line
- In general, catch child/subclass Exceptions before parent Exceptions

### try syntax
```
try { 
    //
} catch (ExceptionType ex) {
    //
} finally {
    //
}
```


# Chaining catch blocks
- chained catch blocks must respect the class hierarchy
- If a catch block is unreachable, the jvm will throw a compilation error
- different idenntifiers can be used for chained blocks
- identifier is only defined in it's enclosing scope(catch)

### chaining catch blocks syntax
```
try {
    //
} catch (ExceptionType1 ex) {
    //
} catch (ExceptionType2 ex) {
    //
}
```

# Multi-catch Block
- only a single identifier
- separated by pipe
- types in multicatch must be disjoint
- parent and child exceptions can not be in a multi-catch block. You must use chained blocks instead of a multi-catch
- multi-catch blocks are great for removing duplication
- multi-catch can be used in chained blocks as well
### Multi-catch Block syntax
```
try {
    //
} catch( Exception1 | Exception2 e) {
    //
}
```

# try-with-resources
- catch is optional
- finally is optional
- Resource type must be declared
- var is allowed
- Resources in try block are not accessibile in catch or finally block
- resources are closed in reverse order (2, then 1)
- Resource must implement autocloseable

### try-with-resources syntax
```
try (Resource1; Resource2) {
    //
}
```

