### Secure
- Oracle secure coding guidelines - https://www.oracle.com/java/technologies/javase/seccodeguide.html
- Defn: Invariant - a check on a variable (assert age < 120)
- Invariants are good to be placed in constructors
- Add Private construictors if using factories
- place final keyword on classes when no need to extend
- making fields final is great. Helps with threadsafe
- package private is better than public and often is enough
- isntead of returning a list, return a copy of the list

### Serialization 
- serialization has seious security flaws

###
Secure Objects
- In general, remember to:
    * keep code simple
    * avoid duplication
    * minimize permission checks
    * detail security
- When designing objects, remember:
    * Encapsulation
    * Immutability
    * Input validation
    * avoid Cloneable and Serializable

- Don't show error stacktraces to users
- It allows hackers to learn about file/class structure
- Don't show exceptions
- Don't ever log personally identifiable info

- defn: Heap Dump
- clean up unused variabled
- use try-with-resources

### Injection
- sql injection
- in spring, use prepared statements instead of statements
- Bind variables (?)
- Don't output untrusted data
- encode outputs

### 
- favor allowlists over blocklists
- parse  user strings to int,long,date 

### Inclusion
- Don't load unkown resource (e.g. uploaded files)
- load remote resources only via Https
- load resources locally using an allowlist

### DDOS
- Defn: Denail of Service Attack - an attack that edxhausts an applications resources preventing authorized access to the system
- consider memory cache abuse
- Zip bomb (20 kb as zipped, 5,46 Gb when unzipped)
- Compression ration
- Overflow checking

### Security Manager
- policy files
- security manager must be turned on via java command line arg
- `java -Djava.security.manager -Djava.security.policy=my.policy`
- policy files are built as allowlist
- policy can specifcy permissions to files, directories, modules
- protection domain

```
grant codeBase "file:/etc/deployments" {
    permission java.io.FilePermission "tracks/-","write";
}
```

