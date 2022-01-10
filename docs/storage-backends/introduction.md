# Introduction

Plume's data storage layer is pluggable. Each graph database is called a storage backend and allows
one to be flexible in which solution one requires.

This section describes the use case and configuration of each storage backend with examples of how
to get started. A driver for a particular storage backend can be obtained by calling the correct
constructor from classes contained in the `com.github.plume.oss.drivers` package.

```scala
import com.github.plume.oss.drivers.TigerGraphDriver

val driver = new TigerGraphDriver(
    hostname = "localhost",
    restPpPort = 9000,
    gsqlPort = 14240,
    username = "tigergraph",
    password = "tigergraph",
)
```

All Plume drivers implement the
[`AutoCloseable`](https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html) interface
and can thus be used within a
[scala.util.Using](https://www.scala-lang.org/api/2.13.3/scala/util/Using$.html) block.