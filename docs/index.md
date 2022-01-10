![Plume Banner](./assets/images/logo-text.png){: style="margin-left: 10%; margin-right: auto; width:80%" }

# Introduction

## What is Plume?

Plume is a **JVM bytecode** to **code property graph** library supported by a
**graph database** storage backend. Given an application compiled to JVM
bytecode, the library will analyze the bytecode using an interprocedural
control-flow graph (ICFG) produced using [Soot](https://soot-oss.github.io/soot). This
ICFG is then converted into what is called a [code property
graph](https://scholar.google.com/scholar_url?url=https://ieeexplore.ieee.org/abstract/document/6956589/&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=12886570087564421680&ei=b405X9CuKqiBy9YP_Y27yAc&scisig=AAGBfm3j_-cCDAxDL775VnqZMs9K7suiYw)
which, at the base level, is simply a combination of an abstract syntax tree
(AST), control flow graph (CFG), and program dependence graph (PDG). This three
part graph is then persisted in a supported graph database and can be queried
and analyzed via tools such as [Joern](https://joern.io/).

Note that Plume produces CPGs ideally used for dataflow tracking. The AST's from
using Jimple as the IR lead to fairly degenerated ASTs which make Plume
unsuitable for AST analysis.

Plume can be downloaded via 
[![Download](https://jitpack.io/v/plume-oss/plume.svg)](https://jitpack.io/#plume-oss/plume).

!!! note "Optimized in Joern as jimple2cpg"

    Plume is the original implementation of [jimple2cpg](https://github.com/joernio/joern/tree/master/joern-cli/frontends/jimple2cpg). The frontend on the Joern project is optimized
    around OverflowDB and is much more lightweight. This is project focuses on experimenting with
    incremental dataflow analysis and comparing database backend performance.

## Benefits of using Plume

Plume is an open-source Scala project which provides a type-safe interface for
interacting with a graph database constructed using
[ShiftLeft's](https://www.shiftleft.io/) (the primary maintainers of Fabian
Yamaguchi's [Joern](https://github.com/ShiftLeftSecurity/joern)) [CPG
schema](https://github.com/ShiftLeftSecurity/codepropertygraph/blob/master/schema/src/main/resources/schemas/base.json).

The idea of storing the CPG in a graph database is motivated by the observation
that this approach will allow the analysis to be done incrementally (one does
not have to regenerate a graph everytime one wishes to perform analysis and
results can be persisted), updates done partially (if one method changes, only
that subtree is regenerated), and be scalable for large applications due to how
the number of nodes and edges scale for the CPG. Plume supports multiple graph
databases so that developers can select a graph database based on their software
stack and processing requirements.

Plume provides JVM bytecode support for the Joern project which means that one
can use Joern to perform analysis on Plume generated CPGs.

## Supported Languages

Since Plume analyzes JVM bytecode, if a language is able to compile to JVM
bytecode then Plume can accept it. Plume no longer supports compiling Java source
code to bytecode automatically.

Simply load the respective `.jar` or `.class` files (or a directory or JAR file
containing either) using the [Jimple2Cpg::createCpg](https://plume-oss.github.io/plume/latest/api/com/github/plume/oss/Jimple2Cpg.html)
method accordingly.

## General Plume Benefits

* Choice of graph database: in-memory to dedicated, open-source to enterprise,
  single node to multi-machine clustered.
* CPG schema strictly enforced in the Plume driver using
  [codepropertygraph](https://github.com/ShiftLeftSecurity/codepropertygraph)
  domain classes.
* Handles very large code property graphs.
* Analysis can be done incrementally.
* Simple to use interface.
* Open source under the liberal [Apache 2
  license](https://en.wikipedia.org/wiki/Apache_License).

## Sponsored by

![Amazon Science](https://assets.amazon.science/dims4/default/ce84994/2147483647/strip/true/crop/1200x630+0+0/resize/1200x630!/quality/90/?url=http%3A%2F%2Famazon-topics-brightspot.s3.amazonaws.com%2Fscience%2F32%2F80%2Fc230480c4f60a534bc077755bae7%2Famazon-science-og-image-squid.png)

## Where does the name come from?

The word "plume" can describe a plume of smoke, dust, or fire rising into the
air in a column in large quantities. Due to the fact that Plume leverages Soot
to construct the code property graph, a colleague of mine, [Lauren
Hayward](https://www.linkedin.com/in/lauren-hayward-8ba853199/), suggested it be
called Plume as homage and so it was named. 
