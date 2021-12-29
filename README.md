## Nginx_1.18.0 & Php & Php_7.4-fpm

#### Step 1 – Installing Nginx-1.18.0
```
$ sudo add-apt-repository ppa:nginx/stable
$ sudo apt-get update
$ sudo apt-get install nginx
```
#### Step 2 – Adjusting the Firewall
```
$ sudo ufw app list
```
You should get a listing of the application profiles:
```
Output 
Available applications: 
  Nginx Full 
  Nginx HTTP 
  Nginx HTTPS 
  OpenSSH
```
```
$ sudo ufw allow 'Nginx HTTP'
$ sudo ufw status
```
#### Step 3 – Checking your Web Server
```
$ systemctl status nginx
```
```
Output
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2020-04-20 16:08:19 UTC; 3 days ago
     Docs: man:nginx(8)
 Main PID: 2369 (nginx)
    Tasks: 2 (limit: 1153)
   Memory: 3.5M
   CGroup: /system.slice/nginx.service
           ├─2369 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─2380 nginx: worker process
```
You can access the default Nginx landing page to confirm that the software is running properly by navigating to your server’s IP address.
```
http://your_server_ip
```
**You should receive the default Nginx landing page:**

![nginx-image](https://assets.digitalocean.com/articles/nginx_1604/default_page.png)

#### Step 4 - Install Php & Php-7.4-fpm

```
$ sudo apt-get update
$ sudo apt -y install software-properties-common
$ sudo add-apt-repository ppa:ondrej/php
$ sudo apt-get update
$ sudo apt-get install php7.4-fpm
```
Check version of php
```
$ php -v
```
You should see
```
$ php -v
PHP 7.4.0beta4 (cli) (built: Aug 28 2019 11:41:49) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0-dev, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.0beta4, Copyright (c), by Zend Technologies
```
After install you need edit nginx file using `nano`or `vim` I use `nano` - `$ sudo nano /etc/nginx/sites-available/default`
add next
```
location ~ \.php$ {
		include snippets/fastcgi-php.conf;
		fastcgi_pass unix:/run/php/php7.4-fpm.sock; # PHP version (php -v command)
	}
```
Save changes and close the file. Check the configuration for validity by running the command in the terminal:
```
sudo nginx -t
```
Apply the configuration changes by running the command in the terminal:
```
sudo service nginx reload
```
#### Testing Nginx with PHP-FPM
```
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```
Check URL `http://<server-ip_or_localhost>/info.php` you must see

![php work](https://kifarunix.com/wp-content/uploads/2020/04/php7.4.png)
