# JUnit Platform OpenTelemetry Tracing
Implementations of JUnit Platform extension points for tracing of JUnit test suites with OpenTelemetry


## Inspirations
- [rakyll/go-test-trace](https://github.com/rakyll/go-test-trace)
- [junit-team/junit5/junit-platform-jfr](https://github.com/junit-team/junit5/tree/main/junit-platform-jfr)


## Running the example

This project includes an example test suite for you to try it out and see how it 
works. This test suite expects an OpenTelemetry Collector to be listening for OLTP
GRPC traces on `localhost:4317`. Collectors are responsible for running "near" instrumented
code, receiving and processing the traces, and exporting them to a human-readable destination.

In order to make this easy, this project ships with two reference collector configurations.

### Export to Honeycomb.io
If you have a honeycomb.io account, use the following command to start an OpenTelemetry collector 
docker container that exports data to Honeycomb. This command expects the `HONEYCOMB_TEAM_KEY`
environment variable to be set on your host. Alternatively, replace `${X_HONEYCOMB_TEAM}` in 
[honeycomb-otel-config.yaml](./honeycomb-otel-config.yaml) with your Honeycomb API key.

```bash
$ docker run --rm -d -p 4317:4317 -v $(pwd)/honeycomb-otel-config.yaml:/etc/otel/config.yaml \
  --env X_HONEYCOMB_TEAM=$HONEYCOMB_TEAM_KEY \
  otel/opentelemetry-collector-contrib:latest
```

### Export to logger

If you just want to see what kind of data is generated by the example test suite, use the following
command to start and OpenTelemetry collector docker container that exports data to the container's 
logger.

```bash
$ docker run --rm -d -p 4317:4317 -v $(pwd)/logging-otel-config.yaml:/etc/otel/config.yaml \
  otel/opentelemetry-collector-contrib:latest
```

### Export to a different address
If you have an OpenTelemetry collector listening on a different address, simply specify the 
address using one of the mechanisms exposed by the [OpenTelemetry Java SDK Autoconfigure Extension].

Example:
```bash
OTEL_EXPORTER_OTLP_ENDPOINT=http://my.customm.host:4317 ./gradlew :example:test
```