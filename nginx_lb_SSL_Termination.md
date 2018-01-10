>Generate Keys And Create An SSL Certificate
```
#Create the SSL certificate directory
mkdir -p /etc/nginx/ssl/example.com

#Create a private key
openssl genrsa -des3 -out server.key 2048

#Remove its passphrase
openssl rsa -in server.key -out server.key

#Create a CSR (Certificate Signing Request)
openssl req -new -key server.key -out server.csr

# generate a self-signed certificate 
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

```
>Create a virtual hosts file inside the Nginx directory
```
nano /etc/nginx/sites-available/example.com

add:

upstream mywebapp1 {
    server 10.130.227.11;
    server 10.130.227.22;
}

server {
    listen 80;
    listen 443 ssl;
    server_name example.com www.example.com;

    ssl on;
    ssl_certificate         /etc/nginx/ssl/example.com/server.crt;
    ssl_certificate_key     /etc/nginx/ssl/example.com/server.key;
    ssl_trusted_certificate /etc/nginx/ssl/example.com/ca-certs.pem;

    ssl_session_cache shared:SSL:20m;
    ssl_session_timeout 10m;

    ssl_prefer_server_ciphers       on;
    ssl_protocols                   TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers            ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS;

    add_header Strict-Transport-Security "max-age=31536000";
    
    location / {
        proxy_pass http://mywebapp1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
> backend servers test script
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
