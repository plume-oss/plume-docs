# PlumeGraph Serialization

The `PlumeGraph` object can be exported to XML and JSON representations for
easy visualization and allows the `PlumeGraph` object to be imported into a
TinkerGraph backend.

## GraphML

[GraphML](http://graphml.graphdrawing.org/) is an XML-based format for
representing various graph formats and is readily able to be visualized by
[Cytoscape](https://cytoscape.org/). Example:

=== "Java"
    ```java
    PlumeGraph graph = driver.getWholeGraph();
    OutputStreamWriter writer = new FileWriter("Plume_Graph.xml");
    // The OutputStreamWriter will be opened and closed in the write method
    GraphMLWriter.INSTANCE.write(graph, writer);
    ```

=== "Kotlin"
    ```kotlin
    val graph = driver.getWholeGraph()
    val writer = new FileWriter("Plume_Graph.xml")
    // The OutputStreamWriter will be opened and closed in the write method
    GraphMLWriter.write(graph, writer)
    ```

## GraphSON

[GraphSON](https://tinkerpop.apache.org/docs/current/reference/#graphson) 
is an JSON-based format with usage aimed to be used in the TinkerGraph
ecosystem. Example:

=== "Java"
    ```java
    PlumeGraph graph = driver.getWholeGraph();
    OutputStreamWriter writer = new FileWriter("Plume_Graph.json");
    // The OutputStreamWriter will be opened and closed in the write method
    GraphSONWriter.INSTANCE.write(graph, writer);
    ```

=== "Kotlin"
    ```kotlin
    val graph = driver.getWholeGraph()
    val writer = new FileWriter("Plume_Graph.json")
    // The OutputStreamWriter will be opened and closed in the write method
    GraphSONWriter.write(graph, writer)
    ```
## Importing Results

Both GraphML and GraphJSON format can readily be imported into a `TinkerGraphDriver`
using the `TinkerGraphDriver::importGraph` method. The following Kotlin example
illustrates this:

```kotlin
val d1 = (DriverFactory(GraphDatabase.TINKER_GRAPH) as TinkerGraphDriver).apply { connect() }
val d2 = (DriverFactory(GraphDatabase.TINKER_GRAPH) as TinkerGraphDriver).apply { connect() }
d1.importGraph("Plume_Graph.xml")
d2.importGraph("Plume_Graph.json")
assertEquals(d1.getWholeGraph(), d2.getWholeGraph())
d1.close()
d2.close()
```