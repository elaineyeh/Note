# Create multiple websites with different domain in 80 port
```
Both in 80 port
project one -> one.prj.com
project two -> two.prj.com
```
## Step 1. Create a project a and b
```
|__a
|  |__index.html
|
|__b
   |__index.html
```
Display anything you want in html

## Step 2. Create project a and b's default.conf in folder conf.d
The project needs to put in a place where is public. And the group needs to be changed to admin.
```
|__conf.d
      |__a.conf
      |__b.conf
```
### a.conf
```
server {
	listen       80 default_server; # set the port
	server_name  one.prj.com; # set the domain

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
		root   /usr/local/nginx_online/a; # set the website html location
		index  index.html index.htm;
	}
}
```
### b.conf
b.conf is similar than a.conf
```
server {
	listen       80;
    server_name  two.prj.com;
    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
		root   /usr/local/nginx_online/b;
        index  index.html index.htm;
	}
}

```

## Step 3. Include conf.d into default.conf
```
include /usr/local/etc/nginx/conf.d/*;
```
