# OverflowDB Graph Serialization

The `Graph` object can be exported to XML and JSON representations for
easy visualization and allows the `Graph` object to be imported into a
TinkerGraph or OverflowDB backend.

## GraphML

[GraphML](http://graphml.graphdrawing.org/) is an XML-based format for
representing various graph formats and is readily able to be visualized by
[Cytoscape](https://cytoscape.org/). Example:

=== "Java"
    ```java
    Graph g = driver.getWholeGraph();
    OutputStreamWriter writer = new FileWriter("graph.xml");
    // The OutputStreamWriter will be opened and closed in the write method
    GraphMLWriter.INSTANCE.write(g, writer);
    ```

=== "Kotlin"
    ```kotlin
    val g = driver.getWholeGraph()
    val writer = new FileWriter("graph.xml")
    // The OutputStreamWriter will be opened and closed in the write method
    GraphMLWriter.write(g, writer)
    ```

## GraphSON

[GraphSON](https://tinkerpop.apache.org/docs/current/reference/#graphson) 
is an JSON-based format with usage aimed to be used in the TinkerGraph
ecosystem. Example:

=== "Java"
    ```java
    Graph g = driver.getWholeGraph();
    OutputStreamWriter writer = new FileWriter("graph.json");
    // The OutputStreamWriter will be opened and closed in the write method
    GraphSONWriter.INSTANCE.write(g, writer);
    ```

=== "Kotlin"
    ```kotlin
    val g = driver.getWholeGraph()
    val writer = new FileWriter("graph.json")
    // The OutputStreamWriter will be opened and closed in the write method
    GraphSONWriter.write(g, writer)
    ```
## Importing Results

Both GraphML and GraphJSON format can readily be imported into a `TinkerGraphDriver`
using the `TinkerGraphDriver::importGraph` method. The following Kotlin example
illustrates this:

```kotlin
val d1 = (DriverFactory(GraphDatabase.TINKER_GRAPH) as TinkerGraphDriver).apply { connect() }
val d2 = (DriverFactory(GraphDatabase.TINKER_GRAPH) as TinkerGraphDriver).apply { connect() }
d1.importGraph("graph.xml")
d2.importGraph("graph.json")
g1 = d1.getWholeGraph()
g2 = d2.getWholeGraph())
assertEquals(g1.nodes().asSequence().toSet(), g2.nodes().asSequence().toSet())
assertEquals(g1.edges().asSequence().toSet(), g2.edges().asSequence().toSet())
g1.close(); g2.close(); d1.close(); d2.close()
```