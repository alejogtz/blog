---
layout: post
category: chuletas
slug: Fast ways to run local folder as website
---

- [Using Docker](#using-docker)
  * [httpd2 (Apache Server)](#httpd2--apache-server-)
  * [thttpd](#thttpd)
- [Traditional web servers.](#traditional-web-servers)
  * [Nginx](#nginx)
  * [Apache](#apache)
- [Using Python](#using-python)
- [Using NPM](#using-npm)


# Hot ways to serve your local folder as simple website

## Using Docker

### httpd2 (Apache Server)

Run :

	docker run --rm --name static-web-server -dit \
	-v /home/www/html:/usr/local/apache2/htdocs/ \
	-p 80:80 \
	httpd:2.4

> If you want only share a single file, choose 
> `-v /home/www/html/file.zip:/usr/local/apache2/htdocs/download.zip`

Now, you can open web server on `http://localhost` port `80`

### thttpd

This is a minimal (approx. 1MB) image that runs thttpd (a web server) on port 80. Without any additional configuration it will serve out a single sample page

If you want to serve your own static content with this image, you can use it like this:

	docker run -v /path/to/my/content:/content larsks/thttpd -d /content

> Text extracted from [here](https://hub.docker.com/r/larsks/thttpd/)

## Traditional web servers.

### Nginx

Download zip server 

	$ curl http://nginx.org/download/nginx-1.20.1.zip -o nginx.zip

Unzip the zip 

	$ unzip nginx.zip -d ./server/ ; mv server/nginx-1.20.1 server/html
	$ tree -L 2 ser
	.
	├── conf
	│   ├── fastcgi.conf
	│   ├── fastcgi_params
	│   ├── koi-utf
	│   ├── koi-win
	│   ├── mime.types
	│   ├── nginx.conf
	│   ├── scgi_params
	│   ├── uwsgi_params
	│   └── win-utf
	├── contrib
	│   ├── README
	│   ├── geo2nginx.pl
	│   ├── unicode2nginx
	│   └── vim
	├── docs
	│   ├── CHANGES
	│   ├── CHANGES.ru
	│   ├── LICENSE
	│   ├── OpenSSL.LICENSE
	│   ├── PCRE.LICENCE
	│   ├── README
	│   └── zlib.LICENSE
	├── html
	│   ├── 404.html
	│   ├── 50x.html
	│   ├── index.html
	├── logs
	├── nginx.exe
	└── temp

	2 directories, 18 files

Edit `conf/nginx.conf` and edit `port` and add custom configuration

	$ sed '/listen\s+([0-9]+)/s/80/9393/' conf/nginx.conf
	
```bash
...
 server {
        listen       9393;
        server_name  localhost;

        charset utf-8;

        # access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }
...
...
```

### Apache

If you have apache installed locally, follow next steps to enable a new site configuration.

- [ ] First, make directory for a new subdomain or domain under the same directory as the original website or create one if not exists.


__Examples__:

	sudo mkdir -p /var/www/example.com/
	sudo mkdir -p /var/www/demo-site/ (would become demo.example.com)
	sudo mkdir -p /var/www/example2.com/
	
Grant permission to the new directory:

	sudo chown -R $USER:$USER /var/www/test.com/

Modify permissions to ensure that read access is permitted:

	sudo chmod -R 755 /var/www
	
Copy the default Virtual Host and Customize for Second(sub/new) Domain
clone existing `000-default.conf` under `/etc/apache2/sites-available/`

	sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-demo.conf

> __IMPORTANT::__ The original or the first Virtual Host must be modifed as below

E.g. if `000-joomla.conf` is the first Virtual Host, then:

        ServerName example.com
        ServerAlias www.example.com
		
> NOTE: forgetting to do the step above will cause database or cache conflict.        


E.g. virtualhost for demo.example.com

	<VirtualHost *:80>
		<Directory /var/www/demo>
				Options Indexes FollowSymLinks MultiViews
				AllowOverride All
				Order allow,deny
				allow from all
		</Directory>
		ServerName demo.example.com
		ServerAlias www.demo.example.com
		ServerAdmin webmaster@localhost
		DocumentRoot /var/www/demo
		...
	</VirtualHost>

after that, enable the New Virtual Host Files as below:

E.g:

	sudo a2ensite example.com.conf
	sudo a2ensite 000-demo.conf

then, reload apache

	sudo service apache2 reload
	
> If you are running webserver on localhost then you need to redirect you `/etc/hosts` for

	# /etc/hosts
	127.0.0.1 github.com
	127.0.0.1 example.com
	127.0.0.1 www.example.com

> if you are running webserver on localhost on windows machine, then edit `hosts` files as this [link](https://gist.github.com/zenorocha/18b10a14b2deb214dc4ce43a2d2e2992)


## Using Python

If you have python installed you can simply run this commans to expose files as downloadable

	python -m SimpleHTTPServer 8000
	
> You can see how to use it in the exmaple follow next link [-->> Here <<--](https://appdividend.com/2019/02/06/python-simplehttpserver-tutorial-with-example-http-request-handler/)

## Using NPM

install `http-serve` from shell

```bash
$ npm install --global http-server
```
**usage**

```bash
$  http-server [path] [options]
```

`[path]` defaults to ./public if the folder exists, and ./ otherwise.

Now you can visit http://localhost:8080 to view your server

> Note: Caching is on by default. Add `-c-1` as an option to disable caching.
