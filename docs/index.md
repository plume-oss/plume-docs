# Introduction

## What is Plume?

Plume is a static analysis library supported by a graph database backend. Given an application compiled to 
JVM bytecode, the library will analyze the bytecode using a call graph produced using [Soot](https://soot-oss.github.io/soot).
This call graph is then converted into what is called a [code-property graph](https://scholar.google.com/scholar_url?url=https://ieeexplore.ieee.org/abstract/document/6956589/&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=12886570087564421680&ei=b405X9CuKqiBy9YP_Y27yAc&scisig=AAGBfm3j_-cCDAxDL775VnqZMs9K7suiYw) which is simply a combination of an abstract syntax tree (AST),
control flow graph (CFG), and program dependence graph (PDG). This three part graph is then persisted in a 
supported graph database and can be queried and analyzed via the library or queried against the database natively.

Note that Plume is still under development and does not have an official release yet.

## Benefits of using Plume

Plume is an open-source Kotlin project which provides a type-safe interface for interacting with a graph database constructed using [ShiftLeft's](https://www.shiftleft.io/) (the primary maintainers of Fabian Yamaguchi's
[Joern](https://github.com/ShiftLeftSecurity/joern)) 
[CPG schema](https://github.com/ShiftLeftSecurity/codepropertygraph/blob/master/codepropertygraph/src/main/resources/schemas/base.json).
Plume is subdivided into three sub-libraries so that a user can use whichever part is applicable and separate
concerns e.g. separate the application which extracts the CPG from the application that analyses it. Since 
Kotlin is interoperable with Java, one should not have any difficulty incorporating Plume in their Java projects.

The idea of storing the CPG in a graph database is motivated by the observation that this approach will allow the analysis to be done incrementally (one does not
have to regenerate a graph everytime one wishes to perform analysis and results can be persisted), updates
done partially (if one method changes, only that subtree is regenerated), and be scalable for 
large applications due to how the number of nodes and edges scale for the CPG. Plume supports multiple graph databases so that developers can select a graph database based on their software stack and processing requirements.

## Supported Languages

Since Plume analyzes JVM bytecode, if a language is able to compile to JVM bytecode then Plume can accept it. Plume supports compiling the following
languages to bytecode if loaded in automatically:

* Java using the system's JDK
* Python 2.7.2 using [Jython](https://www.jython.org/)
* JavaScript 1.7 using [Mozilla Rhino](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino)

Simply load the respective `.java`, `.py`, or `.js` files using the [extractor](./plume-basics/extracting-cpg.md) accordingly.

## General Plume Benefits

* Choice of graph database: in-memory to dedicated, open-source to enterprise, single node to multi-machine clustered.
* CPG schema strictly enforced in the Plume driver.
* Handles very large code property graphs.
* Analysis can be done incrementally.
* Simple to use interface.
* Open source under the liberal [Apache 2 license](https://en.wikipedia.org/wiki/Apache_License).

## Plans for Plume

* To perform alias-aware type-state analysis using [synchronized pushdown systems](https://scholar.google.com/scholar_url?url=https://dl.acm.org/doi/abs/10.1145/3290361&hl=en&sa=T&oi=gsb&ct=res&cd=0&d=15546365361660080180&ei=N5Q5X9XBF_SSy9YPxMG2yAE&scisig=AAGBfm3MtiLeyMfSj5gXy1bzeuLCewQ9-A)
* Benchmark supported graph databases in terms of response times and resource consumption during analysis
* Investigate soundness of analysis for dynamic vs static languages
* Investigate the use of GCNNs for vulnerability detection

## Where does the name come from?

The word "plume" can describe a plume of smoke, dust, or fire rising into the air in a column in large quantities. Due to the fact that Plume leverages Soot to construct
the code property graph, a colleague of mine, [Lauren Hayward](https://www.linkedin.com/in/lauren-hayward-8ba853199/), suggested it be called Plume as homage and so it was named. 
