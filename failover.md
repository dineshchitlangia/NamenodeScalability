# Failover / Startup

* **Optimal value for ZKFC timeout** - Defined in core-site, the default value of ha.health-monitor.rpc-timeout.ms=45000 i.e. 45 seconds.
You can bump it up to 120seconds [120000] for large clusters.
But remember, the more you increase it, you are basically delaying the failover. So in the event of an actual failure it will still wait for this time before triggering the transition.

* **Speed-up quota initialization** - When Namenode starts up, it loads the FSImage and Edits.
It needs to recalculate and verify the quotas for the entire namespace.
 As the size of namespace continues to increase, the quota initialization will take more time. You can assess this by checking Namenode Logs for statement like these:
```bash
(FSDirectory.java:updateCountForQuota(717)) - Quota initialization completed in 80844 milliseconds name space=76383028
```
Assuming you have more than 8 cores of cpu on Namenode hosts, in hdfs-site set 
```bash
dfs.namenode.quota.init-threads=8
```
You can further bump it up to 16 if you have more than 16 cores.


* **Throttle FSImage transfer bandwidth** - The default value is 0 which disables the throttling and it thus tries to use the max bandwidth available at that time. 
This can sometimes lead to the namenode not responding to ZKFC heartbeat for long enough for ZKFC to trigger a failover.

To restrict the transfer speed to 50 MB/s, in hdfs-site, set 
```bash
dfs.image.transfer.bandwidthPerSec=50000000
```


* **Bonus** - If you do not have use cases which need to find last access time for files in HDFS, turn off access time precision to avoid NN writing an edit log entry.
In hdfs-site, set:
```bash
dfs.namenode.accesstime.precision=0
```
