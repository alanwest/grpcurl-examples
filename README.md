# Send stuff and things with grpcurl

OpenTelemetry Protos are the ones [used by the collector at this point in time](https://github.com/open-telemetry/opentelemetry-collector/tree/0d747472e565e6548ebfe42a3b3205d29d9e9065/internal/data).

That is I got 'em like so:
```shell
git clone git@github.com:open-telemetry/opentelemetry-proto.git
cd opentelemetry-proto
git checkout 8ab21e9da6246e465cd9d50d405561aedef31a1e
```

## Send a trace payload
```shell
cat payload_trace.json | \
grpcurl \
    -d @ \
    -proto protos/opentelemetry/proto/collector/trace/v1/trace_service.proto \
    -import-path protos\
    -vv \
    -H "api-key: ${NEW_RELIC_API_KEY}" \
    ${OTEL_COLLECTOR_ENDPOINT} \
    opentelemetry.proto.collector.trace.v1.TraceService/Export
```

## Send a metric payload
```shell
cat payload_metrics.json | \
grpcurl \
    -d @ \
    -proto protos/opentelemetry/proto/collector/metrics/v1/metrics_service.proto \
    -import-path protos\
    -vv \
    -H "api-key: ${NEW_RELIC_API_KEY}" \
    ${OTEL_COLLECTOR_ENDPOINT} \
    opentelemetry.proto.collector.metrics.v1.MetricsService/Export
```