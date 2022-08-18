### Sandbox
https://dataexplorer.azure.com/clusters/help/databases/Samples

### Result to json 
```kql
StormEvents
| take 10
| extend PackedRecord = pack_all()
| summarize Result = make_list(PackedRecord);
```
