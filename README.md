# Send stuff over OTLP to New Relic

New Relic supports the OpenTelemetry Protocol (OTLP). Telemetry can be sent in
three different ways:

1. [OTLP gRPC](https://github.com/open-telemetry/opentelemetry-proto/blob/main/docs/specification.md#otlpgrpc)
2. [OTLP HTTP using the binary protobuf format](https://github.com/open-telemetry/opentelemetry-proto/blob/main/docs/specification.md#binary-protobuf-encoding)
3. [OTLP HTTP using the JSON protobuf format](https://github.com/open-telemetry/opentelemetry-proto/blob/main/docs/specification.md#binary-protobuf-encoding)

This repository uses grpcurl and curl to demonstrate sending data
over gRPC and HTTP (JSON protobuf format). While the binary protobuf
format over HTTP is also supported, it is not demonstrated here.

## Getting started

The commands in this guide require you to set the following environment
variables.

```shell
export NEW_RELIC_API_KEY=your_api_key_here
export OTLP_ENDPOINT=otlp.nr-data.net:4317
```

## Using curl

### curl: Send a trace payload

```shell
curl -vX POST https://${OTLP_ENDPOINT}/v1/traces \
    -d @payload_trace.otlp.json \
    --header "Content-Type: application/json" --header "api-key: ${NEW_RELIC_API_KEY}"
```

### curl: Send a metric payload

```shell
curl -vX POST https://${OTLP_ENDPOINT}/v1/metrics \
    -d @payload_metrics.otlp.json \
    --header "Content-Type: application/json" --header "api-key: ${NEW_RELIC_API_KEY}"
```

### curl: Send a log payload

```shell
curl -vX POST https://${OTLP_ENDPOINT}/v1/logs \
    -d @payload_logs.otlp.json \
    --header "Content-Type: application/json" --header "api-key: ${NEW_RELIC_API_KEY}"
```

### curl: Send an event payload

```shell
curl -vX POST https://${OTLP_ENDPOINT}/v1/logs \
    -d @payload_events.otlp.json \
    --header "Content-Type: application/json" --header "api-key: ${NEW_RELIC_API_KEY}"
```

## Using grpcurl

### Install grpcurl

On a mac using Homebrew

```shell
brew install grpcurl
```

More info here https://github.com/fullstorydev/grpcurl

### grpcurl: Send a trace payload

```shell
cat payload_trace.otlp.json | \
grpcurl \
    -d @ \
    -proto protos/opentelemetry/proto/collector/trace/v1/trace_service.proto \
    -import-path protos\
    -vv \
    -H "api-key: ${NEW_RELIC_API_KEY}" \
    ${OTLP_ENDPOINT} \
    opentelemetry.proto.collector.trace.v1.TraceService/Export
```

### grpcurl: Send a metric payload

```shell
cat payload_metrics.otlp.json | \
grpcurl \
    -d @ \
    -proto protos/opentelemetry/proto/collector/metrics/v1/metrics_service.proto \
    -import-path protos\
    -vv \
    -H "api-key: ${NEW_RELIC_API_KEY}" \
    ${OTLP_ENDPOINT} \
    opentelemetry.proto.collector.metrics.v1.MetricsService/Export
```

### grpcurl: Send a log payload

```shell
cat payload_logs.otlp.json | \
grpcurl \
    -d @ \
    -proto protos/opentelemetry/proto/collector/logs/v1/logs_service.proto \
    -import-path protos\
    -vv \
    -H "api-key: ${NEW_RELIC_API_KEY}" \
    ${OTLP_ENDPOINT} \
    opentelemetry.proto.collector.logs.v1.LogsService/Export
```

### grpcurl: Send an event payload

```shell
cat payload_events.otlp.json | \
grpcurl \
    -d @ \
    -proto protos/opentelemetry/proto/collector/logs/v1/logs_service.proto \
    -import-path protos\
    -vv \
    -H "api-key: ${NEW_RELIC_API_KEY}" \
    ${OTLP_ENDPOINT} \
    opentelemetry.proto.collector.logs.v1.LogsService/Export
```

---

This repo is using the v1.3.2 OpenTelemetry Protos.

That is I got 'em like so:

```shell
git clone git@github.com:open-telemetry/opentelemetry-proto.git
cd opentelemetry-proto
git checkout v1.3.2
```
