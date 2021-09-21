# Scaling the Namenode
General guidelines for scaling the Namenode in HDFS, lessons learnt from the field.

<details><summary>Commonly reported problems</summary>
<p>

* Performance
 * RPC Processing Time
 * GC pauses
 * Read/Write performance
 * Too long to start NN
* Stability
 * Frequent Failover
 * Frequent Crash
  
</p>
</details>

<details><summary>Various causes</summary>
<p>

* Small files
* Sub optimal heap settings
* Missing RPC improvements
* Bad Applications / Mistuned Components
* Degraded AD
* Too frequent/delayed checkpointing
* Heavy Services co-located / Disk throughput
* Too much logging
* Degraded JN / communication between NN/JN/ZK

</p>
</details>


## Tips for Scaling

- [Optimize Logging](logging.md)
