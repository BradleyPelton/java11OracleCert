# Chapter 21 - JDBC
- defn: JDBC   - Java Database Connectivity
- JDBC works by sending a SQL command to the database and then processing the response
- EXAM UPDATE: - CallableStatement is removed from the exam

### Intro Relational Databases and SQL
- defn: database            - an organized collection of data
- defn: relational database - database that is organized into tables(rows and columns)
- Persistence is not on the exam
- defn: SQL - Structured Query Language - a programming language used to interact with database records.
- defn: primary key - a unique way to reference each row
- basic SQL:
    * create - INSERT
    * read   - SELECT
    * update - UPDATE
    * delete - DELETE
- SQL syntax is out of scope

---------------------------------------------------------------------------------------------------

# JDBC Interfaces
- For the exam, we need to know five key interfaces of JDBC, java.sql
- With JDBC, the concrete classes come from the JDBC drivers provided by the creators of the databases
- The concrete classes implement the key interfaces, allowing code to work with different databases.
- 5 key interfaces
    1. Driver            - establishes a connection to the database
    2. Connection        - sends commands to a database
    3. PreparedStatement - Executes a SQL query
    4. CallableStatement - (REMOVED FROM EXAM)
    5. ResultSet         - Reads results of a query

### Connecting to a Database
- Connecting to a database requires a third party JAR downloaded. 
- `jdbc:postgres://localhost:5432/zoo`
- `jdbc:mysql://localhost:3306`
- `jdbc:oracle:thin:@123.123.123.123:1521:zoo`
    1. Protocol    - "jdbc" always
    2. Subprotocol - Product/Vendor name
    3. sub-name     - database specific connection details
- sub-name often includes a URI/IP address
- Separated by colons
- Don't need to memorize the sub-name formats. 

### Getting a Database Connection
- Two main ways to get a Connection
    1. DriverManager
    2. (Not on the exam) DataSource
- DriverManager uses the factory pattern with a static method to return a Connection
- `DriverManager.getConnection("jdbc:postgres://localhost:5432/zoo")`
- `DriverManager.getConnection("jdbc:postgres://localhost:5432/zoo", "username7", "password123")`
- Overloaded to pass "username", "password" optionally
- Checked SQLException

### PreparedStatement
- Statement interface (not in scope)
- Sub-interfaces PreparedStatement and CallableStatements
- Statement can be used directly, but PreparedStatement is better for : 1. Performance 2. Security 3. Readability 4. Future use
- PreparedStatements prevent SQL Injection, but Statements do not
- `try(conn.prepareStatement("SELECT * FROM a")) {}`
- PreparedStatement represents a SQL statement that you want to run using conn. 
- PreparedStatement does not actually execute the query 
- String param with the SQL query is mandatory parameter.
- `.executeQuery()` is used for SELECT, `.exceuteUpdate()` is used for DELETE, INSERT, UPDATE
```
try (PreparedStatement ps = conn.prepareStatement(insertSQLQuery)) {
    int numberOfChangedRows = ps.executeUpdate();
}
```
- Each distinct SQL statement needs its own `conn.prepareStatement()` factory method call
```
try (var ps = conn.prepareStatement(sqlSelectString) ; ResultSet rs = ps.executeQuery()) {
    //
}
```
- PreparedStatement methods:
    * `.executeQuery()`
        * SELECT only   
        * returns ResultSet 
        * `ResultSet rs = ps.executeQuery()`
    * `.executeUpdate()`
        * UPDATE, DELETE, INSERT
        * returns int (number of rows changed)
        * `int numberOfChangedRows = ps.executeUpdate();`
    * `.execute()`
        * any type (SELECT, UPDATE, DELETE, INSERT)
        * returns a boolean
        * returns true if the query would return a ResultSet instead of an integer
        * `boolean isResultSet = ps.execute()`


### Parameters in PreparedStatements
- defn: bind variable - placeholder that lets you specify the actual values at runtime
- Bind variables are like parameters to PreparedStatements.
```
String sqlString = "INSERT INTO names VALUES(?, ?, ?)";
try (PreparedStatement ps = conn.prepareStatement(sqlString)) {
    ps.setInt(1, 1337);
    ps.setString(3, "frank");
    ps.setInt(2, 69420);
    ps.executeUpdate();
}
```
- Use .setInt(), .setString(), .setDouble(), .setObject(), setBoolean() to set the bind variables
- EXAM TRICK: bind variables are 1-indexed! JDBC starts counting columns with 1.
- Bind variables can be set out-of-order 
- All bind variables must be set before the query is executed else SQLException
- You can reuse a PreparedStatement object in a for loop and resetting bind variables

### ResultSet
- Recall calling .executeQuery() will return a ResultSet if the sqlString is a SELECT statement
- ResultSets have a cursor
- Always use loop or if statement for ResultSets when calling `rs.next()`
- defn: cursor - pointer to the current location in the data
- `while (rs.next()) {}`
- The cursor points to a current row and we retrieve that row's columns using `.getInt()` , `.getString()`, etc.
- .getString("middleName") or .getString(3) // 3 is the index of middleName
- Using index out of bounds or a column name that doesn't exist throws SQLException (exam trick: .getInt("count") )
- Column name is better because it provides context and immune to changes in Query ordering
```
try (var ps = conn.prepareStatement("SELECT id, name FROM name_table) ; ResultSet rs = ps.executeQuery()) {
    while(rs.next()) {
        int id = rs.getInt(1);    // 1-indexed!
        String name = rs.getString("name")
    }
}
```

### CallableStatements
- CallableStatement are removed from the exam

### Closing Database Resources
- JDBC resources such as Connection are expensive to create.
- Not closing them creates a resource leak. 
- JDBC resources need to closed in a specific order (reverse order of creation)
    1. ResultSet 
    2. PreparedStatement 
    3. Connection
- Closing a Connection also cascade closes PreparedStatements(which also cascade closes ResultSets)
- JDBC also automatically closes a ResultSet when you run another SQL statement using the same PreparedStatement






---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
