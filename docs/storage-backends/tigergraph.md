# TigerGraph

![TigerGraph Logo](../assets/images/databases/tigergraph.png){: align=right style="height:150px;width:150px" }

>TigerGraph is the world’s fastest graph analytics platform designed to unleash the power of
>interconnected data for deeper insights and better outcomes. TigerGraph fulfills the true promise
>and benefits of the graph platform by tackling the toughest data challenges in real time, no matter
>how large or complex the dataset. TigerGraph supports applications such as IoT, AI and machine
>learning to make sense of ever-changing big data.
>
> —  [TigerGraph About](https://www.tigergraph.com/about/)

TigerGraph is a native parallel graph with a C++ codebase. Plume communicates with TigerGraph via a
REST interface which can be secured with HTTPS and an authentication token. Since TigerGraph uses a
schema there is additional setup beforehand but this is made simple by making use of Docker.

## Driver Configuration and Usage

Since TigerGraph makes use of REST, it only requires server information and (optionally) an
authentication e.g. when using [TigerGraph Cloud](https://www.tigergraph.com/cloud/). By default the hostname will be 127.0.0.1, port 9000 and protocol HTTP. The driver is
created as follows:
```scala
val driver = new TigerGraphDriver(
      hostname = "localhost",
      restPpPort = 9000,
      gsqlPort = 14240,
      username = "tigergraph",
      password = "tigergraph",
    )
```

Plume will make use of a graph called "cpg". This schema can be built by calling:
```scala
driver.buildSchema()
```
This wraps around the `GsqlCli` JAR. This JAR makes a call to `System::exit` which is temporarily
overriden until the calls to `GsqlCli` are done so you may notice some exceptions thrown on 
`stdout`.

## Ideal Use Case

TigerGraph is fast and efficient both in terms of storage and memory consumption. TigerGraph
provides a developer and enterprise edition but one can easily get started with a free [TigerGraph
Cloud](https://www.tigergraph.com/cloud/) instance if resources are limited. Like TinkerGraph,
TigerGraph is ideal for graph exploration as it comes with a built-in graph visualizer using
GraphStudio.

### Benefits

- Free to use for development and the TigerGraph Cloud free tier instance comes with a generous
  resource allocation.
- Distributed capabilities and ACID compliant.
- Fast execution of parallel graph algorithms.
- Real-time capability for streaming updates and inserts using REST.
- Ability to unify real-time analytics with large-scale offline data processing.
- Ability to traverse hundreds of millions of vertices/edges per second per machine.
- Ability to load 50 to 150 GB of data per hour, per machine.
- Ability to stream 2B+ daily events in real-time.
- High compression rates for low resource consumption.

### Limitations

- Enforced schema requires compulsory setup to use with Plume.
