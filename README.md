# Final Project LBE AJK 2020 - Deploy CodeIgniter On NGINX

## Apa itu CodeIgniter?
CodeIgniter adalah sebuah web application network yang bersifat open source yang digunakan untuk membangun aplikasi php dinamis.

CodeIgniter menjadi sebuah framework PHP dengan model MVC (Model, View, Controller) untuk membangun website dinamis dengan menggunakan PHP yang dapat mempercepat pengembang untuk membuat sebuah aplikasi web. [official site](http://codeigniter.com).

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

## Langkah - Langkah Deploy Code Igniter

- Install NGINX Server : 

` sudo apt update`

` sudo apt install nginx`

- Adjusting Firewall :

` sudo ufw app list`
  
` sudo ufw allow 'Nginx HTTP'`
  
` sudo ufw status`
 
- Check Server : 

` systemctl status nginx`

- Kalo Belom Kestart NGINX servernya : 

` sudo systemctl start nginx`

- Install PHP-fpm :

` sudo apt update`

` sudo apt install php-fpm`

- Ubah Hak Akses folder `var/www/` : 

` sudo chmod -R 755 /var/www/`

- Ubah Config di dalam file `/etc/nginx/sites-available/default` :

Pada blok server tambahkan : `index index.php index.html index.htm index.nginx-debian.html;`

Pada blok location hapus comment (aktifkan code) : 

```php
location ~ \.php$ {
    include snippets/fastcgi-php.conf;  
    fastcgi_pass unix:/run/php/php7.4-fpm.sock; 
}
```

- Untuk Check php berfungsi apa belum, create info.php didalam `/var/www/html/` :

isi filenya :

`<?php`

  `phpinfo();`
  
`php?>`

`composer create-project codeigniter4/appstarter` then `composer update` whenever
there is a new release of the framework.

When updating, check the release notes to see if there are any changes you might need to apply
to your `app` folder. The affected files can be copied or merged from
`vendor/codeigniter4/framework/app`.
