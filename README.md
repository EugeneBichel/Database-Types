## Database Types

The choice of a database for storing data depends on the specific nature of the problem being solved.

#### Data nature
Data nature \ Database type | Relation DBs| Key-value DBs | Documented-oriented DBs | Column-Oriented DBs | Graph
:---:  | :---:  | :---:  |:---: |:---: |:---:
Example | PostgreSQL, MySQL, etc | Redis, etc | MongoDB, Elasticsearch, CouchDB, etc | Apache HBase, Cassandra, etc | Neo4j, etc
Data nature |Structured data; <br/> transactions |grouped values by key|semi-structured| Quick access for individual rows by key combined with the ability to use batch approach for big data processing; <br/> There is a requirement to integrate with Big Data, Hadoop, Hive and Spark; <br/> You don't require the ACID properties from your DB. | "Unlike other databases, relationships take first priority in graph databases."[6]

#### ACID
Database|Relation DBs|Graph-oriented DBS [11]|CouchDB (Documented-oriented)|DynamoDB| MongoDB [10]
:---:  | :---:  | :---: | :---:  | :---: | :---: |
Support ACID|YES|YES|YES|YES|YES

#### CAP
Database type \ CAP|CA|AP|CP
:---:|:---:|:---:|:---:|
Relational|PostgreSQL <br/> MySQL| | |
Key-value| |Redis | |
Document-oriented| MongoDB (No partition) |MongoDB (partition, majority connected) <br/> Riak|MongoDB (partition, majority not connected) <br/> Terrastore(default config) <br/> SimpleDB|
Column-oriented|Vertica|Cassandra|HBase|
Graph|Neo4j| | |

### Keep thoughts about
- CAP guarantees if looks at distributed computing ("a shared-data system can have at most two of the three following properties: Consistency, Availability, and tolerance to network Partitions"[3])
    - C - consistency: "all clients always have the same view of the data"[2]
        - "This is equivalent to requiring requests of the distributed shared memory to act as if they were executing on a single node, responding to operations one at a time." [5]
        - "A system is consistent if an update is applied to all relevant nodes at the same logical time. Among other things, this means that standard database replication is not strongly consistent. As anyone whose read replicas have drifted from the master knows, special logic must be introduced to handle replication lag.
     That said, consistency which is both instantaneous and global is impossible. The universe simply does not permit it."[3]
    - A - availability - Every request receives a (non-error) response – without the guarantee that it contains the most recent write
        - "For a distributed system to be continuously available, every request received by a non-failing node in the system must result in a response"[5]
        - "Means that all clients can always read and write."[2]
    - P - partition tolerance - "the system works well despite physical network partitions"[2]
        - "For a distributed (i.e., multi-node) system to not require partition-tolerance it would have to run on a network which is guaranteed to never drop messages (or even deliver them late) and whose nodes are guaranteed to never die. You and I do not work with these types of systems because they don’t exist."[3]

    "If a system chooses to provide Availability over Consistency in the presence of partitions (all together now: failures), it will respond to all requests, potentially returning stale reads and accepting conflicting writes. These inconsistencies are often resolved via causal ordering mechanisms like vector clocks and application-specific conflict resolution procedures. (Dynamo systems usually offer both of these; Cassandra’s hard-coded Last-Writer-Wins conflict resolution being the main exception.)"[2]

    It is not easy to figure out the two of the three for some specific database

- Data nature
    - structured
    - transactions
    - semi-structured
    - grouped small groups of data by key
    - availability fast scan

### Relation DBs
PostgreSQL, MySQL, etc

- Stores the structured information in a normalized form
- Transactions support
- CAP: consistency and availability

Ex: storage of financial information

### Non-Relation DBs
 - Aggregate-oriented databases
    - Document-based NoSQL databases (e.g. MongoDB, CouchDB)
    - Key/Value NoSQL databases (e.g. Redis)
    - Column family NoSQL databases (e.g. Hibase, Cassandra)
 - Graph-oriented databases (e.g. Neo4J)

#### Key-value
Redis, etc

- Obtain a small amount of information by key
- CAP
    - Redis: consistency and partition tolerance

Ex: storage of users profile

#### Documented-oriented DBs
MongoDB, Elasticsearch, CouchDB, etc

- Storage semi-structured data
- CAP
    - MongoDB: consistency is by default
      also MongoDB can support other guarantees, which are depends on database/driver configuration

Ex: search for large, weakly structured logs, hierarchical aggregation, product catalog in e-commerce systems, storing comments in CMS

#### Column-oriented (BigTable like)
Apache HBase, Cassandra, etc

- Quick access for individual rows by key combined with the ability to use batch approach for big data processing
- CAP
    - Cassandra: availability and partition tolerance

Ex: Storage of features for machine learning

#### Graph DBs
Neo4j

- "Unlike other databases, relationships take first priority in graph databases."[6]
- CAP: consistency and availability

Ex: system of roads, network of devices, knowledge graph, social media

#### OLAP
Vertica, Druid, ClickHouse

- Fast full data scan to build reports in unpredictable slices
- CAP
    Vertica: consistency and availability

Ex: real-time reports for millions of sites in Google Analytics

### CAP triangle taken from [2]
Be careful with the following diagram. Please read comments in [2]

"Visual Guide to NoSQL System" [2]

￼![CAP taken from http://blog.nahurst.com/visual-guide-to-nosql-systems](./CAP.png)


### References:
- [1] [Wiki CAP](https://en.wikipedia.org/wiki/CAP_theorem)
- [2] [Visual Guide to NoSQL Systems](http://blog.nahurst.com/visual-guide-to-nosql-systems)
- [3] [You Can’t Sacrifice Partition Tolerance](https://codahale.com/you-cant-sacrifice-partition-tolerance/)
- [4] [MongoDB Use Cases](https://docs.mongodb.com/ecosystem/use-cases/)
- [5] [Gilbert and Lynch. Brewer’s conjecture and the feasibility of consistent, available, partition-tolerant web services.](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.67.6951&rep=rep1&type=pdf)
- [6] [What is a Graph Database?](https://neo4j.com/why-graph-databases/)
- [7] [Comparison of the Open Source OLAP Systems for Big Data: ClickHouse, Druid and Pinot](https://medium.com/@leventov/comparison-of-the-open-source-olap-systems-for-big-data-clickhouse-druid-and-pinot-8e042a5ed1c7)
- [8] https://stackoverflow.com/questions/11292215/where-does-mongodb-stand-in-the-cap-theorem
- [9] https://aphyr.com/posts/322-call-me-maybe-mongodb-stale-reads
- [10] https://www.mongodb.com/blog/post/multi-document-transactions
- [11] https://martinfowler.com/articles/nosqlKeyPoints.html
