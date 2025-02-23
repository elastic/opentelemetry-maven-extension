# Maven OpenTelemetry extension

Maven extension to observe Maven builds as distributed traces.

| ⚠️ WARNING: This version of the OpenTelemetry Maven Extension is deprecated in favor of the offficial [OpenTelemetry Maven Extension](https://github.com/open-telemetry/opentelemetry-java-contrib/tree/main/maven-extension) hosted by the official [open-telemetry/opentelemetry-java-contrib/](https://github.com/open-telemetry/opentelemetry-java-contrib) project |
| --- |

**Migration**

To migrate please replace "co.elastic.opentelemetry.contrib:opentelemetry-maven-extension:0.1.0-beta-5" by "io.opentelemetry.contrib:opentelemetry-maven-extension:1.7.0-alpha":

```xml
<project>
  ...
  <build>
    <extensions>
     <!-- DEPRECATED -->
      <extension>
          <groupId>co.elastic.opentelemetry.contrib</groupId>
          <artifactId>opentelemetry-maven-extension</artifactId>
          <version>0.1.0-beta-5</version>
      </extension>
      <!-- NEW ARTIFACT -->
      <extension>
          <groupId>io.opentelemetry.contrib</groupId>
          <artifactId>opentelemetry-maven-extension</artifactId>
          <version>1.7.0-alpha</version>
      </extension>
    </extensions>
  </build>
</project>
```

---



## Getting Started (Deprecated)

The Maven OpenTelemetry Extension is configured using environment variables or JVM system properties and it can be added to a build using one of the following ways:
* adding the extension jar to `${maven.home}/lib/ext`
* adding the path to the extension jar to`-Dmaven.ext.class.path`,
* adding the extension as a build extension in the `pom.xml`,
* (since Maven 3.3.1) configuring the extension in `.mvn/extensions.xml`.


### Adding the extension to the classpath

Add the Maven OpenTelemetry Extension to `${maven.home}/lib/ext` or to the classpath using `-Dmaven.ext.class.path=`.

```
mvn dependency:copy \
      -Dartifact=co.elastic.opentelemetry.contrib:opentelemetry-maven-extension:0.1.0-beta-5 \
      -DoutputAbsoluteArtifactFilename=opentelemetry-maven-extension.jar

export OTEL_EXPORTER_OTLP_ENDPOINT="http://localhost:4317"
      
mvn -Dmaven.ext.class.path=target/dependency/opentelemetry-maven-extension.jar verify
```

### Declaring the extension in the `pom.xml` file (Deprecated)

Add the Maven OpenTelemetry Extension in the `pom.xml` file:

```xml
<project>
  ...
  <build>
    <extensions>
      <extension>
          <groupId>co.elastic.opentelemetry.contrib</groupId>
          <artifactId>opentelemetry-maven-extension</artifactId>
          <version>0.1.0-beta-5</version>
      </extension>
    </extensions>
  </build>
</project>
```

```
export OTEL_EXPORTER_OTLP_ENDPOINT="http://localhost:4317"

mvn verify
```

## Configuration  (Deprecated)

The Maven OpenTelemetry Extension supports a subset of the [OpenTelemetry auto configuration environment variables and JVM system properties](https://github.com/open-telemetry/opentelemetry-java/tree/main/sdk-extensions/autoconfigure).

| System property              | Environment variable        | Description                                                               |
|------------------------------|-----------------------------|---------------------------------------------------------------------------|
| otel.exporter.otlp.endpoint  | OTEL_EXPORTER_OTLP_ENDPOINT | The OTLP traces and metrics endpoint to connect to. Must be a URL with a scheme of either `http` or `https` based on the use of TLS. Example `http://localhost:4317`.            |
| otel.exporter.otlp.headers   | OTEL_EXPORTER_OTLP_HEADERS  | Key-value pairs separated by commas to pass as request headers on OTLP trace and metrics requests.        |
| otel.exporter.otlp.timeout   | OTEL_EXPORTER_OTLP_TIMEOUT  | The maximum waiting time, in milliseconds, allowed to send each OTLP trace and metric batch. Default is `10000`.  |
| otel.resource.attributes | OTEL_RESOURCE_ATTRIBUTES | Specify resource attributes in the following format: key1=val1,key2=val2,key3=val3 |


ℹ️ The `service.name` is set by default to `maven`, it can be overwritten specifying resource atributes.


## Examples 

![](https://github.com/elastic/maven-opentelemetry-extension/raw/main/docs/images/maven-execution-trace-jaeger.png)

## Example of a distributed trace of a Jenkins pipeline executing a Maven build

Distributed trace combining the OpenTelemetry Maven Extension with the [Jenkins OpenTelemetry plugin](https://plugins.jenkins.io/opentelemetry/).

### Trace visualized with [Elastic Observability](https://www.elastic.co/observability)

![](https://raw.githubusercontent.com/cyrille-leclerc/opentelemetry-maven-extension/main/docs/images/jenkins-maven-execution-trace-elastic.png)

### Trace visualized with [Jaeger Tracing](https://www.jaegertracing.io/)

![](https://raw.githubusercontent.com/cyrille-leclerc/opentelemetry-maven-extension/main/docs/images/jenkins-maven-execution-trace-jaeger.png)

# Other CI/CD Tools supporting OpenTelemetry traces

List of other CI/CD tools that support OpenTelemetry traces and integrate with the Maven OpenTelemetry Extension creating a distributed traces providing end to end visibility.

## Jenkins OpenTelemetry Plugin

The [Jenkins OpenTelemetry Plugin](https://plugins.jenkins.io/opentelemetry/) exposes Jenkins pipelines & jobs as OpenTelemetry traces and exposes Jenkins health indicators as OpenTelemetry metrics.

## Otel CLI

The [`otel-cli`](https://github.com/equinix-labs/otel-cli) is a command line wrapper to observe the execution of a shell command as an OpenTelemetry trace.
