---
{"dg-publish":true,"permalink":"/para/projects/event-store/notes/write-path-generator-evaluation/","tags":["Database/Clickhouse","Benchmark"]}
---



# Honeycomb traces generator

- [[_PARA/Projects/EventStore/Notes/Write path generator evaluation#Commands\|Commands]]
	- [[_PARA/Projects/EventStore/Notes/Write path generator evaluation#Commands\|basic script that generates 100 traces per second for 10 seconds]]
		- [[_PARA/Projects/EventStore/Notes/Write path generator evaluation#basic script that generates 100 traces per second for 10 seconds\|Data in ClickHouse]]
	- [[_PARA/Projects/EventStore/Notes/Write path generator evaluation#Commands\|Script with custom spanAttributes added to it]]
		- [[_PARA/Projects/EventStore/Notes/Write path generator evaluation#Script with custom spanAttributes added to it\|Data in clickhouse]]
- [[_PARA/Projects/EventStore/Notes/Write path generator evaluation#Generator details\|Generator details]]



Repo - [GitHub - honeycombio/loadgen: A flexible command-line load generator to create traces in OTel or Honeycomb formats](https://github.com/honeycombio/loadgen)

[[_PARA/Projects/EventStore/Notes/attachments/Diagram - loadgen-setup\|Diagram - loadgen-setup]]
### Commands 
#### basic script that generates 100 traces per second for 10 seconds
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

#### Script with custom spanAttributes added to it
```shell
./loadgen --tps=100 --depth=10 --nspans=10 --extra=8 --runtime=10s --ramptime=5s --sender=otel --insecure --host=localhost:4317 --dataset=generatorTest product=/sa discount=/b20 price=/fg100,50
```

##### Data in clickhouse
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

### Generator details
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



