# Group Lookup Performance

The NameNode frequently performs group lookups for users submitting requests. 
This is part of the normal access control mechanism. Group lookups may involve contacting an LDAP server. If the LDAP server is slow to process requests, it can hold up NameNode RPC handler threads and affect cluster performance. When the NameNode suffers from this problem it logs a warning that looks like this:

Potential performance problem: getGroups(user=john) took 23861 milliseconds.
The default warning threshold is 5 seconds. If you see this log message you should investigate the performance of your LDAP server right away. There are a few workarounds if it is not possible to fix the LDAP server performance immediately.

* **Increase the group cache timeout** - 
  The NameNode caches the results of group lookups to avoid making frequent lookups. 
  The cache duration is set via hadoop.security.groups.cache.secs in your core-site.xml file. 
  The default value of 300 (5 minutes) is too low. We have seen this value set as high as 7200 (2 hours) in busy clusters to avoid frequent group lookups.
* **Increase the negative cache timeout** - 
  For users with no group mapping, the NameNode maintains a short negative cache. 
  The default negative cache duration is 30 seconds and may be too low. 
  You can increase this with the hadoop.security.groups.negative-cache.secs setting in core-site.xml. 
  This setting is useful if you see frequent messages like No groups available for user alice in the NameNode logs.
* **Add a static mapping override for specific users** - 
  A static mapping is an in-memory mapping added via configuration that eliminates the need to lookup groups via any other mechanism. 
  It can be a useful last resort to work around LDAP server performance issues that cannot be easily addressed by the cluster administrator. 
  An example static mapping for two users jane and john looks like this:
  ```bash
  <property>
    <name>hadoop.user.group.static.mapping.overrides</name>
    <value>jane=staff,hadoop;john=students,hadoop;</value>
  </property>
  ```
