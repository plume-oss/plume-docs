# Introduction

Plume's data storage layer is pluggable. Each graph database is called a storage backend and allows
one to be flexible in which solution one requires.

This section describes the use case and configuration of each storage backend with examples of how
to get started. A driver for a particular storage backend can be obtained by using the
`DriverFactory` given the `GraphDatabase` enum. e.g.

```kotlin
val driver = DriverFactory(GraphDatabase.JANUS_GRAPH) as JanusGraphDriver
```

All Plume drivers implement the
[`AutoCloseable`](https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html) interface
and can thus be used within
[try-with-resource](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)
statements in Java or the [`use`](https://www.baeldung.com/kotlin-try-with-resources) function in
Kotlin.