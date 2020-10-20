# fluentbit
## Remove Unstructured logs
```
[FILTER]
    Name    grep
    Match   *
    Exclude unstructured_logs .*
```
## Add Host Name
```
[Filter]
    Name    record_modifier
    Match   *
    Record Host_Name ${HOSTNAME}
```
## Filter Parser
```
[FILTER]
    Name parser
    Match *
    Key_Name restOfMsg
    Parser parser_name
    Reserve_Data On
    Preserve_Key On
```
