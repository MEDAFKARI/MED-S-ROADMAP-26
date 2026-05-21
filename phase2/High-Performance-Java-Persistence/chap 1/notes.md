# Performance and Scaling

The most important metrics in an entreprise application are response time and throughput 
lower time of the response = more responsive app

throughput : is the amount of work, data, or materials a system processes or produces within a specific time frame

---

## Response time and throughput

transaction response time means the time it takes to complete a transaction 
-starting from the connection 
-time it takes to send data to db over the wire (connection)
-execution time
-sending the results back 
-idle (the queue before the execution of the statement)

```
T =tacq +treq +texec +tres +tidle
```

throughput is the number of the transaction executed in a time interval

X = transaction_count/time

the less the time is the more transaction the system can treat

Adding more connection to database can show an improvement in the throughput
```
X(N) =X(1)×C(N)
```

but adding more database connection doesnt mean that the performance will imporve in a linear line  

it improves at first, then flattens out, and eventually gets worse

```
C(N) = N / [1 + α(N − 1) + βN(N − 1)]
```
N = number of concurrent database connections (or load generators)
C(N) = relative throughput you actually get (compared to 1 connection)
1 = the baseline work of the first connection (no waiting, no coordination)
α(N − 1) = time lost to contention (waiting for serial resources)
βN(N − 1) = time lost to coherency (keeping all sessions consistent)


**USL Peak Concurrency**: `N_max = √((1-α)/β)` — optimal connections before throughput drops.  
**Absolute Capacity**: `X_max = X(1) × C(N_max)` — real-world max throughput at `N_max`.  
**α** = contention (serial bottlenecks), **β** = coherency (coordination overhead).  
If `N > N_max`, adding connections *reduces* throughput (retrograde scalability).  
→ Tune connection pools using measured α/β; don't just "add more".

---

## Database connections boundaries

Every connection client -> server requires a TCP socket
the number of the connection depends heavily on the underlying hardware resources and how many connections the server can handle 

Even if all indexes are entirely cached in memory,disk access might still occur if the requested data blocks are not cached into the memory buffer pool.

high-throughput database applications experience contention on CPU
When all the database server resources are in use, adding more workload only increases contention(conflictss)

---

## Scaling up and scaling out

Scaling Up (Vertical): Add more resources (CPU, RAM, storage) to a single machine. 
Simpler to manage but limited by hardware caps and creates a single point of failure.

Scaling Out (Horizontal): Add more machines to distribute load. More complex to operate but offers better cost efficiency with commodity hardware and improved fault tolerance.

Historical Context: Relational databases traditionally scaled vertically, relying on Moore's Law for hardware gains.

Examples: Facebook uses horizontal scaling (open-source stack); StackOverflow uses vertical scaling (licensing costs influenced decision).

Key Takeaway: No single server is immune to failure—database replication is essential for high availability regardless of scaling strategy.

Decision Factors: Consider hardware/license costs, operational complexity, performance needs, and resilience requirements.

---

### Master-Slave replication

Use Case: Ideal for enterprise systems with high read/write ratios to boost availability.

Roles: Master handles all writes (system of record); Slaves replicate changes for reads.

Replication Types: Binary (uses WAL) or Statement-based (replays SQL commands).

Async Replication: Common & high throughput, but Slaves are eventually consistent (may lag).

Warm Standby: Asynchronous topology; failover requires an election process (not instant).

Sync Replication: One synchronous Slave ensures data consistency but increases write latency.

Hot Standby: Synchronous topology; Slave is an exact copy ready for immediate automatic failover.

Trade-off: Async favors performance/latency; Sync favors data consistency/durability.

Scaling Reads: Offloading read traffic to Slaves reduces Master contention and improves response times.

Limitation: If write traffic overwhelms the Master, consider switching to Multi-Master replication.

---

### Multi-Master replication

Multi-master replication all nodes are equal and they both can accet read-only and read-write operations 

this splitting algorithm can increase the transaction throughtput 

cost:

-The same data can be modified concurrently in multiple nodes so theres some high possibility of conflicting updates 

-No longer a single source of truth, a fail in one node can rollback all the writes in all nodes 

To avoid conflicts :

-Commit protocol can be used to enlist all participating nodes in one distributed transaction. This design allows all nodes to be in sync at all time, at the cost of increasing transaction response time 

Although avoiding conflicts is better from a data consistency perspective, 

synchronous replication might incur high transaction response times. 

asynchronous replication can provide better throughput,

The asynchronous Multi-Master replication requires a conflict detection and an automatic
conflict resolution algorithm.
