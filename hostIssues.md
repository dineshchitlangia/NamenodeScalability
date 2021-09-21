### Dealing with host level issues

* Dedicated disks for NN / JN
* Don’t co-locate heavy services like HiveServer2, HBase Master/Region server, Yarn RM etc on the same host as NN
* Verify Kernel Configs after every OS patching

<details><summary>CPU Scaling should prefer Performance over Power Saving</summary>
<p>
  
Please set scaling governors to performance, which means running the CPU at maximum frequency. 
To do so run `cpufreq-set -r -g performance` OR 
edit /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor and set the content to 'performance'

</p>
</details>

<details><summary>Disable Transparent Huge Pages</summary>
<p>

Disable THP to reduce the amount of system CPU utilization on your worker nodes. 
THP needs to be disabled by ensuring that the following proc entries are set to [never] instead of [always]: 
/sys/kernel/mm/redhat_transparent_hugepage/enabled

</p>
</details>
