### Sandbox
https://dataexplorer.azure.com/clusters/help/databases/Samples

### Result to json 
```kql
StormEvents
| take 10
| extend PackedRecord = pack_all()
| summarize Result = make_list(PackedRecord);
```

https://dataexplorer.azure.com/clusters/help/databases/SampleLogs
### Parsing JSON
Dictionary -> toString -> parse_json() -> bag_unpack() -> 
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
