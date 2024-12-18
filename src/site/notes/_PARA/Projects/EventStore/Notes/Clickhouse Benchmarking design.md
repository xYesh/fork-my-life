---
{"dg-publish":true,"permalink":"/para/projects/event-store/notes/clickhouse-benchmarking-design/","tags":["Database/Clickhouse","Benchmark"]}
---


# Function testing
## <font color="#f79646">Flipkart specific testing</font>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/para/projects/event-store/notes/attachments/diagram-functional-testing/" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">





==⚠ Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'

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



