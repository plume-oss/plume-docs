# Using the Extractor

All of the following graphs are constructed with the TinkerGraph driver and visualized with
[Cytoscape](https://cytoscape.org/). The code being projected is a simplified process taken from the
extractor unit tests - specifically the
[`ExtractorTest`](https://github.com/plume-oss/plume-extractor/blob/develop/src/test/kotlin/za/ac/sun/plume/ExtractorTest.kt)
suite.

## Using the Extractor

The extractor uses Soot in order to extract the code-property graph from JVM bytecode. The extractor
is constructed by accepting an `IDriver` with which it will build the CPG and a `File` which acts as
the target directory of the source files.

```kotlin
// The driver is what the extractor is going to use to build the graph with an determine
// which database is being written to. This will automatically connect in the extractor.
val driver = DriverFactory(GraphDatabase.TINKER_GRAPH) as TinkerGraphDriver
// The above to variables are the constructor arguments
val extractor = Extractor(driver)
```

Once we have a correctly configured driver, we can then load our classes:

```kotlin
// Load files and add them to the set of files
extractor.load(File("File1.java"))
extractor.load(File("File2.java"))
// or
listOf(File("File1.java"), File("File2.java")).forEach(extractor::load)

// Project all loaded files a remove them from the queue
extractor.project()
```

Once `project()` is called, all loaded classes will be projected to the storage backend as defined
by the given `IDriver`.

**Note**: Due to the way Plume detected for changes, all application classes need to be loaded for
each subsequent projection otherwise missing classes will be detected as removed. For more details
on this, see the [incremental updates](./incremental.md) page.

## Extracting from a Source or Class File

One can extract a CPG from a source or class file using `load()`. If using a source file, the class
file is then compiled from the source file using the installed JDK. If the class file is compiled
without debugging info (`-g` flag) then all variable names will use placeholder names.

```kotlin
val targetFile = File("extractor_tests/Test1.java") # or Test1.class
val driver = DriverFactory(GraphDatabase.TINKER_GRAPH) as TinkerGraphDriver
driver.use {
    val extractor = Extractor(it)
    extractor.load(targetFile)
    extractor.project()
    it.exportGraph("graph.xml")
}
```

### Source Code

=== "extractor_tests/Test1.java"
    ```java
    package extractor_tests;

    public class Test1 {

        public static void main(String[] args) {
            int a = 3;
            int b = 2;
            int c = a + b;
        }

    }
    ```

### Code Property Graph

![Directory Graph](../assets/images/plume-basics/extracting-cpg/src-graph.png)

## Extracting from a Directory

One can extract all source and class files from a given directory using `load()`. The same is
applicable with a JAR file as it will simply be unzipped to a temporary directory where all
supported files will be loaded from.

```kotlin
val targetDirectory = File("extractor_tests/dir_test") # or /path/to/target/file.jar
val driver = DriverFactory(GraphDatabase.TINKER_GRAPH) as TinkerGraphDriver
driver.use {
    val extractor = Extractor(it)
    extractor.load(targetDirectory)
    extractor.project()
    it.exportGraph("graph.xml")
}
```

### Source Code

=== "extractor_tests/dir_test/Dir1.java"
    ```java
    package dir_test;

    public class Dir1 {

        public static void main(String[] args) {
            int a = 0;
            a += 1;
        }

    }
    ```

=== "extractor_tests/dir_test/pack/Dir2.java"
    ```java
    package dir_test.pack;

    public class Dir2 {

        public static void main(String[] args) {
            int a = 0;
            a -= 1;
        }

    }
    ```

### Code Property Graph

![Directory Graph](../assets/images/plume-basics/extracting-cpg/dir-graph.png)
