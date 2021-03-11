# Neo4j

![Neo4j Logo](../assets/images/databases/neo4j.png){: align=right style="height:150px;width:150px" }

> Neo4j is a native graph database, built from the ground up to leverage not only data but also data
> relationships. Neo4j connects data as it’s stored, enabling queries never before imagined, at
> speeds never thought possible.
>
>  —  [Neo4j Homepage](https://neo4j.com/)

Neo4j is a schema-less graph database written in Java with a simple query language called
[Cypher](https://neo4j.com/developer/cypher/guide-sql-to-cypher/) which is queried either via a
transactional HTTP endpoint or through the Bolt protocol. Plume queries Neo4j via Cypher over the
Bolt protocol.

## Driver Configuration and Usage

Neo4j's driver can be created as follows:
```kotlin
val driver = (DriverFactory(GraphDatabase.NEO4J) as Neo4jDriver).apply { 
        hostname("127.0.0.1")
            .port(7687)
            .username("neo4j")
            .password("neo4j123")
            .database("neo4j")
    }
```

The driver makes use of the official [Neo4j Java
driver](https://github.com/neo4j/neo4j-java-driver). As described by the Gremlin driver's README,
the following are the pros and cons of using the Gremlin driver over the Cypher one:

## Ideal Use Case

Neo4j is a mature graph database vendor with a simple learning curve. The UI is mature and Cypher is
a simple query language to get started with basic traversals. Neo4j provides a cloud service called
[Neo4j Aura](https://neo4j.com/cloud/) which provides consumption-based pricing and much easier
maintainability. The community for Neo4j is extremely large and so is the number of available
resources and books.

When combined with Plume, this schema-free graph database is ideal for rapid prototyping and quick
deployments as the software is lightweight and UI very simple to use. With this does come
limitations as query results cannot be visualized in more than a force-directed structure and, since
it runs on the JVM, heapspace consumption can become a problem on extremely large code property
graphs.

### Benefits

The following benefits are obtained from the Neptune homepage:

- Supports clustering with master-slave topology.
- High read and write performance while maintaining data integrity with concurrent transaction
  support.
- Scalable architecture optimized for response times and ACID (Atomicity, Consistency, Isolation,
  Durability) compliant. Supports transactions and locking.
- Drivers and APIs for all major languages.
- Flexible data model to allow a change of schema on the fly.
- Provides high availability and real-time data analysis.

### Limitations

- Data is in-memory on each machine and disk seeks are expensive. So once system memory is hit, the
  performance slows down dramatically as data overflows on the disk.
- Master-slave architecture means that all writes are sent to master - but once a master is down, a
  new master election does not take long. This results in a write-bottleneck.
- Requires a lot of JVM configuration to use effectively, since queries and context are stored in
  the JVM heap, over-allocation to heap memory may result in increasing GC pauses and
  under-allocation may result in out-of-memory exceptions.