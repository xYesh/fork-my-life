---
{"dg-publish":true,"permalink":"/para/resources/processed/how-tos/alertmanager-ap-is/","tags":["O11y/Alertmanager"]}
---



# Creating an alert on the Alertmanager

```Plain
current_time=$(date -u +"%Y-%m-%dT%H:%M:%SZ")
end_time=$(date -u -v +10M +"%Y-%m-%dT%H:%M:%SZ")

# Send the alert to Alertmanager
curl -X POST 'http://10.24.34.250/api/v2/alerts' \
     -H 'Content-Type: application/json' \
     -d '[
           {
             "labels": {
               "alertname": "AA_ExampleAlert",
               "severity": "critical"
             },
             "annotations": {
               "summary": "Example alert",
               "description": "This is a test alert"
             },
             "startsAt": "'"$current_time"'",
             "endsAt": "'"$end_time"'",
             "generatorURL": "http://localhost:9090"
           }
         ]'
```

# Silencing an alert on the alertmanager

```Plain
curl -X POST 'http://alertmanager-instance/api/v2/silences' \
-H 'Content-Type: application/json' \
-d '{
  "matchers": [
    {
      "name": "alertz_zone",
      "value": "in-chennai-2",
      "isRegex": false
    }
  ],
  "startsAt": "'"$(date -u +"%Y-%m-%dT%H:%M:%SZ")"'",
  "endsAt": "'"$(date -u -d '1 day' +"%Y-%m-%dT%H:%M:%SZ")"'",
  "createdBy": "HealthCheckScript",
  "comment": "AlertZV3 Degrade due to cosmos lag"
}'
```