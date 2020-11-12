# Understanding the PlumeGraph Object

The `PlumeGraph` object is a primitive adjacency graph representation of query results
from the underlying graph database. The `IDriver` interface provides methods such a 
`getWholeGraph`, `getNeighbours`, etc. which return results in the form of the `PlumeGraph` 
object.

## Use cases

The `PlumeGraph` object was designed for the following ideal use cases:

* **Graph Database Agnostic Model**. Many known analysis' have imperative algorithms already
    implemented using adjacency graph representations. Implementing these directly on the 
    graph database requires graph database specific query language expertise.
* **CPG Specific Representation of Results**. The `PlumeGraph` object makes use of domain
    objects and thus provides a schema adherring representation of results.
* **View-Model Ready Representation**. `PlumeGraph` provides an ideal representation of
    results for visualization tools. `PlumeGraph` can be serialized to XML and JSON 
    representations - see [PlumeGraph Serialization](graph-io.md).
* **Ideal for Debugging and Validation**. `PlumeGraph` is great for making sure that the CPG
    graph in the database is valid and the result is as intended.
* **Useful for Manual Graph Traversals**. When traversing paths and the graph, one could 
    work on a per method basis as each `PlumeVertex` in the `PlumeGraph` object can be used 
    in `IDriver::getNeighbours` for DFS and BFS-style algorithms.

## Limitations

The `PlumeGraph` object is a versatile and useful in-memory representation of results
but it comes with its limitations.

* **Limited Scalability**. Using `IDriver::getWholeGraph` on industrial sized programs will 
    defeat the scalability advantages provided by plume in the first place as `PlumeGraph` 
    is an in-memory representation of results after all. For industrial sized programs it is
    recommended that one starts with `IDriver::getProgramStructure` or `IDriver::getMethod`
    as these have a smaller memory footprint and can be used in conjunction with 
    `IDriver::getNeighbours` to traverse the call graph.
* **Slower than Querying the Database Directly**. Using `PlumeGraph` with `IDriver` methods 
    will result in many database queries and a lot of serializing and deserializing 
    overhead. Directly querying the graph database for results will be much faster.

## Example

Let us refer back to the basic graph produced by the source code example, `Test1.java`, in 
[Using the Extractor](extracting-cpg.md). We can create a `PlumeGraph` representation of this
result by calling the `IDriver::getWholeGraph` method on the driver connected to the 
underlying graph database:

```kotlin
>>> val plumeGraph = driver.getWholeGraph()
>>> println(plumeGraph)
PlumeGraph(vertices:35, edges:58)
```

We can do this easily as `Test1.java` is a very small program. The edges are mapped by edge 
label keys whose values are maps of source vertex keys with their target vertices as a set. 
Let us find the main vertex and obtain its neighbouring vertices:

```kotlin
>>> val mainMethodV = plumeGraph.vertices()
            .filterIsInstance<MethodVertex>()
            .first { it.fullName == "extractor_tests.Test1.main" }
>>> println(mainMethodV)
MethodVertex(name='main', fullName='extractor_tests.Test1.main', signature='void main(java.lang.String[])')
>>> println(plumeGraph.edgesIn(mainMethodV))
{AST=[TypeDeclVertex(name='Test1', fullName='extractor_tests.Test1', typeDeclFullName='Test1')]}
>>> println(plumeGraph.edgesOut(mainMethodV))
{SOURCE_FILE=[FileVertex(name='Test1', order=32)], CFG=[BlockVertex(name='BODY', typeFullName='void')], AST=[LocalVertex(code='int c', typeFullName='int', lineNumber=5), MethodReturnVertex(name='RETURN', typeFullName='void', evaluationStrategy=BY_VALUE), ModifierVertex(name=STATIC, order=12), ModifierVertex(name=PUBLIC, order=13), LocalVertex(code='byte b', typeFullName='byte', lineNumber=5), LocalVertex(code='byte a', typeFullName='byte', lineNumber=5), BlockVertex(name='BODY', typeFullName='void'), MethodParameterInVertex(code='java.lang.String[] args', evaluationStrategy=BY_REFERENCE, typeFullName='java.lang.String[]', lineNumber=5)]}
```
When using `PlumeGraph::edgesIn` and `PlumeGraph::edgesOut`, the result is a map categorized 
by edge label keys and the values are sets of the incoming or outgoing vertices 
respectively.