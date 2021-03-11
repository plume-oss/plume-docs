# Metrics

Plume allows one to obtain metrics about how much time Plume took to perform certain tasks. This is
done by probing `System.nanoTime()` calls around function calls. Metric results can be obtained by
classes in the [`metrics`](https://plume-oss.github.io/plume/kotlindoc/io/github/plume/oss/metrics/)
package.

## Timers

[`PlumeTimer`](https://plume-oss.github.io/plume/kotlindoc/io/github/plume/oss/metrics/plumetimer/)
measures time spent doing certain tasks. Database related categories reflect CPU time and may not
match up with wall clock time. Other categories reflect wall clock time. An example of obtaining
recorded times:

```kotlin
> Extractor.load(f).project()
> println(PlumeTimer.getTimes())
{ 
    COMPILING_AND_UNPACKING=879852492,
    SOOT=955148167,
    DATABASE_WRITE=142191831,
    DATABASE_READ=137036982,
    SCPG_PASSES=107799476,
    BASE_CPG_BUILDING=472255329
}
```

`PlumeTimer::getTimes` returns `Map<ExtractorTimeKey, Long>` where the values are in nanoseconds.