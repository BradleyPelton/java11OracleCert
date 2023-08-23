# Chapter 22 - Security
- The exam only covers JAVA SE apps, not web apps

### Designing a Secure Object
- techniques:
    1. Limiting Accessibility
        * Principle of least privilege
    2. Restricting Extensibility
        * Preventing classes from being inherited. 
    3. Creating Immutable Objects
        * Marking classes as final, marking instance variables private, removing setters in favor of constructor.
        * Getters returning copies of instance fields instead of the original reference.
    4. Cloning Objects
        * Cloneable interface default (not abstract!) .clone() method
        * .clone() makes a shallow copy by default.
        * overriding .clone() with unique implementation
    
### Injection and Input Validation
- defn: Injection - an attack where dangerous input runs in a program as part of a command.
- defn: exploit - an attack that takes advantage of weak security. 
- SQL Injection:
    * PreparedStatement > string concatenation/formatting
    * PreparedStatements without bind variables is just as bad as concatenation
- Command Injection:
    * uses operating system commands to do something malicious
    * sending ".." as a file name
    * using whitelist/blacklist of allowed/denied values
    * whitelist is better than blacklist because you don't have to forsee all possible issues.

### Confidential Information
- avoid putting confidential information in a `.toString()` method, stack traces, error messages, writing files.
- defn: dump file - a file containing values of everything in memory
- Storing passwords in char[] instead of String to avoid the String Pool
- null out values instead of waiting for garbage collection

### Limiting File Access with Security Policy
- Security Policy explicitly gives certain read/write/execute permissions to each file.
- don't need to know how to write or run a policy.

### Serializing and Deserializing Objects
- Specifying which fields to Serialize
- Don't serialize sensitive inofrmation with `transient` optional specifier
- Encrypting fields that you Serialize
- readResolve() - runs after the readObject() method and is capable of replacing the reference of the object returned by deserialization.
- writeReplace() - runs before writeObject() and allows us to replace the object that got serialized.

### Constructing Sensitive Objects
- Preventing subclasses from changing the behavior
- Marking Methods final
- Marking Classes final
- Marking Constructors private

### DDOS
- defn: Denial of Service attack - DoS attack - one or more requests with the intent of disrupting legitimate requests.
- sending very large requests or sending a lot of requests. 
- defn Distributed denial of service - DDoS - DOS attack from many sources at once.
- checking the size of a file before reading it. 
- defn: inclusion attack - when multiple files or components are embedded within a single file.
- "Zip bomb", "billion laughs attack"
- Overflowing numbers abuse
- overriding .hashCode() to flood a HashMap or HashSet
- Creating arrays of billions of elements


### Extra Exam Topic
https://www.oracle.com/java/technologies/javase/seccodeguide.html

Oracle added the word privileged to the security objective and this results in the doPrivileged() method now being in scope. For these questions, you need to read Section 9 of the Oracle Secure Coding Guidelines for Java SE. Some people reported seeing doPrivileged() on the 1Z0-816 exam, though, so it’s possible Oracle updated the 1Z0-816 exam more recently with this topic.    






---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
