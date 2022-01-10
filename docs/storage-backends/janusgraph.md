# JanusGraph

![JanusGraph Logo](../assets/images/databases/janusgraph.png){: align=right style="height:150px;width:150px" }

> JanusGraph is designed to support the processing of graphs so large that they require storage and
> computational capacities beyond what a single machine can provide. Scaling graph data processing
> for real time traversals and analytical queries is JanusGraph’s foundational benefit.
>
>  —  [JanusGraph Documentation Introduction](https://docs.janusgraph.org/)

JanusGraph is an open-source JVM-based graph database that is queried via the Gremlin query
language. JanusGraph is a split into three parts: itself (the graph database abstraction), a
pluggable storage backend and a pluggable search engine. Plume communicates with JanusGraph over the
Gremlin remote connection which is described in the documentation
[here](https://docs.janusgraph.org/connecting/java/).

!!! warning "JanusGraph No Longer Supported"
    
    As from version 1.0.0 JanusGraph is no longer a supported backend. These docs follow from 
    older versions (<= 0.6.3) from the Kotlin-based Plume libraries.

## Driver Configuration and Usage

JanusGraph's driver can be created as follows:
```kotlin
val driver = (DriverFactory(GraphDatabase.JANUS_GRAPH) as JanusGraphDriver)
    .apply { remoteConfig("src/main/resources/conf/remote-graph.properties").connect() }
```

Where one needs two configuration files `remote-graph.properties` and `conf/remote-objects.yaml`. In
the example above, these two files are in `src/main/resources/conf/`. For a simple local JanusGraph
connection, one can use the following configurations:

=== "conf/remote-graph.properties"
    ```conf
    gremlin.remote.remoteConnectionClass=org.apache.tinkerpop.gremlin.driver.remote.DriverRemoteConnection
    gremlin.remote.driver.clusterFile=src/main/resources/conf/remote-objects.yaml
    gremlin.remote.driver.sourceName=g
    ```

=== "conf/remote-objects.yaml"
    ```yaml
    hosts: [localhost]
    port: 8182
    serializer: { 
        className: org.apache.tinkerpop.gremlin.driver.ser.GryoMessageSerializerV1d0,
        config: { ioRegistries: [org.janusgraph.graphdb.tinkerpop.JanusGraphIoRegistry] }}
    ```

The driver will automatically detect if the JanusGraph connection makes use of transactions or not
and will create and commit them accordingly.

More information on making use of JanusGraph can be found on [their
documentation](https://docs.janusgraph.org/).

## Ideal Use Case

JanusGraph is ideal in the case where budget for an enterprise license is limited but a dedicated,
distributed, and remote graph database is required. [Scylla](https://www.scylladb.com/) is the
recommended storage backend for a fast and efficient distributed system with
[ElasticSearch](https://www.elastic.co/) as the search engine. For resource sensitive single-node
environments I would suggest using
[BerkeleyDB](https://www.oracle.com/technetwork/database/berkeleydb/overview/index-093405.html) and
[Lucene](https://lucene.apache.org/).

### Benefits

The following exerpt is taken from the documentation:

- Support for very large graphs. JanusGraph graphs scale with the number of machines in the cluster.
- Support for very many concurrent transactions and operational graph processing. JanusGraph’s
  transactional capacity scales with the number of machines in the cluster and answers complex
  traversal queries on huge graphs in milliseconds.
- Support for global graph analytics and batch graph processing through the Hadoop framework.
- Native support for the popular property graph data model exposed by [Apache
  TinkerPop](https://tinkerpop.apache.org/).
- Native support for the graph traversal language
  [Gremlin](https://tinkerpop.apache.org/gremlin.html).
- Numerous graph-level configurations provide knobs for tuning performance.
- Vertex-centric indices provide vertex-level querying to alleviate issues with the infamous [super
  node problem](http://thinkaurelius.com/2012/10/25/a-solution-to-the-supernode-problem/).
- Provides an optimized disk representation to allow for efficient use of storage and speed of
  access.
- Open source under the liberal [Apache 2 license](https://en.wikipedia.org/wiki/Apache_License).

### Limitations

- JanusGraph with plugged in storage backends and search engines can become very resource intensive.
  An example is with Cassandra and ElasticSearch.
- JanusGraph is the slowest storage backend of the bunch so for large programs, indexing and
  enforcing the CPG schema in the configuration will become necessary. An example of this is in the
  [Plume Examples](https://github.com/plume-oss/plume-examples) repository.
- JanusGraph can store up to a quintillion edges (2^60) and half as many vertices. That limitation
  is imposed by JanusGraph’s id scheme.
- Retrieving an edge by id, e.g `tx.getEdge(edge.getId())`, is not a constant time operation because
  it requires an index call on one of its adjacent vertices. Hence, the cost of retrieving an
  individual edge by its id is `O(log(k))` where `k` is the number of incident edges on the adjacent
  vertex. JanusGraph will attempt to pick the adjacent vertex with the smaller degree.

    This also applies to index retrievals for edges via a standard or external index.