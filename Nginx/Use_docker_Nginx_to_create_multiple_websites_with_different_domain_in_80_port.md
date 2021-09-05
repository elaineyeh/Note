# Use docker Nginx to create multiple websites with different domain in 80 port
```
Both in 80 port
project one -> one.prj.com
project two -> two.prj.com
```
The project needs to put in a place where Docker has permission to read.
## Step 1. Create a and b project
```
|__Document
     |__nginx
          |__a
          |  |__index.html
          |
          |__b
             |__index.html
```
Display anything you want in html

## Step 2. Run project a and b docker
You can check where you are first.
```
$ echo $PWD
```

### project a
cd to the location where project a's website is.
```shell
$ docker run --name prj_a -v $PWD:/usr/share/nginx/html -d nginx
```
### project b
cd to the location where project a's website is.
```shell
$ docker run --name prj_b -v $PWD:/usr/share/nginx/html -d nginx
```

## Step 3. Write default.conf of middle docker
cd to the location where you like. In this case, I put it together with project a and b.
```
|__Document
     |__nginx
          |__a
          |  |__index.html
          |
          |__b
            |__index.html
     |__default.conf
```

### default.conf
```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

server {
	listen       80 default_server; # set the port
	server_name  one.prj.com; # set the domain. If the domain is correct, execute the command below

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        proxy_pass http://prj_a; # prj_a are the name when run middle docker you set
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade; # set header's Upgrade are line 1 we write
        proxy_set_header Connection $connection_upgrade; # set header's Connection are line 1 we write
        proxy_set_header Host $host; # Keep the url http://one.prj.com
	}
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

## Step 4. Run project middle docker
Use middle docker to connect local and projects (a and b).
```
$ docker run --name middle --link prj_a:prj_a --link prj_b:prj_b -v $PWD/default.conf:/etc/nginx/conf.d/default.conf -p 0.0.0.0:80:80 -d nginx
```
```
--link prj_a:prj_a -> link docker docker you have run:the name you want to call in this docker
-p -> set the ip and port
0.0.0.0:80:80 -> ip is 0.0.0.0, which means all ip can access. And local port is 80 port, docker port is 80 port.
```

## Step4. Modify the hosts file to set the domain
```
$ vim /etc/hosts
```

### If you want to check whether it works, check middle's logs.
```
$ docker logs -f middle
```
Refeash the website and check the terminal.
