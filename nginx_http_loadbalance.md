>installation packages
```
apt-get update
apt-get upgrade -y
apt-get install nginx php php-fpm
```
>nginx Configuration for frontend
```
nano /etc/nginx/sites-enabled/default

upstream mywebapp1 {
        
        # ip_hash available to 3 octets of ip no same
        # not work on same subnet , work load to asign one backend server
        ip_hash;

        server 172.18.111.151 max_fails=3 fail_timeout=60s;
        server 172.18.111.152 max_fails=3 fail_timeout=60s;
        server 172.18.111.153 max_fails=3 fail_timeout=60s;
        server 172.18.111.154 max_fails=3 fail_timeout=60s;
        # if fail load 3 times , set server unavaiable to fail_timeout 60s
}

server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name lbl.net;

    location / {
        proxy_pass http://mywebapp1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
>nginx Configuration for backend
```
nano /etc/nginx/sites-enabled/default

server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name server_domain_or_IP;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
>Backend Script Testing
```
<?php
    header( 'Content-Type: text/plain' );
    echo 'Host: ' . $_SERVER['HTTP_HOST'] . "\n";
    echo 'Remote Address: ' . $_SERVER['REMOTE_ADDR'] . "\n";
    echo 'X-Forwarded-For: ' . $_SERVER['HTTP_X_FORWARDED_FOR'] . "\n";
    echo 'X-Forwarded-Proto: ' . $_SERVER['HTTP_X_FORWARDED_PROTO'] . "\n";
    echo 'Server Address: ' . $_SERVER['SERVER_ADDR'] . "\n";
    echo 'Server Port: ' . $_SERVER['SERVER_PORT'] . "\n\n";
?>
```
