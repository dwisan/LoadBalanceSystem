> /etc/security/limit.conf

```
[x] Number of file (file descriptor)
www-data soft nofile 1000000
www-data hard nofile 1200000

[x] Number of Processes (max user processes)
www-data soft nproc 1000000
www-data hard nproc 1200000

```
