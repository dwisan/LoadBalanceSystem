>/etc/nginx/nginx.conf
```
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
        worker_connections 30000;
        multi_accept on;
}

worker_rlimit_nofile 100000;

stream {
        upstream mysql {
                #hash $remote_addr consistent;
                least_conn;
                server 172.18.111.36:3306 max_fails=2 fail_timeout=10s;
                server 172.18.111.37:3306 max_fails=2 fail_timeout=10s;
                server 172.18.111.38:3306 max_fails=2 fail_timeout=10s;
                server 172.18.111.39:3306 max_fails=2 fail_timeout=10s;
        }
        server {
                listen 172.18.111.19:3306;
                proxy_pass mysql;
                proxy_connect_timeout 30s;
                proxy_buffer_size 16k;
        }
}
```
