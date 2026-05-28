## Datasource
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

