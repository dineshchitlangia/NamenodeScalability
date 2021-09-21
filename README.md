# Scaling the Namenode
General guidelines for scaling the Namenode in HDFS, lessons learnt from the field.

This repo is a collateral for my presentation at ApacheCon 2021 titled "Scaling the Namenode - Lessons Learnt"
<p></p>

[The slide deck from the talk is available here.](Slides_ApacheCon2021.pdf)

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
- [Optimize RPC](rpc.md)
- [Eliminating Host Level issues](hostIssues.md)
- [Group Lookup Performance](groupLookup.md)
- [Tuning Failover / Startup](failover.md)
- [Heap Memory](heap.md)
- [Block Reports](blockReport.md)
- [Managing External Factors](externalFactors.md)
