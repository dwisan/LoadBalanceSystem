>/etc/nginx.conf
```
stream {
        upstream mysql {
                hash $remote_addr consistent;
                server 172.18.111.102:3306;
                server 172.18.111.103:3306;
                server 172.18.111.104:3306;
        }
        server {
                listen 172.18.111.107:3306;
                proxy_pass mysql;
                proxy_connect_timeout 1s;
        }
}
```
