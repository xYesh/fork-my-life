---
{"dg-publish":true,"permalink":"/para/projects/event-store/notes/jaegar-as-a-frontend-for-clickhouse/"}
---


# References

[[_PARA/Resources/Jaegar clickhouse community edition\|Jaegar clickhouse community edition]]

![Pasted image 20241218222454.png](/img/user/_PARA/Projects/EventStore/Notes/attachments/Pasted%20image%2020241218222454.png)

# Workflow

### On Page Load

- The list of services are populated on page load.
    - The operation parameter is also loaded for the service that is selected by default. 
    - "operation" here just refers to the **SpanName**. 

### On Clicking "Find Traces"

- Based on the various parameters given above, a single query is sent to the backend which returns the number of **traces equal to "limit results", sorted by their timestamp.**
- A single json is returned from the backend containing the entire trace with all the details for the "x" number of traces. 

### Sort Dropdown

- All the various sorting options shown here are only UI operations. 
- No request is sent to the backend. 

### Compare Page

- If you know two trace IDs, It generates a comparison page.

  

---

# Queries That Are Run in the Backed to Generate This UI

## Otel Schema

### Get the List of Services

|   |
|---|
|`SELECT DISTINCT ServiceName FROM otel_traces`|

  

### List of Operations

|   |
|---|
|`SELECT DISTINCT SpanName FROM otel_traces where ServiceName = {ServiceName}`|

  

### Find Traces Based on Criteria

|   |
|---|
|`#Get the list of spans.`<br><br>`SELECT * FROM otel_traces`<br><br>`WHERE ServiceName = {ServiceName}`<br><br>`AND SpanName = {SpanName}`<br><br>`AND Duration > {min_duration} AND Duration < {max_duration}`<br><br>`AND SpanAttributes[{SpanAttribute_key}] = {SpanAttribute_value}`<br><br>`AND Timestamp > {lookBackTime_timestamp} AND Timestamp < {current_timestamp}`<br><br>`#Unqiue Trace IDs are aquired from the above query and gotten using the below query.`|

References - [https://github.com/jaegertracing/jaeger-clickhouse/blob/main/storage/clickhousespanstore/reader.go](https://github.com/jaegertracing/jaeger-clickhouse/blob/main/storage/clickhousespanstore/reader.go)

### Get Trace Details Based on TraceID

|   |
|---|
|`SELECT * FROM otel_traces`<br><br>`WHERE TraceId = {TraceID}`<br><br>`AND Timestamp >= (SELECT Start FROM otel_traces_trace_id_ts WHERE TraceId = {TraceID})`<br><br>`AND Timestamp <= (SELECT End FROM otel_traces_trace_id_ts WHERE TraceId = {TraceID})`|

  

### Trace Comparison Page

|                                  |
| -------------------------------- |
| `#Uses the same query as above.` |
