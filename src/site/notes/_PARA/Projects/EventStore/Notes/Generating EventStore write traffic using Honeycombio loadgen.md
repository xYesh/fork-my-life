---
{"dg-publish":true,"permalink":"/para/projects/event-store/notes/generating-event-store-write-traffic-using-honeycombio-loadgen/","tags":["Database/Clickhouse","Benchmark"]}
---



# Honeycomb Traces Generator evaluation

- [[_PARA/Projects/EventStore/Notes/Generating EventStore write traffic using Honeycombio loadgen#Commands\|Commands]]
	- [[_PARA/Projects/EventStore/Notes/Generating EventStore write traffic using Honeycombio loadgen#Commands\|basic script that generates 100 traces per second for 10 seconds]]
		- [[_PARA/Projects/EventStore/Notes/Generating EventStore write traffic using Honeycombio loadgen#basic script that generates 100 traces per second for 10 seconds\|Data in ClickHouse]]
	- [[_PARA/Projects/EventStore/Notes/Generating EventStore write traffic using Honeycombio loadgen#Commands\|Script with custom spanAttributes added to it]]
		- [[_PARA/Projects/EventStore/Notes/Generating EventStore write traffic using Honeycombio loadgen#Script with custom spanAttributes added to it\|Data in clickhouse]]
- [[_PARA/Projects/EventStore/Notes/Generating EventStore write traffic using Honeycombio loadgen#Generator details\|Generator details]]

Repo - [GitHub - honeycombio/loadgen: A flexible command-line load generator to create traces in OTel or Honeycomb formats](https://github.com/honeycombio/loadgen)

[[_PARA/Projects/EventStore/Notes/attachments/Diagram - loadgen-setup\|Diagram - loadgen-setup]]

### Commands
#### Basic Script That Generates 100 Traces per Second for 10 Seconds
```Shell
./loadgen --tps=100 --depth=10 --nspans=10 --extra=8 --runtime=10s --ramptime=5s --sender=otel --insecure --host=localhost:4317
```

##### Data in ClickHouse
```json
"Timestamp": "2024-12-09 02:59:01.119417000",
"TraceId": "eaabcb311030f2f513e49ceed5fd11c5",
"SpanId": "146e7331c23d2e3d",
"ParentSpanId": "f31d8bb11a464d72",
"TraceState": "",
"SpanName": "wasabi",
"SpanKind": "SPAN_KIND_INTERNAL",
"ServiceName": "generatorTest",
"ResourceAttributes": {"service.version":"unknown","telemetry.sdk.language":"go","telemetry.sdk.name":"otelconfig","telemetry.sdk.version":"1.17.0","host.name":"BLRETV-V72JHX3G.local","service.name":"generatorTest"},
"ScopeName": "loadgen",
"ScopeVersion": "dev",
"SpanAttributes": {"small-ship":"true","early-horn":"32","hard-leg":"10187.073664152671","low-needle":"false","process_id":"47819","clear-horn":"0.9111312770436745","free-comb":"38","clear-ticket":"0.9597385593732547","whole-drawer":"8321d0c2bc55a888"},
"Duration": "511857375",
"StatusCode": "STATUS_CODE_UNSET",
"StatusMessage": "",
"Events.Timestamp": ["2024-12-09 02:59:01.119443000"],
"Events.Name": ["exception"],
"Events.Attributes": [{"exception.type":"error","exception.message":"error message","exception.stacktrace":"stacktrace","exception.escaped":"false"}],
"Links.TraceId": [],
"Links.SpanId": [],
"Links.TraceState": [],
"Links.Attributes": []
```

#### Script with Custom spanAttributes Added to it
```shell
./loadgen --tps=100 --depth=10 --nspans=10 --extra=8 --runtime=10s --ramptime=5s --sender=otel --insecure --host=localhost:4317 --dataset=generatorTest product=/sa discount=/b20 price=/fg100,50
```

##### Data in Clickhouse
```json
"Timestamp": "2024-12-09 03:40:32.810734000",
"TraceId": "fba470055ed0e606bccdea7112ce1e24",
"SpanId": "baba30b516af0c72",
"ParentSpanId": "89e05f9f751e9f1a",
"TraceState": "",
"SpanName": "coriander",
"SpanKind": "SPAN_KIND_INTERNAL",
"ServiceName": "generatorTest",
"ResourceAttributes": {"host.name":"BLRETV-V72JHX3G.local","service.name":"generatorTest","service.version":"unknown","telemetry.sdk.language":"go","telemetry.sdk.name":"otelconfig","telemetry.sdk.version":"1.17.0"},
"ScopeName": "loadgen",
"ScopeVersion": "dev",
"SpanAttributes": {"early-horn":"79","price":"0","clear-ticket":"0.9673112665661086","process_id":"59917","free-comb":"77","small-ship":"true","whole-drawer":"5344d1da62db0c38","hard-leg":"9532.70196019242","clear-horn":"0.11460945733595683","low-needle":"true","product":"xzynghiozkgggnjj","discount":"false"},
"Duration": "27313709",
"StatusCode": "STATUS_CODE_UNSET",
"StatusMessage": "",
"Events.Timestamp": [],
"Events.Name": [],
"Events.Attributes": [],
"Links.TraceId": [],
"Links.SpanId": [],
"Links.TraceState": [],
"Links.Attributes": []
```

### Generator Details

[GitHub - honeycombio/loadgen: A flexible command-line load generator to create traces in OTel or Honeycomb formats](https://github.com/honeycombio/loadgen?tab=readme-ov-file#generators)

| type  | description                                              | p1                          | p2                           |
| ----- | -------------------------------------------------------- | --------------------------- | ---------------------------- |
| i, ir | rectangularly distributed integers                       | min (0)                     | max (100)                    |
| ig    | gaussian integers                                        | mean (100)                  | stddev (10)                  |
| f, fr | rectangularly distributed floats                         | min (0)                     | max (100)                    |
| fg    | gaussian floats                                          | mean (100)                  | stddev (10)                  |
| b     | boolean                                                  | percentage true (50)        |                              |
| s, sa | alphabetic string                                        | length in chars (16)        |                              |
| sw    | pronounceable words, rectangular distribution            | cardinality (16)            |                              |
| sq    | pronounceable words, quadratic distribution              | cardinality (16)            |                              |
| sx    | hexadecimal string                                       | length in chars (16)        |                              |
| k     | key fields used for testing intermittent key cardinality | cardinality (50)            | period (60)                  |
| u     | url-like (2 parts)                                       | cardinality of 1st part (3) | cardinality of 2nd part (10) |
| uq    | url with random query                                    | cardinality of 1st part (3) | cardinality of 2nd part (10) |
| st    | status code                                              | percentage of 400s          | percentage of 500s           |


# Data shape

## ARB data

| Distributed Tracing                      |                                                                  |
| ---------------------------------------- | ---------------------------------------------------------------- |
| Ingestion throughput                     | 1-2 M spans/sec (per DC) BAU<br><br>2-4 M spans/sec (per DC) BBD |
| Number of spans in a Trace               | ~ 100-200                                                        |
| # of attributes in a span                | ~ 30-50                                                          |
| Max Depth of Trace                       | 10-20                                                            |
| Span Payload size                        | 5KB avg (Max 50KB)                                               |
| Retention                                | 1 month                                                          |
| Read throughput                          | 15 rps avg (Max 100 rps)                                         |
| Data Freshness                           | ~ 1-2 min (apart from tail)                                      |
| Query Latency<br><br>(Primary queries)   | 10s                                                              |
| Query Latency<br><br>(Secondary queries) | ~ 1-2 min                                                        |
| # of call Graphs/flows in FK             | 10 critical user path flows                                      |
| # of services in a trace                 | lower double digits                                              |
| Max Cardinality of a span                | lower double digits                                              |
| Cardinality of span attributes           |                                                                  |
## Span / Custom attributes

| Attribute Name   | Attribute Type |     | Cardinality |
| ---------------- | -------------- | --- | ----------- |
| fk.page.type     | String         |     |             |
| fk.url           | String         |     |             |
| fk.page.uri      | url            |     |             |
| http.method      | method         |     |             |
| http.status_code | int            |     |             |
| flow             | string         |     |             |
| originZone       | string         |     |             |
| status.code      | int            |     |             |
| net.peer.port    | ip             |     |             |
| net.peer.name    |                |     |             |
| apiMethod        |                |     |             |
|                  |                |     |             |

