---
{"dg-publish":true,"permalink":"/para/projects/event-store/notes/clickhouse-benchmarking-design/","tags":["Database/Clickhouse","Benchmark"]}
---


# Function testing
## <font color="#f79646">Flipkart specific testing</font>

![Pasted image 20241218222401.png](/img/user/_PARA/Projects/EventStore/Notes/attachments/Pasted%20image%2020241218222401.png)
### Goals of functional testing
- Ensuring Clickhouse can ingest Flipkart data in same format.
- Reading data from Clickhouse using the **default Clickhouse datasource in Grafana.**

# Benchmarking

## Single server clickhouse

### Architecture
![Pasted image 20241218222412.png](/img/user/_PARA/Projects/EventStore/Notes/attachments/Pasted%20image%2020241218222412.png)


