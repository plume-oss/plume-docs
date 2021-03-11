# Extractor Configuration

The extractor has options to configure how the graph is generated. This should be configured before
`Extractor::project()` is called to have an effect. These options are configured via the
`ExtractorOptions` class.

## Call Graph Configuration Options

Plume makes use of Soot to construct the call graph using two of its points-to analysis algorithms.
By default CHA is used but one can opt not to use any algorithm which will
simply generate a intraprocedural CPG.

### None

This option will skip any call graph construction and not generate any `REF` edges between
`CallVertex` vertices and `MethodVertex` vertices.

```kotlin
ExtractorOptions.callGraphAlg = ExtractorOptions.CallGraphAlg.NONE
```

### Class Heirarchy Analysis (CHA)

This is the default option will generate an imprecise call graph. CHA simply assumes that every
variable might point to every other variable which is conservatively sound but not terribly accurate
and may overapproximate. The pro is that this algorithm is fast.

```kotlin
// Configuration not necessary this is the default
ExtractorOptions.callGraphAlg = ExtractorOptions.CallGraphAlg.CHA 
```

### Soot Pointer Analysis Research Kit (SPARK)

SPARK is much more precise than CHA and provides a rich set of options to configure. SPARK can be
configured to use a naïve iterative algorithm or more efficient worklist algorithm. SPARK will take
longer to construct a call graph than CHA but will produce a smaller and more precise call graph
than CHA.

```kotlin
ExtractorOptions.callGraphAlg = ExtractorOptions.CallGraphAlg.SPARK
```

Plume makes the following configuration for SPARK by default via a map:

```kotlin
sparkOpts["verbose"] = "false"
sparkOpts["propagator"] = "worklist"
sparkOpts["simple-edges-bidirectional"] = "false"
sparkOpts["on-fly-cg"] = "true"
sparkOpts["set-impl"] = "double"
sparkOpts["double-set-old"] = "hybrid"
sparkOpts["double-set-new"] = "hybrid"
sparkOpts["enabled"] = "true"
```

For more information on these options, check out the Soot 
[options documentation](https://soot-build.cs.uni-paderborn.de/public/origin/develop/soot/soot-develop/options/soot_options.htm#phase_5_2).