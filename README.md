# Send stuff over OTLP with grpcurl to New Relic

## Install grpcurl

On a mac using Homebrew
```shell
brew install grpcurl
```

More info here https://github.com/fullstorydev/grpcurl

## Set some environment variables
```shell
export NEW_RELIC_API_KEY=your_api_key_here
export OTLP_ENDPOINT=otlp.nr-data.net:4317
```

## Send a trace payload
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

## Send a metric payload
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

## Send a log payload
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

---

This repo is using the v0.11.0 OpenTelemetry Protos.

That is I got 'em like so:
```shell
git clone git@github.com:open-telemetry/opentelemetry-proto.git
cd opentelemetry-proto
git checkout v0.11.0
```