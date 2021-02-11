# Use docker compose up multiple website
## Step 1. Create a project a and b
```
|__nginx
     |__a
     |  |__index.html
     |
     |__b
        |__index.html
```
輸入你想輸入的內容

## Step 2. Create a and b's docker-compose.yml
```
|__nginx
     |__a
     |  |__index.html
     |  |__docker-compose-a.yml
     |
     |__b
        |__index.html
        |__docker-compose-b.yml
```
a b 專案不能同時用 `./`，會導致兩個專案網頁都掛到 a 專案，所以目前我們位置先寫死，晚點再改成相對位置

### docker-compose-a.yml
```
version: '3'
services:
  prj_a: # other containers can call by this name
    image: nginx
    volumes:
      - /Users/awesome/Documents/Git/nginx/a/:/usr/share/nginx/html
```

### docker-compose-b.yml
```
version: '3'
services:
  prj_b:
    image: nginx
    volumes:
      - /Users/awesome/Documents/Git/nginx/b:/usr/share/nginx/html
```

## Step 3. Create middle's docker-compose.yml
```
|__nginx
     |__a
     |  |__index.html
     |  |__docker-compose-a.yml
     |
     |__b
     |  |__index.html
     |  |__docker-compose-b.yml
     |
     |__middle
        |__docker-compose.yml
```
```
version: '3'
services:
  nginx:
    container_name: aws_middle
    image: nginx
    volumes:
      - ~/Documents/Git/nginx/conf.d:/etc/nginx/conf.d
    ports:
      - "80:80" # local port:docker container port
```

```
volumes:
      - ~/Documents/Git/nginx/conf.d:/etc/nginx/conf.d

docker nginx.conf 會預設 include conf.d 資料夾，所以只要將 a b 的 conf 檔放在同一個資料夾掛上 volume 就可以了。之後若有新的網頁只要將 conf 檔放進這個資料夾即可
```

## Step 4. Create middle_a.conf and middle_b.conf
```
|__conf.d
     |__middle_a.conf
     |__middle_b.conf
```
This file will connect between middle and a, b.
### middle_a.conf
```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

server {
	listen       80;
	server_name  one.prj.com;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        proxy_pass http://prj_a;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_set_header Host $host;
	}
}
```

### middle_b.conf
```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

server {
	listen       80;
	server_name  two.prj.com;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        proxy_pass http://prj_b;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_set_header Host $host;
	}
}

```

## Step 5. Docker compose up
現在我們可以 docker compose 並將他們連接起來
```shell
$ docker-compose -f ~/Documents/Git/nginx/a/docker-compose-a.yml -f ~/Documents/Git/nginx/b/docker-compose-b.yml -f ~/Documents/Git/nginx/middle/docker-compose.yml up -d
```
如果有錯誤可以不下 `-d`，來顯示錯誤

---
顯示該 docker 的 log
```
$ docker log -f aws_middle
```
檢查 vomues 是否正常運作
```
$ docker exec -it aws_middle /bin/bash/
```
