### Optimizing Logging

<details><summary>Disable getfileinfo audit logging</summary>
<p>

```bash
dfs.namenode.audit.log.debug.cmdlist = getfileinfo
```
Default is empty

</p>
</details>

<details><summary>Async Logging</summary>
<p>
In hdfs-site.xml set,
```bash
dfs.namenode.edits.asynclogging = true
dfs.namenode.audit.log.async = true
```

</p>
</details>

<details><summary>Minimize State Change Logging</summary>
<p>

```bash
log4j.logger.BlockStateChange=WARN
log4j.logger.org.apache.hadoop.hdfs.StateChange=WARN
```
If you want to be more aggressive, you can set them to ERROR

</p>
</details>
