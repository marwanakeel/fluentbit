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
Long lines
```
    Buffer_Chunk_Size 1M
    Buffer_Max_Size 1M
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
## Add Host Openshift
```
[Filter]
    Name    record_modifier
    Match   *
    Record MY_NODE_NAME ${MY_NODE_NAME}
    Record MY_POD_NAME ${MY_POD_NAME}
    Record MY_POD_NAMESPACE ${MY_POD_NAMESPACE}
    Record MY_POD_IP ${MY_POD_IP}
    Record MY_HOST_IP ${MY_HOST_IP}
    Record MY_POD_SERVICE_ACCOUNT ${MY_POD_SERVICE_ACCOUNT}
```
then add this in the DC
```
env:
    - name: MY_NODE_NAME
      valueFrom:
        fieldRef:
          apiVersion: v1
          fieldPath: spec.nodeName
    - name: MY_POD_NAME
      valueFrom:
        fieldRef:
          apiVersion: v1
          fieldPath: metadata.name
    - name: MY_POD_NAMESPACE
      valueFrom:
        fieldRef:
          apiVersion: v1
          fieldPath: metadata.namespace
    - name: MY_POD_IP
      valueFrom:
        fieldRef:
          apiVersion: v1
          fieldPath: status.podIP
    - name: MY_HOST_IP
      valueFrom:
        fieldRef:
          apiVersion: v1
          fieldPath: status.hostIP
    - name: MY_POD_SERVICE_ACCOUNT
      valueFrom:
        fieldRef:
          apiVersion: v1
          fieldPath: spec.serviceAccountName

```
