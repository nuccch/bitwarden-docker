# README

## Nginx

config SSL and reverse proxy for bitwarden service.
```shell
server {
    listen       80;
    server_name  bitwarden.zhangsan.org.cn;

    rewrite ^(.*)$  https://$host$1 permanent;
}

server {
    listen 443 ssl;
    server_name  bitwarden.zhangsan.org.cn;

    ssl_certificate  /data/cert_chain.pem;
    ssl_certificate_key  /data/cert_key.key;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_pass_header Server;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;
        proxy_pass http://bitwarden;
    }
}
```

## Start

start bitwarden service by docker compose.
```shell
docker-compose up
```

or 

```shell
docker-compose up -d
```

