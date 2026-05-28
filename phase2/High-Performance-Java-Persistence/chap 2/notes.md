## JDBC Connection Management 

JDBC is an interface for communicating with Db server
all the networking logic and the database communication are hidden away behind the independent JDBC API

Jdbc interfaces must be implemented depending on requirements

java.sql.Driver is the main entry for interacting with the JDBC 

There is 4 driver types 

• Type 1 (JDBC-ODBC Bridge) → Translates JDBC calls into ODBC calls. Requires an ODBC driver on the client machine. Platform-dependent, slow, and removed in Java 8.
• Type 2 (Native API) → Uses database-specific native libraries (e.g., C/C++ DLLs). Faster than Type 1, but requires installing native code on every client, making deployment harder.
• Type 3 (Middleware/Network) → Sends JDBC calls to a middle-tier server using a generic network protocol. The server then talks to the database. Centralizes connectivity but adds infrastructure complexity.
• Type 4 (Pure Java/Thin) → Implements the database's wire protocol directly in Java. No native libraries, no middleware, no ODBC. Just a .jar file.

• Connection → Your active session to the database (used to run queries & manage transactions).

• Driver → The vendor-specific engine that actually knows how to speak the database's protocol.

• DriverManager → Java's built-in helper that automatically matches your JDBC URL to the correct driver and returns a ready-to-use Connection.

Bottom line: Instead of manually handling low-level drivers, you just call DriverManager.getConnection(). It finds the right driver for you, keeps your code database-agnostic, and simplifies setup.

### Driver Manager 

each time the driver manager invoke getConnection, the driver request a new connection from the driver

in the first versions of JDBC for each user = a new physical connection to db


### Datasource 

Three tier application : is an application where theres a middlware between the client request and the database source

having a middleware can have numerous advantages:
-in a entreprise application user request (higher) than the database connections,
so the middleware acts as a database connection

logical connections : proxies or handlers that means using the same physical connection over and over with diffrent
requets

JTA (JAVA TRANSACTION API): it can help to make manage a distributed transaction, it uses the logical connections 

Diffrence between DriverManager and Datasource interface 

DriverManager : creates a physicall connection everytime you invoke getConnection()
Datasource : creates a logical connection (that uses an existent physical connection) when you invoking getConnection()

