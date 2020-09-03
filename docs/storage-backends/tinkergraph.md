# TinkerGraph

![TinkerGraph Logo](../assets/images/databases/tinkergraph.png){: align=right style="height:150px;width:150px" }

>
TinkerGraph is a lightweight, POJO based, in-memory property graph that serves as the reference implementation for the [property graph model](https://github.com/tinkerpop/blueprints/wiki/Property-Graph-Model). If you have a small graph that can be loaded and saved using the GraphML reader and writer library, then TinkerGraph can be handy to use. It is also great for use in writing unit tests in place of other implementations that require greater resources.
>
> —  [TinkerGraph GitHub](https://github.com/tinkerpop/blueprints/wiki/TinkerGraph)

## Driver Configuration and Usage

TinkerGraph is Plume's in-memory storage backend option which can be obtained as follows:
```kotlin
val driver = (DriverFactory(GraphDatabase.TINKER_GRAPH) as TinkerGraphDriver).apply { connect() }
```
On construction the following configuration is automatically added to the `BaseConfiguration` object:
```kotlin
config.setProperty("gremlin.graph", "org.apache.tinkerpop.gremlin.tinkergraph.structure.TinkerGraph")
```
To add more configurations, simply use the `addConfig(key: String, value: String)` method of the `TinkerGraphDriver`.

No additional configuration is typically required as the graph is held in memory in the heap space allocated to the Java Virtual Machine running the application making use of the Plume library. To instantiate the graph, use the `connect()` method which will create an instance of a TinkerGraph database using the stored configuration. The `close()` method can be used to tear down the graph and all resources consumed by it.

The data held in this graph will be deleted upon the termination of the application or process using Plume but this graph can be imported and exported to XML, JSON, or Kryo using the `importGraph(filePath: String)` and `exportGraph(filePath: String)` methods respectively.

## Ideal Use Case

### Rapid testing

The in-memory graph database option is used primarily for testing and graph exploration. TinkerGraph can be paired with a graph visualizer such as Cytoscape as it recognized GraphML. This is the recommended backend to get familiar with Plume and how it works as it is also the simplest to configure.

The extractor's unit tests all make use of the TinkerGraph configuration for fast testing on low resource TravisCI runners.

### Production

There are many use cases where a dedicated storage backend is not necessary. Here are a few applications:

* For analysis on smaller programs or parts of programs where its feasible to store snapshots of graphs in something compact such as Kryo.
* To be part of a DevSecOps lifecycle where the process is run on low resource containers and the graph itself may be disposable but the results can be stored elsewhere.
* High performance is required, but no expertise/resources available for dedicated backends.

### Limitations

* Does not scale well for large programs as uncompressed POJOs represent the graph and everything is stored in the heap of a single JVM.
* No transactions and no resilience to failures. Once a transaction has started, although chance of failure is low, there is no clean rollback if a failure occurs in the middle of a transaction.
