# Deploy CodeIgniter On NGINX

## Apa itu CodeIgniter?
CodeIgniter is a powerful PHP framework with a very small footprint, built for developers who need a simple and elegant toolkit to create full-featured web applications. [official site](http://codeigniter.com).

## Requirements

App : 
- Install Nginx or Apache Server
- Install PHP-fpm (FastCGI Process Manager)
- Install Mysql (Optional)
- Install Composer

PHP version 7.2 or higher is required, with the following extensions installed: 

- [intl](http://php.net/manual/en/intl.requirements.php)
- [libcurl](http://php.net/manual/en/curl.requirements.php) if you plan to use the HTTP\CURLRequest library

Additionally, make sure that the following extensions are enabled in your PHP:

- json (enabled by default - don't turn it off)
- [mbstring](http://php.net/manual/en/mbstring.installation.php)
- [mysqlnd](http://php.net/manual/en/mysqlnd.install.php)
- xml (enabled by default - don't turn it off)

## Step by Step How To Install NGINX , PHP

- Install NGINX Server : 

` sudo apt update`

` sudo apt install nginx`

- Adjusting Firewall :

` sudo ufw app list`
  
` sudo ufw allow 'Nginx HTTP'`
  
` sudo ufw status`
 
- Check Server : 

` systemctl status nginx`

![Image of status nginx](/img/capture1.png)

*if Nginx is already running, the status will appear as shown above

- If Nginx has not started yet  : 

` sudo systemctl start nginx`

- Install PHP-fpm :

` sudo apt update`

` sudo apt install php-fpm`

` sudo systemctl restart nginx.service`

- Change Access to folder `var/www/` : 

` sudo chmod -R 755 /var/www/`

- Change config on file `/etc/nginx/sites-available/default` :

Add this line on Server block : `index index.php index.html index.htm index.nginx-debian.html;`

PUse this Line below on PHP Block : 

```php
location ~ \.php$ {
    include snippets/fastcgi-php.conf;  
    fastcgi_pass unix:/run/php/php7.4-fpm.sock; 
}
```

- To check whether the PHP is running or not, go to `/var/www/html/` :

Then create info.php file :

```php
<?php
  phpinfo();
php?>
```

If PHP is already running, there is an info for your php on your nginx server on `localhost/info.php`

## Step by Step to Deploy / Install CodeIgniter on NGINX

- install composer : 

`sudo apt install composer`

`sudo apt install php-curl`

`sudo apt install php-intl`

- Create Config file on `/etc/nginx/sites-available/codeigniter.conf` : 

```php

server { listen 8080 default_server; listen [::]:8080 default_server;

root /var/www/codeigniter/public;

# Add index.php to the list if you are using PHP
index index.php index.html index.htm index.nginx-debian.html;

server_name _;

location / {
	# First attempt to serve request as file, then
	# as directory, then fall back to displaying a 404.
	try_files $uri $uri/ /index.php;
}

# pass PHP scripts to FastCGI server
#
location ~ \.php$ {
	include snippets/fastcgi-php.conf;

#	# With php-fpm (or other unix sockets):
	fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
#	# With php-cgi (or other tcp sockets):
#	fastcgi_pass 127.0.0.1:9000;
}

```

- Create Link to sites-enabled :

`sudo ln -s /etc/nginx/sites-available/codeigniter.conf /etc/nginx/sites-enabled/`

`sudo nginx -t`

`sudo systemctl restart nginx`

- Install CodeIgniter using composer : 

Open Directory Root :

`cd /var/www/`

Install CodeIgniter : 

`composer create-project codeigniter4/appstarter [your-project-name]`


`composer update`


![Image of berhasil deploy](/img/capture2.PNG)


## Reference

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04

https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-ubuntu-18-04
