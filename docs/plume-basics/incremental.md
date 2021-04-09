# Incremental Updates

Part of why Plume stores the CPG in a backend database is to allow for long term storage of a large
initial projection and only updating the parts of the graph associated with code updates going
forward.

Whenever code is changed, simply re-add the artifact via the `Extractor::load` function and project.
There are hashes stored in the graph database to detect changes on an artifact level, class level,
and method level. Due to this, the whole artifact needs to be loaded each time e.g. re-build and
re-load a JAR. Any missing classes will be detected as removed from the application and trimmed from
the underlying storage backend.

Roughly, the algorithm reacts to changes in the following order:

1. Hash the whole artifact, does this hash match what is in the database? If yes, abort.
1. Load each class into Plume, calculating the hash of each.
    1. Remove any missing classes from the database. These are classes in the database but not
       loaded into Plume. This removes that class' type, program, member, and method information.
    1. Classes with hashes differing to the files in the database are marked for update.
    1. Classes with matching hashes to the files in the database are removed from the queue.
1. Class structure undergoes projection:
    1. All new classes undergo full projection of type, program, member, and method information.
    1. All classes marked for update have their modifier, field, and method information scanned for
       changes:
        1. Any field or method changed will be dropped and marked for re-add.
        1. Modifiers are updated on the spot.
1. Fields undergo projection. Fields to update and add are bundled together.
1. Method heads and bodies undergo projection. Methods to update and add bundled together.

The hash used is XXH32 from [xxHash](http://cyan4973.github.io/xxHash/) which is a fast and
non-cryptographic hash.
