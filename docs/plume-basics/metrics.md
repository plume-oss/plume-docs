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
    COMPILING_AND_UNPACKING=1356361863,
    SOOT=1358402417,
    BASE_CPG_BUILDING=247750313,
    PROGRAM_STRUCTURE_BUILDING=79187694,
    DATABASE_WRITE=77964642,
    DATABASE_READ=53746571,
    DATA_FLOW_PASS=71401839
}
```

`PlumeTimer::getTimes` returns `Map<ExtractorTimeKey, Long>` where the values are in nanoseconds.