> PHP-FPM 7.0 on Ubuntu 16.04.4
- [x] edit pool named 'www'
```
# nano /etc/php/7.0/fpm/pool.d/www.conf

;listen = /run/php/php7.0-fpm.sock
listen = 127.0.0.1:2999

listen.allowed_clients = 127.0.0.1

****** case : pm = dynamic *******
pm = dynamic

; pm.max_children = all_memory - 1024/mem_per_process using htop monitor
pm.max_children = 135

; Default Value: min_spare_servers + (max_spare_servers - min_spare_servers) / 2
pm.start_servers = 30
pm.min_spare_servers = 20
pm.max_spare_servers = 40
pm.max_requests = 600

****** case : pm = ondemand *******
pm = ondemand
pm.max_children = 135
pm.process_idle_timeout = 10s
pm.max_requests = 600
```
- [x] Set open file descriptor rlimit (Default Value: system defined value)
```
 [x] System Defined value checking
 
 # cat /proc/{php-fpm}/limits
 exam.
 # cat /proc/1133/limits
 Limit            Soft Limit    Hard Limit     Units 
 ....
 Max open files     1024           4096        files
 ....
 
 [x] Set more than 4096
 
 # mkdir /etc/systemd/system/php7.0-fpm.service.d
 # nano /etc/systemd/system/php7.0-fpm.service.d/local.conf
 
 [Service]
 LimitNOFILE=100000
 
 # systemctl daemon-reload
 # /etc/init.d/php7.0-fpm restart
 
 [x] System Defined value checking after edit
 
 # cat /proc/1133/limits
 Limit            Soft Limit    Hard Limit     Units 
 ....
 Max open files     100000           100000        files
 ....
```
