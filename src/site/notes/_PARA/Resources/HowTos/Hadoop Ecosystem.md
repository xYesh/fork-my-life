---
{"dg-publish":true,"permalink":"/para/resources/how-tos/hadoop-ecosystem/"}
---

### enable erasure coding

```Plain
./hadoop-3.2.1/bin/hdfs ec -setPolicy -path /zmail/LibTest -policy XOR-2-1-1024k
```

### give all permissions

```Plain
hadoop fs -chmod +wx /zmail
```

### count the number of file

```Plain
sudo -u hdfs hdfs dfs -count -h /user/
```

### size of all the folders

```Plain
sudo -u hdfs hdfs dfs -du -s /v_log/5/* | sort -h -k1
```

# Yarn

### Get logs from container

```Plain
sudo -u hdfs yarn logs --applicationId application_1704692964984_97803
```