# Batch updates 	

The batch statement is introduced in JDBC 2.0 
it helps to reduce the database roundtrips and focus to send multiple statements in a single request

---

### Batching statements

statement is an interface defined by JDBC to use Batching api 

NB : using statement for CRUD can be bad for production for its prone to SQL injection attacks

The executeBatch() function returns the size of rows affected 

int[] updateCounts = statement.executeBatch();

#### Drivers behavior differs 

Oracle JDBC doesnt support batching statement when calling addBatch() it still treats it as one by one

Only preparedStatement works

In MySql batching is fake until configured in the right way

So to enable batching you should enable this 

rewriteBatchedStatements=true


#### Does reorder statement works

Test cases :
Case A
post insert
comment insert
post insert
comment insert

Case B
all post inserts first
all comment inserts after


No major diffrence 

because the bottleneck is not the order but :

- network calls
- driver behavior
- batch support 
