# fluentbit
## Remove Unstructured logs
```
[FILTER]
    Name    grep
    Match   *
    Exclude unstructured_logs .*
```

```
[Filter]
    Name    record_modifier
    Match   *
    Record Host_Name ${HOSTNAME}
```
