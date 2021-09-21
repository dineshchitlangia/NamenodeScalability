# Optimizing RPC

<details><summary>Service RPC & Handler count</summary>
<p>

### Enable Service RPC
The service RPC port gives the DataNodes a dedicated port to report their status via block reports and heartbeats. The port is also used by Zookeeper Failover Controllers for periodic health checks by the automatic failover logic. The port is never used by client applications hence it reduces RPC queue contention between client requests and DataNode messages.

For an HA cluster, the service RPC port can be enabled with settings like the following, replacing mycluster, nn1 and nn2 appropriately.
```bash
 <property>
    <name>dfs.namenode.servicerpc-address.mycluster.nn1</name>
    <value>mynamenode1.example.com:8040</value>
  </property>

  <property>
    <name>dfs.namenode.servicerpc-address.mycluster.nn2</name>
    <value>mynamenode2.example.com:8040</value>
  </property>
```
If you have enabled Automatic Failover using ZooKeeper Failover Controllers, an additional step is required to reset the ZKFC state in ZooKeeper. 
 
Stop both the ZKFC processes and run the following command as the hdfs super user.
```bash
  hdfs zkfc –formatZK
```

Restart HDFS Service for changes to take effect. 

To minimize downtime, you may perform rolling restarts of Namenodes, Datanodes, Journal Nodes and ZKFC.

### RPC Handler Count
The Hadoop RPC server consists of a single RPC queue per port and multiple handler (worker) threads that dequeue and process requests. 
If the number of handlers is insufficient, then the RPC queue starts building up and eventually overflows. 
You may start seeing task failures and eventually job failures and unhappy users.
It is recommended that the RPC handler count be set to 20 * log2(Cluster Size) with an upper limit of 200.

The RPC handler count can be configured with the following setting in hdfs-site.xml, eg. for 50 node cluster:
```bash
  <property>
    <name>dfs.namenode.handler.count</name>
    <value>120</value>
  </property>
```

### Service RPC Handler Count
There is no precise calculation for the Service RPC handler count however the default value of 10 is too low for most production clusters. 
We have often seen this initialized to 50% of the dfs.namenode.handler.count in busy clusters and this value works well in practice.
 
e.g. for the same 50 node cluster you would initialize the service RPC handler count with the following setting in hdfs-site.xml.
```bash
  <property>
    <name>dfs.namenode.service.handler.count</name>
    <value>60</value>
  </property>
```

</p>
</details>

<details><summary>RPC Congestion Control</summary>
<p>
  
RPC Congestion Control feature was designed to help Hadoop Services respond more predictably under high load. 
RPC queue overflow can cause request timeouts, and eventually job failures.
This problem can be mitigated if the NameNode sends an explicit signal back to the client when its RPC queue is full. 
Instead of waiting for a request that may never complete, the client throttles itself by re-submitting the request with an exponentially increasing delay. 
If you are familiar with the Transmission Control Protocol, this is similar to how senders react when they detect network congestion.
 
The feature can be enabled with the following setting in your core-site.xml file. 
Replace 8020 with your NameNode RPC port number if it is different. 
Do not enable this setting for the Service RPC port or the DataNode lifeline port.
```bash
<property>
    <name>ipc.8020.backoff.enable</name>
    <value>true</value>  
</property>
```

</p>
</details>

<details><summary>RPC Fair Call Queue</summary>
<p>
  
The RPC FairCallQueue replaces the single RPC queue with multiple prioritized queues(not to be confused with Yarn Queues). 
The RPC server maintains a history of recent requests grouped by user. 
It places incoming requests into an appropriate queue based on the user's history. 
RPC handler threads will dequeue requests from higher priority queues with a higher probability.
 
FairCallQueue complements RPC congestion control very well and works best when you enable both features together. 
FairCallQueue can be enabled with the following setting in your core-site.xml. 
Replace 8020 with your NameNode RPC port if it is different. 
Do not enable this setting for the Service RPC port or the DataNode lifeline port.

```bash
  <property>
    <name>ipc.8020.callqueue.impl</name>
    <value>org.apache.hadoop.ipc.FairCallQueue</value>
  </property>
```

</p>
</details>

<details><summary>Datanode Lifeline Protocol</summary>
<p>
  
Changing the lifeline protocol settings requires a restart of the NameNodes, DataNodes and ZooKeeper Failover Controllers to take full effect. 
If you have a NameNode HA setup, you can restart the NameNodes one at a time followed by a rolling restart of the remaining components to avoid cluster downtime.
 
Datanode Lifeline Protocol introduces a new lightweight RPC message that is used by the DataNodes to report their health to the NameNode. 
It was developed in response to problems seen in some overloaded clusters where the NameNode was too busy to process heartbeats and spuriously marked DataNodes as dead.
For an HA cluster, the lifeline RPC port can be enabled with settings like the following, replacing mycluster, nn1 and nn2 appropriately.
  
```bash
  <property>
    <name>dfs.namenode.lifeline.rpc-address.mycluster.nn1</name>
    <value>mynamenode1.example.com:8050</value>
  </property>
 
  <property>
    <name>dfs.namenode.lifeline.rpc-address.mycluster.nn2</name>
    <value>mynamenode2.example.com:8050</value>
  </property>
```

</p>
</details>
