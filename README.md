# fluentbit
## Input File
```
[INPUT]
    Name        tail
    Path        /var/log/*
    Exclude     *.gz
    Path_Key    filepath
    Key unstructured_logs
    Tag tagMatch
    DB /etc/td-agent-bit/tail.db
    Parser       generic
```
## Output Eleastic
```
[OUTPUT]
    Name    es
    Host    10.230.84.20
    Port    9201
    HTTP_User   elastic
    HTTP_Passwd elastic123
    Logstash_Format True
    Logstash_Prefix indexName
    Match *
    Type _doc
```
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
