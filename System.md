> Number of process checking
```
;The default value for this file, 32768, 
;results in the same range of PIDs as on earlier kernels (<=2.4). 
;The value in this file can be set to any value up to 2^22 (PID_MAX_LIMIT, approximately 4 million). 

# sysctl kernel.pid_max 
or
# cat /proc/sys/kernel/pid_max

# cat /proc/sys/kernel/threads-max
```
> /etc/security/limit.conf

```
[x] Number of file (file descriptor)
www-data soft nofile 1000000
www-data hard nofile 1200000

[x] Number of Processes (max user processes)
www-data soft nproc 1000000
www-data hard nproc 1200000

```
