
- Conditions must be wrapped in parentheses
- Condition must evaluate to a boolean


# Switch
- defaults can be anywhere
- cases in switch statements are called branches
- fall-through = after first statement, evaluate all branches below until you reach a match
- a case can fall-through into a default and then back to a case


switch (x * 2) {
    case 4:
        //
    case 6:
        //
        break;
    default:
        //
}

Switch supported types:
- byte/Byte
- short/Short
- int/Integer
- char/Character
- String
- Enum

### Case
- Case values must be "compile time constants"
- compile time constant = CONSTANTS x {arithemetic operators}
- No method calls.

# if/else vs switch
- if else is very flexible
- switch is better for when you are looking for exact value matches
- switch shows intention better


# do/while
do {
    i--;
} while(i > 0)


# for loop
for (initialize; condition; update) {}

- multiple terms in a for loop (i++, j--)
- condition is checked at loop start
- loop control variable isn't defined outside of loop

# for-each
- works with any type that implements Iterable or Array


### Intentional infinite loops:
- for(;;)
- while(true)