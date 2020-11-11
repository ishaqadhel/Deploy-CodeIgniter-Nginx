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

## Langkah - Langkah Install NGINX , PHP

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

` sudo systemctl restart nginx.service`

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

```php
<?php
  phpinfo();
php?>
```

kalau php sudah terinstall dan sudah run seharusnya ketika ke url `localhost/info.php` seharusnya muncul detail tentang info php dalam page tersebut.

## Langkah - Langkah Deploy / Install CodeIgniter di NGINX

- install composer : 

`sudo apt install composer`

`sudo apt install php-curl`

`sudo apt install php-intl`

- Buat File Config di `/etc/nginx/sites-available/codeigniter.conf` : 

```php

server { listen 8085 default_server; listen [::]:8085 default_server;

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
*saya buat nama codeigniter karena nanti project foldernya namanya codeigniter

*untuk server agar punya tempat berbeda dilink ke :8085

- Buat Link ke sites-enabled :

`sudo ln -s /etc/nginx/sites-available/codeigniter.conf /etc/nginx/sites-enabled/`

`sudo nginx -t`

`sudo systemctl restart nginx`

- Install CodeIgniter menggunakan composer : 

Open Directory Root :

`cd /var/www/`

Install CodeIgniter : 

`composer create-project codeigniter4/appstarter nama-project`

*untuk nama project bisa diganti sesuai kebutuhan, disini saya menggunakan nama codeigniter

`composer update`

Seharusnya, jika tidak ada error berhasil install dan deploy CodeIgniter.

## Kesimpulan

Untuk menginstall CodeIgniter diperlukan Server (Nginx / Apache), php, mysql(optional), serta composer. Untuk caranya menginstallnya pun kadang berbeda sesuai kondisi device / server masing-masing, kadang diperlukan cara tambahan ketika terjadi error ditengah step instalasi.
