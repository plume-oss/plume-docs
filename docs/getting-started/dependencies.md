# Dependencies

For version numbers, consult Plume's `gradle.properties` file
[here](https://github.com/plume-oss/plume/blob/develop/gradle.properties).

## Core Dependencies

These dependencies cover what Plume needs before extraction and database communcation

=== "Gradle"
    ```groovy
    dependencies {
        implementation 'org.apache.logging.log4j:log4j-core'
        implementation 'org.apache.logging.log4j:log4j-slf4j-impl'
        implementation 'org.jetbrains.kotlin:kotlin-stdlib-jdk8'
        implementation 'org.jetbrains.kotlin:kotlin-reflect'
        implementation "io.shiftleft:codepropertygraph_2.13"
        implementation "io.shiftleft:semanticcpg_2.13:"
    }
    ```
=== "Maven"
    ```mxml
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-slf4j-impl</artifactId>
    </dependency>
    <dependency>
        <groupId>org.jetbrains.kotlin</groupId>
        <artifactId>kotlin-stdlib-jdk8</artifactId>
    </dependency>
    <dependency>
        <groupId>org.jetbrains.kotlin</groupId>
        <artifactId>kotlin-reflect</artifactId>
    </dependency>
    <dependency>
        <groupId>io.shiftleft</groupId>
        <artifactId>codepropertygraph_2.13</artifactId>
    </dependency>
    <dependency>
        <groupId>io.shiftleft</groupId>
        <artifactId>codepropertygraph_2.13</artifactId>
    </dependency>
    ```

## Driver Dependencies

Based on the storage backend being used, `IDriver` classes will need to be paired with 
the necessary dependencies required to communicate with the database. OverflowDB is
inherently supported since it is a core dependency used as a cache for subgraph processing.

=== "TinkerGraph"
    Gradle
    ```groovy
    dependencies {
        implementation 'org.apache.tinkerpop:gremlin-core'
        implementation 'org.apache.tinkerpop:tinkergraph-gremlin'
    }
    ```
    Maven
    ```mxml
    <dependency>
        <groupId>org.apache.tinkerpop</groupId>
        <artifactId>gremlin-core</artifactId>
    </dependency>
    <dependency>
        <groupId>org.apache.tinkerpop</groupId>
        <artifactId>tinkergraph-gremlin</artifactId>
    </dependency>
    ```

=== "JanusGraph"
    Gradle
    ```groovy
    dependencies {
        implementation 'org.apache.tinkerpop:gremlin-core'
        implementation 'org.janusgraph:janusgraph-driver'
    }
    ```
    Maven
    ```mxml
    <dependency>
        <groupId>org.apache.tinkerpop</groupId>
        <artifactId>gremlin-core</artifactId>
    </dependency>
    <dependency>
        <groupId>org.janusgraph</groupId>
        <artifactId>janusgraph-driver</artifactId>
    </dependency>
    ```

=== "TigerGraph"
    Gradle
    ```groovy
    dependencies {
        implementation 'com.fasterxml.jackson.core:jackson-databind'
        implementation 'khttp:khttp'
    }
    ```
    Maven
    ```mxml
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
    </dependency>
    <dependency>
        <groupId>khttp</groupId>
        <artifactId>khttp</artifactId>
    </dependency>
    ```

=== "Amazon Neptune"
    Gradle
    ```groovy
    dependencies {
        implementation "org.apache.tinkerpop:gremlin-core"
        implementation "org.apache.tinkerpop:gremlin-driver"
    }
    ```
    Maven
    ```mxml
    <dependency>
        <groupId>org.apache.tinkerpop</groupId>
        <artifactId>gremlin-core</artifactId>
    </dependency>
    <dependency>
        <groupId>org.apache.tinkerpop</groupId>
        <artifactId>gremlin-driver</artifactId>
    </dependency>
    ```

=== "Neo4j"
    Gradle
    ```groovy
    dependencies {
        implementation "org.neo4j.driver:neo4j-java-driver"
    }
    ```
    Maven
    ```mxml
    <dependency>
        <groupId>org.neo4j.driver</groupId>
        <artifactId>neo4j-java-driver</artifactId>
    </dependency>
    ```

## Extractor Dependencies

These dependencies used for extracting the CPG from JVM bytecode.

=== "Gradle"
    ```groovy
    dependencies {
        implementation 'org.soot-oss:soot'
        implementation 'org.lz4:lz4-java'
    }
    ```
=== "Maven"
    ```mxml
    <dependency>
        <groupId>org.soot-oss</groupId>
        <artifactId>soot</artifactId>
    </dependency>
    <dependency>
        <groupId>org.lz4</groupId>
        <artifactId>lz4-java</artifactId>
    </dependency>
    ```