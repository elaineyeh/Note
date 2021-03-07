# Use SSL certificate
在 project_a 跟 project_b 掛上 SSL 憑證
```
__.crt 憑證
__.key 私鑰
```
## Step 1. Put the .crt and .key file to ssl folder
```
|__ssl
     |__file.crt
     |
     |__file.key
```

## Step 2. Hang the SSL certificate into aws_prj_a.conf and aws_prj_b.conf
### aws_prj_a.conf and aws_prj_b.conf
#### ssl certificate 的檔案位置是 middle 在 docker 的位置
```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

server {
	listen       80;
	server_name  one.ponddy.com;

	listen 443 ssl;
  	listen [::]:443 ssl;

	ssl_certificate /etc/nginx/ssl/42b90b44ed6060a7.crt;
	ssl_certificate_key /etc/nginx/ssl/ponddy.key;

    location / {
        proxy_pass http://project_a;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_set_header Host $host;
	}
}
```

## Step 3. Add the SSL setting into docker compose-project_middle.yml
將 SSL 憑證檔案掛 volume 到 docker 裡，同時開啟 443 port
```
version: '3'
services:
  project_middle:
    image: nginx
    volumes:
      - /home/ubuntu/conf.d:/etc/nginx/conf.d
      - /home/ubuntu/ssl/:/etc/nginx/ssl
    ports:
      - "80:80"
      - "443:443"
```

## Step 4. Check the localhost and EC2 /etc/hosts setting

### Tips
```
如果 docker 開不起來或是有錯誤
$ docker logs -f container_name
```
```
如果要檢查 docker 裡的檔案
$ docker exec -it container_name /bin/bash
```
