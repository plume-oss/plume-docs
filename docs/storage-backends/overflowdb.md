# OverflowDB

![OverflowDB Logo](../assets/images/databases/overflowdb.png){: align=right style="width:150px" }

>
ShiftLeft's OverflowDB is an in-memory graph database, which implements a swapping mechanism to deal
with large graphs.
>
> — [ShiftLeft GitHub](https://github.com/ShiftLeftSecurity/overflowdb)

## Driver Configuration and Usage

OverflowDb is an in-memory storage backend option with an overflow to disk mechanism when memory
starts filling up. This can be obtained as follows:
```scala
val driver = new OverflowDbDriver(
      // To specifies if OverflowDb should write to disk when memory is constrained
      storageLocation = Some(dbOutputFile),
      // Percentage of the heap from when overflowing should begin to occur
      heapPercentageThreshold = 80, 
      // If specified, OverflowDB will measure and report serialization/deserialization timing averages
      serializationStatsEnabled = false,
      // Max call depth when tracking data-flows on calls to nodesReachableBy
      mallCallDepth = 2
    )
```

This exported graph can then be loaded back into the driver by specifying the same path under `storageLocation`.

## Benefits of using OverflowDB

### Low memory footprint

OverflowDB enforces a strict schema and uses implicit/virtual edges to help lower the memory
footprint. When memory does become limited, OverflowDB will start serializing instances to disk and
out of the heap to avoid `OutOfMemoryError`.

By making use of a strict schema using domain classes, memory isn't wasted on many `Map` instances
as is the case with [TinkerGraph](tinkergraph.md).

### High performance

Due to the whole graph being in-memory the performance is fast (until swapping to disk occurs).

### Production

There are many use cases where a dedicated storage backend is not necessary. Here are a few
applications:

* For analysis on smaller programs or parts of programs where its feasible to store snapshots of
  graphs in something compact such as the `bin` format.
* To be part of a DevSecOps lifecycle where the process is run on low resource containers and the
  graph itself may be disposable but the results can be stored elsewhere.
* High performance is required, but no expertise/resources available for dedicated backends.
