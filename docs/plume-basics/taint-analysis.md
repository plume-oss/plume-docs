# Taint Analysis

One can do basic taint analysis based on simple reachability when using the `OverflowDB` driver.
This wraps around Joern's dataflow engine and adds an incremental aspect by saving the IFDS-style
paths calculated during the query. More on this can be found in the next section.

## Example

Let's refer to the example found on [Plume Examples](https://github.com/plume-oss/plume-examples).
Let's look at two class files where one calls the other:

```java
public class Foo {

    public static void main(String[] args) {
        var a = 1;
        var b = 2;
        var c = Bar.add(a, b);
        System.out.println(c);
    }

}
```

```java
public class Bar {

    public static int add(int x, int y) {
        return x + y;
    }

}
```

Using `OverflowDbDriver::nodesReachableBy` one can find which sources will reach a given sink. E.g.

```scala
def taintAnalysis(d: OverflowDbDriver): Unit = {
    import io.shiftleft.semanticcpg.language._
    import io.shiftleft.codepropertygraph.{Cpg => CPG}

    println("Finding data flows from source identifiers with the name " +
        "'a' sinking at methods with names 'add'")
    val cpg = CPG(d.cpg.graph)
    d.nodesReachableBy(cpg.identifier("a"), cpg.call("add"))
        .map { n => s"${n.label}: ${n.property("CODE")} @ line ${n.property("LINE_NUMBER")}" }
        .foreach(println(_))
}
```

The code above will return something like:
```
Finding data flows from source identifiers with the name 'a' sinking at methods with names 'add'
IDENTIFIER: a @ line 4
IDENTIFIER: a @ line 6
```

Note that the signature is: `nodesReachableBy(<source>, <sink>)`. For more on how to use the
`nodesReachableBy` call refer to the [Joern Docs](https://docs.joern.io/cpgql/data-flow-steps).