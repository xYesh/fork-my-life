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

#### Script with print the traces to local
```shell
./loadgen --tracecount=3 --depth=10 --nspans=10 --extra=8 --sender=print --dataset=generatorTest product=/sa discount=/b20 price=/fg100,50


./loadgen --tracecount=1 --depth=10 --nspans=20 --sender=print --dataset=prodRepl net.peer.name=0.0.0.0 http.status_code=/st10,0.1 status.code=/i-1,1 net.peer.port=-1 fk.page.uri=/uq10,100000 fk.url=/u10,1000 flow=/u10,1000 originZone=/sw8 fk.page.type=/sq1000 apiMethod=/sw5000  http.method=/sq10 EngagementTime=/i0,1000 Uptime=/ir0,1000 FULLY_PAINTED_TIME=/ig0,1000 appName=/sw20 SessionInfo=/sa16


./loadgen --tracecount=100 --depth=10 --nspans=20 --sender=print --dataset=prodRepl fk.page.type=/sq100000
```
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
#### References - 
- Asterix - [Jiro Bundles](http://jiro.fkcloud.in/admin/config/asterix/zone/in-hyderabad-1/bundles/20)
- RUM - [New Relic](https://one.newrelic.com/data-exploration/data-explorer/explorer?account=3700279&duration=1800000&state=06b1df31-cc06-595e-7ca2-dacb64cfb07d)
- DT - [UAR Traces](https://confluence.fkinternal.com/display/MTL/Attribute+Details+of+UAR+Traces)

| Source  | Attribute Name     | Attribute Type | Example                                                                                                     | Cardinality | Loadgen parameter |
| ------- | ------------------ | -------------- | ----------------------------------------------------------------------------------------------------------- | ----------- | ----------------- |
| UAR/DT  | net.peer.name      | ip             | 10.24.15.135                                                                                                |             |                   |
| UAR/DT  | http.status_code   | int            | 200                                                                                                         |             |                   |
| UAR/DT  | status.code        | int            | 0                                                                                                           |             |                   |
| UAR/DT  | net.peer.port      | int            | -1                                                                                                          |             |                   |
| UAR/DT  | fk.page.uri        | url            | "/%20/p/%20?pid=TSHGFF84YKTPQN5E"                                                                           | High        |                   |
| UAR/DT  | fk.url             | url            | "/4/page/fetch"                                                                                             | Low         |                   |
| UAR/DT  | flow               | url            | "/4/page/fetch/productPage"                                                                                 | Low         |                   |
| UAR/DT  | originZone         | String         | "in-hyderabad-1"                                                                                            | Low         |                   |
| UAR/DT  | fk.page.type       | String         | productPage                                                                                                 | Low         |                   |
| UAR/DT  | apiMethod          | String         | "com.flipkart.astra.server.resource.CustomAstraResource.fetchWidget"                                        | Low         |                   |
| UAR/DT  | http.method        | String         | "POST"                                                                                                      | Low         |                   |
| RUM     | EngagementTime     | int            | 2                                                                                                           |             |                   |
| RUM     | Uptime             | int            | 300                                                                                                         |             |                   |
| RUM     | FULLY_PAINTED_TIME | int            | 1234                                                                                                        |             |                   |
| RUM     | appName            | String         | fkAndroid                                                                                                   |             |                   |
| RUM     | SessionInfo        | String         | UUID + Some string                                                                                          |             |                   |
| RUM     | sessionCrashed     | Boolean        | true                                                                                                        |             |                   |
| RUM     | deviceUUID         | UUID           |                                                                                                             | High        |                   |
| RUM     | SessionID          | UUID           |                                                                                                             | High        |                   |
| RUM     | ABInfo             | UUID           |                                                                                                             | High        |                   |
| RUM     | AppVersion         | String         | 8.13                                                                                                        | Low         |                   |
| RUM     | requestDomain      | url            | www.Flipkart.com                                                                                            | Low         |                   |
| RUM     | JS_Crash_Message   | String         | WIDGET_RENDERING_ERROR_ATLAS_WIDGET_default_fk_hp_tab_marketplace_wo-grocery_1: undefined is not a function | Low         |                   |
| RUM     | MobileOSVersion    | String         | 8.0.1                                                                                                       | Low         |                   |
| RUM     | deviceManufacturer | String         |                                                                                                             | Low         |                   |
| RUM     | deviceModel        | String         |                                                                                                             | Low         |                   |
| RUM     | deviceType         | String         | Mobile / Tablet / random string                                                                             | Low         |                   |
| Asterix | fk-client-ip       | ip             |                                                                                                             |             |                   |
| Asterix | CheckoutId         | UUID           |                                                                                                             |             |                   |
| Asterix | OrderId            | UUID           |                                                                                                             |             |                   |
| Asterix | EventId            | UUID           |                                                                                                             |             |                   |
| Asterix | CustomerId         | UUID           |                                                                                                             | High        |                   |
| Asterix | offerStatus        | String         |                                                                                                             | Low         |                   |
| Asterix | x-user-agent       | String         |                                                                                                             | Low         |                   |
