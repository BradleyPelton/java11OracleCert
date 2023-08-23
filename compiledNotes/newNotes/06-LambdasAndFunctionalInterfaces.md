# Chapter 6 - Lambdas and Functional Interfaces
- Doesn't cover method references, see chapter 14

# Lambda expressions
- aka lambdas
- defn: Lambda expression - let you express instances of single-method classes more compactly.
- Lambdas are like a method that you can pass as if it were a variable.
- Lambdas are an example of Deferred execution - code is specified now, but will run later.
- Lambdas work with interfaces that have only one abstract method(functional interfaces).

### Syntax
- Simplest form `a -> a.canHop()`
- lambda param types can be omitted
- lambda parameter types are inferred from the context(functional interface params).
- lambda return type(s) are inferred from the context(functional interface return type).
- rules:
    * Parentheses can be omitted only if there is a single parameter AND no type
    * Braces can be omitted when body has only a single statement (same with if/else)
    * return is omitted IFF braces are omitted IFF semicolon is omitted 
    * lambda parameter can be var, but if one var, all params must be var.
- Just like methods, when returning void, return is optional.
- edge case: () -> {} is valid syntax.

### Functional Interface
- defn: Functional Interface - Interface with only 1 abstract method
- @FunctionalInterface annotation is similar type safety as @Override. Optional, but best practice

### Four Fundamental Functional interfaces
- java.util.function
- Predicate - T -> boolean
    * `.test()`
- Consumer T -> {}
    * `.accept()`
- Supplier {} -> T
    * `.get()`
- Comparator TxT -> int
    * `.compare()`
    * `(a,b) -> a.compareTo(b)` is a Lambda that results a list in ascending order {1,2,3}

### Lambda Rules
- defn: effectively final - values does not change after it is set. Can be marked final and still compile.
- effectively final means no reassignment. Effectively final != immutable. Effectively final variables can be altered.
- Variables can appear in three places in lambdas: 1. Params, 2. Local variables defined inside Lambda body, 3. Variables already defined outside of lambda body.
    1. param variables 
        - var can be used as type for lambda param type
        - param can be defined final inside of lambda parentheses
    2. local variables defined inside lambda body
        - can't redeclare variables already defined in outer scope
    3. Variables already defined outside of lambda body
        - lambda can access instance variables, method parameters, or even other local variables
        - method params/local variables      must be effectively final or final
        - instance/static variables don't have to be effectively final or final
        - lambda parameters         don't have to be effectively final or final
- instance/static variables don't have to be effectively final because when the temporary java class file is created for the Lambda, the instance/static variables are copied over.

### Example
- `List.removeIf(Predicate<T>)`
- `Collections.sort(Comparator<T>)`
- `Collection.forEach(Consumer<T>)`




---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
