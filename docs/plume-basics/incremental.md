# Incremental Updates

Part of why Plume stores the CPG in a backend database is to allow for long term storage of a large
initial projection and only updating the parts of the graph associated with code updates going
forward.

## Intermediate Representation

Whenever code is changed, simply re-add the artifact via the `Jimple2Cpg::createCpg` function. There
are hashes stored in the graph database to detect changes on a per class level. Due to this, the
whole artifact needs to be loaded each time e.g. re-build and re-load a JAR. Any missing classes
will be detected as removed from the application and trimmed from the underlying storage backend.

Roughly, the algorithm reacts to changes in the following order:

1. Load each class into Plume, calculating the hash of each.
    1. Remove any missing classes from the database. These are classes in the database but not
       loaded into Plume. This removes that class' type, program, member, and method information.
    1. Classes with hashes differing to the files in the database are marked for update.
    1. Classes with matching hashes to the files in the database are removed from the queue.
1. Remaining classes which need to be added or re-added are then generated.

The hash used is XXH32 from [xxHash](http://cyan4973.github.io/xxHash/) which is a fast and
non-cryptographic hash.

## Data-Flow Representation

When a call to `nodesReachableBy` is called, data-flow paths are calculated on the fly and used to
determine whether a node is reachable via a control flow path or not. These data-flow paths are then
stored in a cache for future queries.

During a separate session, these paths are then loaded back into the data-flow engine cache.

!!! note "Future Feature"

    Loading data-flow paths is a feature scheduled for v1.1.0
