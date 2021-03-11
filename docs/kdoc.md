The documentation for the API is written using KDoc and generated with Orchid + Dokka. These
docs can be found [here](https://plume-oss.github.io/plume/).

Plume uses logging fascades for logging. The implementation used in testing is Log4j2. Here are the
logging dependencies:

```groovy
    // Logging
    compile "org.apache.logging.log4j:log4j-api:$log4jVersion"
    compile "org.apache.logging.log4j:log4j-core:$log4jVersion"
    compile "org.apache.logging.log4j:log4j-slf4j-impl:$log4jVersion"
    compile "org.slf4j:jul-to-slf4j:$slf4jVersion" // JUL bridge
    compile "org.slf4j:jcl-over-slf4j:$slf4jVersion" // Apache Commons Logging (JCL) bridge
    compile "org.slf4j:log4j-over-slf4j:$slf4jVersion" // log4j1.2 bridge
```