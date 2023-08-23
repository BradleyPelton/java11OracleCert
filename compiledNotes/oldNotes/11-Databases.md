### Two Categories of Database
- Relational
    * SQL
    * rows & columns , tables related via keys
- NonRelational
    * NoSQL
    * json format

### Structured Query Language
- set-based language (set theory)
- flyway is a tool for managing databases
- flyway applies migrations

### JDBC drivers
- Java classes provided by the vendor(mongo, postgres, etc.)
- We code against the common interfaces and not the implementations by each vendor
- drivers are specific for each database type
- drivers implement a common interface to increase swapability

### JDBC Interfaces
- Driver
- Connection
- PreparedStatement
- CallableStatement
- ResultSet

### JDBC URLS
- `jdbc:mysql://localhost:3306/loboticket`
- always starts with "jdbc"
- subprotocol is the vendor/product name (e.g. "mysql")
- Database specific URI

### Connection
- `throws SQLExceptions` almost always need to handle SQLExceptions
- `Connection conn = DriverManager.getConnection(jdbcUrlString);`
- database connections are native resources
- Must be closed after use
- closing a connection will cascade and close statement and resultset
- resources in try-with-resource are separated by (;) not (,)

### Prepared Statement
- The statement interface has two implementations:
    * PreparedStatement
    * CallableStatement

### PreparedStatement
- represents a SQL statement that will be sent to the database
- can be used for any SQL statement
- like connections, Prepared statements must be closed 
```
PreparedStatement stmt = conn.prepareStatement(sqlString)
ResultSet rs = stmt.executeQuery()
```
- executeQuery() returns resultset
- executeUpdate() returns number of affected rows
- execture() returns boolean (if statement returned a ResultSet)
- stmt.getResultSet()
- PreparedStatement can be paramterized
- parameters represented by "?" in the query string
- parameters have a type (Int, String, etc.)
- 1 bsed, not 0 based indexing parameters
```
String sqlString = "insert into venus (name, capacity) values (?,?)";
PreparedStatement ps = conn.prepareStatement(sqlString)
ps.setString(1, "Home");
ps.setInt(2, 300);
ps.executeUpdate();
```
- All query types CRUD support
- Settings can be done out of order
- All parameters must be set however, else error
- setInt(), setString, setObject() on statement object
- The same statement can be executed multiple times, with or without editing it

### ResultSet
- execute query to get a result set
- "Cursor" based
- call "next" to move the cursor
    * return true if there is another row, false else

```
PreparedStatement ps = conn.prepareStatement("select * from table");
ResultSet rs = ps.executeQuery()

while (rs.next()) {
    String name = rs.getString("name");
    int capacity = rs.getInt("capacity")
}
```
- instead of getting values based on column names, we can use index
- `String name = rs.getString(1)`
- If using `COUNT(*)` or similar one row responses, you can use `if(rs.next())` instead of `while(rs.next())`
- int count = rs.next().getInt("count")
- int count = rs.next().getInt(1)
- Using column anmes is safer just incase indexes change
- name columns using "as" in sql
- ps.SetNull(2, Types.CHAR); (settings preparedStatement param as null)

### Callable Statements
- Use CallableStatement to execute a "stored procedure"
- CallableStatement can be passed params, and can have return types
```
String sqlString = " { call GetActs() }";
CallableStatement cs = conn.prepareCall(sqlString);
ResultSet rs = cs.executeQuery
... // loop over resultset that is returned from stored procedure
```
- callable statement with params
- question marks just like PreparedStatement
```
String sqlString = " { call GigReport(?,?) }";
CallableStatement cs = con.prepareCall(sqlString);
cs.set
cs.set
```

### Out Parameters
- `{ ? = call sproc_name(?)}`
- optiona, not all databases support
- register any out Parameters
- cs.registerOutParameter(1, Types.DECIMAL)
- 
### InOut Params
- Set in parameters
- Set INOUT param as in
- register INOUT paramas as out
- execute and retrieve just like a regular out param















