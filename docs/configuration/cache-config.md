# Cache Configuration

During and between subsequent projections, Plume makes use of a cache layer between the database
to reduce the number of queries. In order not to waste memory on the cache, there is a configurable
limit one can specify for this:

```kotlin
import io.github.plume.oss.options.CacheOptions

CacheOptions.cacheSize = 10000L
```

The default is 10 000 and this refers to the number of `NewNodeBuilder` objects are stored. This
total is divided as follows:

| Vertex Type     | Percentage Cache |
| --------------- | ---------------- |
| TYPE_DECL       | 26%              |
| TYPE            | 26%              |
| FILE            | 24%              |
| NAMESPACE_BLOCK | 24%              |

The slight skew is because external libraries share files and namespaces so the growth is skewed 
towards type information and this is confirmed in benchmarks on large open-source applications.

The caching library used is [cache2k](https://cache2k.org/).