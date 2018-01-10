>ha.cf
```
debugfile     /var/log/ha-debug
logfile         /var/log/ha-log
logfacility     local0
keepalive 1
deadtime 6
initdead 60
udpport 695
ucast enp3s0 172.18.111.151
ucast enp3s0 172.18.111.152
node ubuntu01 ubuntu02
auto_failback on
```
>haresources
```
ubuntu01 IPaddr2::172.18.111.149/24/enp3s0
```
>authkeys
```
auth 1
1 md5 fcaa1a5a236da68948f3e16fc8469e71
```
