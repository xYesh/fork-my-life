---
{"dg-publish":true,"permalink":"/para/projects/event-store/notes/clickhouse-benchmarking-design/","tags":["Database/Clickhouse","Benchmark"]}
---


# Function testing
## <font color="#f79646">Flipkart specific testing</font>


<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">




==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data
## Text Elements
Prod UAR APP
 
OTEL collector
 
Pulsar pipeline 
Individual span data
 
Perf app (amplifying traces) 
pulsar pipeline 
Otel agent
Directly or via 
a feeder app 
Clickhouse
Cluster 
E
L
B 
K6 cluster 
Read queries
     via
HTTP API 
Load 
balancer 
TCP
Clickhouse binary format 
Traces generation app pipeline 


</div></div>

### Goals of functional testing
- Ensuring Clickhouse can ingest Flipkart data in same format.
- Reading data from Clickhouse using the **default Clickhouse datasource in Grafana.**

# Benchmarking

## Single server clickhouse

### Architecture

<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">




==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data
## Text Elements
loadgen cluster
 
OTEL collector
w/
Clickhouse exporter 
Clickhouse-Benchmark
 
TCP
port:9000 
TCP
 
gRCP 
Clickhouse
Standalone
 


</div></div>



