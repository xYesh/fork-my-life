---
{"dg-publish":true,"permalink":"/para/projects/event-store/notes/jaegar-as-a-frontend-for-clickhouse/"}
---


# References 
[[_PARA/Resources/Jaegar clickhouse community edition\|Jaegar clickhouse community edition]]


<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">




==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data
## Text Elements
Get list of services irrespective of time range
 
Key value pairs of span attributes 
Time range
 
Min and max trace Duration
 
These are only UI operations, 
Don't impact the query
 
Graph is generated based on the 
trace data of the "20" traces.
20 is the limit given 
No. Of traces to be returned
 
List of operations irrespective of time range
 
User input: 
If you know the traceID 
Entire trace data is already fetched 
When you click on find traces 
## Embedded Files
57f4a73f8ce08072f9929a79e84c0e65e8f23294: [[Screenshot 2024-12-12 at 6.11.45 PM.png]]



</div></div>



# Workflow

### On page load

- The list of services are populated on page load.
    - The operation parameter is also loaded for the service that is selected by default. 
    - "operation" here just refers to the **SpanName**. 

### On clicking "Find Traces"

- Based on the various parameters given above, a single query is sent to the backend which returns the number of **traces equal to "limit results", sorted by their timestamp.**
- A single json is returned from the backend containing the entire trace with all the details for the "x" number of traces. 

### Sort dropdown

- All the various sorting options shown here are only UI operations. 
- No request is sent to the backend. 

### Compare page

- If you know two trace IDs, It generates a comparison page.

  

---

# Queries that are run in the backed to generate this UI

## Otel schema

### Get the list of services

|   |
|---|
|`SELECT DISTINCT ServiceName FROM otel_traces`|

  

### List of operations

|   |
|---|
|`SELECT DISTINCT SpanName FROM otel_traces where ServiceName = {ServiceName}`|

  

### Find traces based on criteria

|   |
|---|
|`#Get the list of spans.`<br><br>`SELECT * FROM otel_traces`<br><br>`WHERE ServiceName = {ServiceName}`<br><br>`AND SpanName = {SpanName}`<br><br>`AND Duration > {min_duration} AND Duration < {max_duration}`<br><br>`AND SpanAttributes[{SpanAttribute_key}] = {SpanAttribute_value}`<br><br>`AND Timestamp > {lookBackTime_timestamp} AND Timestamp < {current_timestamp}`<br><br>`#Unqiue Trace IDs are aquired from the above query and gotten using the below query.`|

References - [https://github.com/jaegertracing/jaeger-clickhouse/blob/main/storage/clickhousespanstore/reader.go](https://github.com/jaegertracing/jaeger-clickhouse/blob/main/storage/clickhousespanstore/reader.go)

### Get trace details based on TraceID

|   |
|---|
|`SELECT * FROM otel_traces`<br><br>`WHERE TraceId = {TraceID}`<br><br>`AND Timestamp >= (SELECT Start FROM otel_traces_trace_id_ts WHERE TraceId = {TraceID})`<br><br>`AND Timestamp <= (SELECT End FROM otel_traces_trace_id_ts WHERE TraceId = {TraceID})`|

  

### Trace comparison page

|                                  |
| -------------------------------- |
| `#Uses the same query as above.` |