# Create multiple websites with different domain in 80 port in local
```
Both in 80 port
project one -> one.prj.com
project two -> two.prj.com
```
The project needs to put in a place where Nginx has permission to read.
## Step 1. Create a project a and b
```
|_nginx
    |__a
    |  |__index.html
    |
    |__b
       |__index.html
```
Display anything you want in html  
**Notice the a and b project's file and folder user and group need to be changed. To ensure files and folders can be controlled.**  
**At the same time, you need to ensure the permission of directories**  

## Step 2. Create project a and b's default.conf in folder conf.d
The group needs to be changed to admin.
```
|__etc
    |__nginx
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

## Step4. Modify the hosts file to set the domain
```
$ vim /etc/hosts
```

---
## Reference URL:
[Nginx Base Usage](https://medium.com/tool-s/%E5%AE%89%E8%A3%9D-nginx-on-mac-eb739295e153)  
[Multiple domains on a Nginx server](https://linuxhint.com/install-multiple-domains-nginx-server/)  
