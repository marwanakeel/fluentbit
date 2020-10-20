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
## Time offset Parser
```
[PARSER]
    Name    parser_name
    Format  regex
    Regex   (?<firstline>(?<timestamp>\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}.\d{3}).*reply\sis\s(?<status>.*?)\sfor\s{(?<type>.*?)}.*cs_refid='(?<csrefid>.*?)'\sfor\sintx=(?<intx>.*)\.\s*$)
    Time_Format %Y-%m-%dT%H:%M:%S.%L
    Time_key timestamp
    Time_Offset +0200
```
