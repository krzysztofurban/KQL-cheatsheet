### Sandbox
https://dataexplorer.azure.com/clusters/help/databases/

### Result to json 
Database: Samples
```kql
StormEvents
| take 10
| extend PackedRecord = pack_all()
| summarize Result = make_list(PackedRecord);
```


### Parsing JSON
Database: SampleLogs
Dictionary -> tostring() -> parse_json() -> bag_unpack() -> project-away
```kql
TraceLogs
| take 10
| extend PropertiesToString = tostring(Properties)
| extend ChceckType = gettype(PropertiesToString)
| extend StringToJson = parse_json(PropertiesToString)
| project StringToJson
| evaluate bag_unpack(StringToJson)
| project-away compressedSize
```

###Joining two tables
Database: SampleLogs
Create table in scope of the script -> bag_unpack() -> parse SomeMessage -> join
```
let MyTable = datatable (procid:string, Name:string)
[
    "2202717", "Name1",
    "2202716", "Name2"
];
RawSysLogs
| take 10
| where fields has_cs "pam_unix"
| project fields
| evaluate bag_unpack(fields)
| extend TimeStamp = totimespan(timestamp)
| parse message with * "pam_unix(" SomeMessage ")" *
| extend TypeOf = gettype(procid)
| extend SomeMessage
| join kind=leftouter (MyTable) on procid
```
