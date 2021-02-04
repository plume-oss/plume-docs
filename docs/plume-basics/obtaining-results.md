# Obtaining Results

The `Graph` object is an OverflowDB graph used for query results from the underlying graph database.
The `IDriver` interface provides methods such a `getWholeGraph`, `getNeighbours`, etc. which return
results in the form of the `Graph` object.

## Example

Let us refer back to the basic graph produced by the source code example, `Test1.java`, in 
[Using the Extractor](extracting-cpg.md). We can create a `Graph` representation of this
result by calling the `IDriver::getWholeGraph` method on the driver connected to the 
underlying graph database:

```kotlin
>>> val g = driver.getWholeGraph()
>>> println(g)
Graph [35 nodes]
```

We can do this easily as `Test1.java` is a very small program. The edges are mapped by edge 
label keys whose values are maps of source vertex keys with their target vertices as a set. 
Let us find the main vertex and obtain its neighbouring vertices:

```kotlin
>>> val mainMethodV = g.nodes().asSequence()
            .filterIsInstance<Method>()
            .first { it.fullName() == "extractor_tests.Test1.main" }
>>> println(mainMethodV)
io.shiftleft.codepropertygraph.generated.nodes.Method[label=METHOD; id=11]
>>> :p
println(
    g.V(mainMethodV.id()).next().inE()
        .asSequence()
        .map { "${it.outNode().label()} -(${it.label()})-> ${it.inNode().label()}" }
        .toList()
)
[TYPE_DECL -(AST)-> METHOD]
>>> :p
println(
    g.V(mainMethodV.id()).next().outE()
        .asSequence()
        .map { "${it.outNode().label()} -(${it.label()})-> ${it.inNode().label()}" }
        .toList()
)
[METHOD -(SOURCE_FILE)-> FILE, METHOD -(CFG)-> BLOCK, METHOD -(AST)-> BLOCK,
METHOD -(AST)-> METHOD_RETURN, METHOD -(AST)-> MODIFIER, METHOD -(AST)-> MODIFIER,
METHOD -(AST)-> METHOD_PARAMETER_IN, METHOD -(AST)-> LOCAL, METHOD -(AST)-> LOCAL,
METHOD -(AST)-> LOCAL]
>>> g.close()
```

**NB**: Remember to call `g.close()` when you are done using the object otherwise memory will leak into your 
JVM heap space.