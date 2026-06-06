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


### Batching preparedStatements


WHy using preparedStatements is way better 

its way safer than using statement cuz it validates the provided params at the runtime 

example :

PreparedStatement postStatement = connection.prepareStatement(
"INSERT INTO post (title, version, id) " +
"VALUES (?, ?, ?)");
postStatement.setString(1, String.format("Post no. %1$d", 1));
postStatement.setInt(2, 0);
postStatement.setLong(3, 1);
postStatement.addBatch();


And the figures shows that preparedStatement use case perfoms better than statement for crud Operations

the fact is that the statement should be used for bulk processing instead of the CRUD

#### Choosing the right batch size

You cant choose the right batch size cuz theres no mathematical equation 

therefor the right way to find the exact number is the trial and error

Diminishing Returns: Performance gains are not linear. 

Moving from a batch size of 1 to 10 yields a massive speed boost, but increasing it from 100 to 110 yields almost no noticeable improvement.

##### the danger of too big 

Too Big is Bad: While larger batches save network trips, 

making them too large can actually hurt performance by causing memory exhaustion, 

long database locks, or transaction timeouts.


Rule of Thumb: Always test different values, 

but as a general starting point,

a batch size between 10 and 30 is usually the safest and most effective choice.

