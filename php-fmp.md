> PHP-FPM 7.0 on Ubuntu 16.04.4 
```
****** case : pm = dynamic *******

# nano /etc/php/7.0/fpm/pool.d/www.conf

;listen = /run/php/php7.0-fpm.sock
listen = 127.0.0.1:2999

listen.allowed_clients = 127.0.0.1

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
```
