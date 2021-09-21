# Block Reports

For large clusters:

* Always split block report by volume
```dfs.blockreport.split.threshold = 0```

* Reduce full block report frequency
```dfs.blockreport.intervalMsec``` (default = 21600000 for every 6 hours. Consider making it 43200000 for 12 hours)

* Batch incremental block reports
```dfs.blockreport.incremental.intervalMsec``` (default = 0 disables IBR batching. Consider making it 100 to batch every 100ms)
