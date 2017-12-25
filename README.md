# phpmyadmin-on-LEMP
install phpmyadmin on Ubuntu 16.04, Nginx, MariaDB 10, PHP

## Prepare LEMP  
L = Ubuntu 16.04.3 LTS  
E = Nginx 1.10.3  
M = MariaDB 10.2.11  
P = PHP 7.0.22

```
# purge apache (including config files) and install Nginx instead
sudo apt purge apache2*   
sudo apt install -y nginx 

# install MariaDB
sudo apt-get install software-properties-common
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
## Using a repo provided by digitalocean 
sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://nyc2.mirrors.digitalocean.com/mariadb/repo/10.2/ubuntu xenial main'
sudo apt update
sudo apt install -y mariadb-server

# install PHP and php-mysql 
sudo apt install -y php php-fpm php-mysql
```

configure nginx to support PHP  
`sudo vim /etc/nginx/sites-available/default`  
update / uncomment below lines:  
```
server {
	
	        index index.php index.html index.htm index.nginx-debian.html;
	        location ~ \.php$ {
	                include snippets/fastcgi-php.conf;
	                fastcgi_pass unix:/run/php/php7.0-fpm.sock;
	        }
	
	        location ~ /\.ht {
	                deny all;
	        }
}
```
Restart Nginx 
`sudo systemctl restart nginx.service`  

## install and setup phpmyadmin 
`sudo apt install -y phpmyadmin`  
- Do NOT select any item, jump to 'Ok' button directly at page "Web server to reconfigure automatically"  
- Configure database for phpmyadmin with dbconfig-common?  / YES  
- MySQL application password for phpmyadmin:  / A new password for user phpmyadmin  

point http://localhost/phpmyadmin to folder /usr/share/phpmyadmin/  
`sudo ln -s /usr/share/phpmyadmin/ /var/www/html/phpmyadmin`

## todo: security imporve  
... 

