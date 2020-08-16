# Introduction

## What is Plume?

Plume is a static analysis library supported by a graph database backend. Given an application compiled to 
JVM bytecode, the library will analyze the bytecode using a call graph produced using [Soot](https://soot-oss.github.io/soot).
This call graph is then converted into what is called a [code-property graph](https://scholar.google.com/scholar_url?url=https://ieeexplore.ieee.org/abstract/document/6956589/&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=12886570087564421680&ei=b405X9CuKqiBy9YP_Y27yAc&scisig=AAGBfm3j_-cCDAxDL775VnqZMs9K7suiYw) which is simply a combination of an abstract syntax tree (AST),
control flow graph (CFG), and program dependence graph (PDG). This three part graph is then persisted in a 
supported graph database and can be queried and analyzed via the library or queried against the database natively.

Note that plume is still under development and does not have an official release yet.

## Benefits of using Plume

Plume is an open-source, Kotlin project which provides a type-safe interface for interacting with the graph
database based off of [ShiftLeft's](https://www.shiftleft.io/) (the primary maintainers of Fabian Yamaguchi's
[Joern](https://github.com/ShiftLeftSecurity/joern)) 
[CPG schema](https://github.com/ShiftLeftSecurity/codepropertygraph/blob/master/codepropertygraph/src/main/resources/schemas/base.json).
Plume is broken up into three sub-libraries so that a user can use whichever part is applicable and separate
concerns e.g. separate the application which extracts the CPG vs the application that analyzes it. Since 
Kotlin is interoperable with Java, one should not have any difficulty incorporating Plume in their Java projects.

The idea of storing the CPG in a graph database is so that the analysis can be done incrementally (one does not
have to regenerate a graph everytime one wishes to perform analysis and results can be persisted), updates
done partially (if one method changes, only that subtree is regenerated), and be scalable for extremely 
large applications due to how the number of nodes and edges scale for the CPG. Plume supports multiple graph 
databases to allow for a user to pick and choose based on their currrent stack or what kind of processing they
require.

## General Plume Benefits

* Choice of graph database: inmemory to dedicated, open-source to enterprise, single node to clustered.
* CPG schema strictly enforced in the Plume driver.
* Handles very large code property graphs.
* Analysis can be done multi-machine clusters and incrementally.
* Simple to use interface.
* Open source under the liberal [Apache 2 license](https://en.wikipedia.org/wiki/Apache_License).

## Plans for Plume

* Extract an interprocedural CPG given JVM bytecode.
* To perform alias-aware type-state analysis using [synchronized pushdown systems](https://scholar.google.com/scholar_url?url=https://dl.acm.org/doi/abs/10.1145/3290361&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=15546365361660080180&ei=N5Q5X9XBF_SSy9YPxMG2yAE&scisig=AAGBfm3MtiLeyMfSj5gXy1bzeuLCewQ9-A). 
* Solve IDFS or IDE problems.
