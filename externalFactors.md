# Managing External Factors

* Control small files
* Avoid **du**, use **ls** instead
* Reduce Max Replication ```dfs.replication.max``` from default 512 to a ranger of 10-20.
* Hive/Tez - Merge map/reduce/intermediate output, ACID Compactions
* Spark - Enable History Cleaner
* YARN - Log aggregation
* HBase - Carefully choose the RegionSplitPolicy, merge size 0 regions
* Data retention policy
