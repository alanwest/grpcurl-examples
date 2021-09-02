# Send stuff and things with grpcurl

OpenTelemetry Protos are the ones [used by the collector at this point in time](https://github.com/open-telemetry/opentelemetry-collector/tree/0594aa1ade95c33443c0bd036f77301d3a8164e4/model/internal).

That is I got 'em like so:
```shell
git clone git@github.com:open-telemetry/opentelemetry-proto.git
cd opentelemetry-proto
git checkout 8672494217bfc858e2a82a4e8c623d4a5530473a
```

## Send a trace payload
```shell
cat payload_trace.protobuf.json | \
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
cat payload_metrics.protobuf.json | \
grpcurl \
    -d @ \
    -proto protos/opentelemetry/proto/collector/metrics/v1/metrics_service.proto \
    -import-path protos\
    -vv \
    -H "api-key: ${NEW_RELIC_API_KEY}" \
    ${OTEL_COLLECTOR_ENDPOINT} \
    opentelemetry.proto.collector.metrics.v1.MetricsService/Export
```

## Send a log payload
```shell
cat payload_logs.protobuf.json | \
grpcurl \
    -d @ \
    -proto protos/opentelemetry/proto/collector/logs/v1/logs_service.proto \
    -import-path protos\
    -vv \
    -H "api-key: ${NEW_RELIC_API_KEY}" \
    ${OTEL_COLLECTOR_ENDPOINT} \
    opentelemetry.proto.collector.logs.v1.LogsService/Export
```