> Number of process checking
```
;The default value for this file, 32768, 
;results in the same range of PIDs as on earlier kernels (<=2.4). 
;The value in this file can be set to any value up to 2^22 (PID_MAX_LIMIT, approximately 4 million). 
; Value 4194303 is the maximum limit for x86_64 and 32767 for x86.

# sysctl kernel.pid_max 
or
# cat /proc/sys/kernel/pid_max

```
>  maximum number of threads that can be created by a process under Linux
```
max_threads = totalram_pages / (8 * 8192 / 4096);

# sysctl kernel.threads-max
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
