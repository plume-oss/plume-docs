# Dependencies

Until the first official release, the Plume JARs will need to be added to the classpath or build script
from a directory and not from a repository such as jCenter or Maven Central. The following assumes
you have the respective jar e.g. `plume-driver-X.X.X.jar` in a folder called `lib` on the project root.

The JARs can be downloaded from the respective Plume repository.


=== "Driver"
    Gradle
    ```groovy
    repositories {
        flatDir {
            dirs 'lib'
        }
    }
    dependencies {
        implementation name: 'plume-driver-X.X.X'
    }
    ```
    Maven
    ```mxml
    <dependency>
        <groupId>za.ac.sun.plume</groupId>
        <artifactId>plume-driver</artifactId>
        <version>X.X.X</version>
        <scope>system</scope>
        <systemPath>${project.basedir}/lib/plume-driver-X.X.X.jar</systemPath>
    </dependency>
    ```

=== "Extractor"
    Gradle
    ```groovy
    repositories {
        flatDir {
            dirs 'lib'
        }
    }
    dependencies {
        implementation name: 'plume-extractor-X.X.X'
    }
    ```
    Maven
    ```mxml
    <dependency>
        <groupId>za.ac.sun.plume</groupId>
        <artifactId>plume-extractor</artifactId>
        <version>X.X.X</version>
        <scope>system</scope>
        <systemPath>${project.basedir}/lib/plume-extractor-X.X.X.jar</systemPath>
    </dependency>
    ```

### General Dependencies

All Plume repositories make use of the following dependencies.

=== "Gradle"
    ```groovy
    dependencies {
        implementation 'org.apache.logging.log4j:log4j-core:2.8.2 '
        implementation 'org.apache.logging.log4j:log4j-slf4j-impl:2.8.2'
        implementation 'org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.3.72'
        implementation 'org.jetbrains.kotlin:kotlin-reflect:1.3.72'
    }
    ```
=== "Maven"
    ```mxml
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.8.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-slf4j-impl</artifactId>
        <version>2.8.2</version>
    </dependency>
    <dependency>
        <groupId>org.jetbrains.kotlin</groupId>
        <artifactId>kotlin-stdlib-jdk8</artifactId>
        <version>1.3.72</version>
    </dependency>
    <dependency>
        <groupId>org.jetbrains.kotlin</groupId>
        <artifactId>kotlin-reflect</artifactId>
        <version>1.3.72</version>
    </dependency>
    ```

### Driver Dependencies

Based on the storage backend being used, `plume-driver` will need to be paired with 
the necessary dependencies required to communicate with the database.

=== "TinkerGraph"
    Gradle
    ```groovy
    dependencies {
        implementation 'org.apache.tinkerpop:gremlin-core:3.4.6'
        implementation 'org.apache.tinkerpop:tinkergraph-gremlin:3.4.6'
    }
    ```
    Maven
    ```mxml
    <dependency>
        <groupId>org.apache.tinkerpop</groupId>
        <artifactId>gremlin-core</artifactId>
        <version>3.4.8</version>
    </dependency>
    <dependency>
        <groupId>org.apache.tinkerpop</groupId>
        <artifactId>tinkergraph-gremlin</artifactId>
        <version>3.4.8</version>
    </dependency>
    ```

=== "JanusGraph"
    Gradle
    ```groovy
    dependencies {
        implementation 'org.apache.tinkerpop:gremlin-core:3.4.6'
        implementation 'org.janusgraph:janusgraph-driver:0.5.2'
    }
    ```
    Maven
    ```mxml
    <dependency>
        <groupId>org.apache.tinkerpop</groupId>
        <artifactId>gremlin-core</artifactId>
        <version>3.4.8</version>
    </dependency>
    <dependency>
        <groupId>org.janusgraph</groupId>
        <artifactId>janusgraph-driver</artifactId>
        <version>0.5.2</version>
    </dependency>
    ```

=== "TigerGraph"
    Gradle
    ```groovy
    dependencies {
        implementation 'com.fasterxml.jackson.core:jackson-databind:2.11.2'
        implementation 'khttp:khttp:1.0.0'
    }
    ```
    Maven
    ```mxml
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.11.2</version>
    </dependency>
    <dependency>
        <groupId>khttp</groupId>
        <artifactId>khttp</artifactId>
        <version>1.0.0</version>
    </dependency>
    ```

### Extractor Dependencies

The only additional dependencies required are the Plume driver and Soot:

=== "Gradle"
    ```groovy
    repositories {
        mavenCentral()
        flatDir {
            dirs 'lib'
        }
    }
    dependencies {
        implementation 'org.soot-oss:soot:4.2.1'
        implementation name: 'plume-driver-X.X.X'
    }
    ```
=== "Maven"
    ```mxml
    <dependency>
        <groupId>org.soot-oss</groupId>
        <artifactId>soot</artifactId>
        <version>4.2.1</version>
    </dependency>
    <dependency>
        <groupId>za.ac.sun.plume</groupId>
        <artifactId>plume-driver</artifactId>
        <version>X.X.X</version>
        <scope>system</scope>
        <systemPath>${project.basedir}/lib/plume-driver-X.X.X.jar</systemPath>
    </dependency>
    ```