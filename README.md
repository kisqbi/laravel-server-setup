

## Setup Ubuntu 20.04.2 LTS for Laravel 8

### Install NGINX

    sudo apt-get install nginx

create folder:

    sudo mkdir /var/www/[DOMAIN]/htdocs/public

create domain file

    sudo nano /etc/nginx/sites-available/[DOMAIN]

use this config
 
    enter server {
        listen 80;
        listen [::]:80;
        # SSL configuration
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        # Set root directive with your /public Laravel directory
        root /var/www/[DOMANIN]/htdocs/public;
        # Set index directive with index.php
        index index.php;
        # Set server_name directive with the hostname
        server_name [DOMAIN];
        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }
        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
        }
        location ~ /\.ht {
                deny all;
        }
    }
        

enable domain

    sudo ln -s /etc/nginx/sites-available/[DOMAIN] /etc/nginx/sites-enabled/
   
### Install PHP 8

    sudo apt install software-properties-common

>  It provides some useful scripts for adding and removing PPAs.

    sudo add-apt-repository ppa:ondrej/php
    sudo apt update
    sudo apt ugrade
    sudo apt install php8.0-fpm
    php -v
    sudo service php8.0-fpm status


Install  Laravel 8 depedencies

    apt-get install php8.0-mysql php8.0-mbstring php8.0-xml php8.0-bcmath


### Configure Nginx and PHP 8 fpm

create user for the domain ( or deploy user )

adduser deploy  
usermod -aG www-data deploy  
chown -R deploy:www-data /var/www/mydomain/


Install MySql server 8

    sudo apt install mysql-server &&
    sudo mysql_secure_installation


It prompst some question

    The existing password for the user account root has expired. Please set a new password.
    
    New password:
    Re-enter new password:
    
    Remove anonymous users? (Press y|Y for Yes, any other key for No) : y 
    Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y 
    Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y 
    Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y

Restart MYSQL service

    service mysql restart

Completely remove MYSQL from  Ubuntu 20.04.2 LTS 

    sudo apt-get  remove  --purge  mysql*  -y &&
    echo -e "\033[1;32m END: sudo apt-get  remove  --purge  mysql*  -y;\033[0m" &&
    sudo apt-get purge  mysql*  -y &&
    echo -e "\033[1;32m END: sudo apt-get purge  mysql*  -y;\033[0m" &&
    sudo apt-get autoremove -y &&
    echo -e "\033[1;32m END: sudo apt-get autoremove -y;\033[0m" &&
    sudo apt-get autoclean -y &&
    echo -e "\033[1;32m END: sudo apt-get autoclean -y;\033[0m" &&
    sudo apt-get  remove  dbconfig-mysql -y &&
    echo -e "\033[1;32m END: sudo apt-get  remove  dbconfig-mysql -y;\033[0m" &&
    sudo apt-get dist-upgrade -y &&
    echo -e "\033[1;32m END: sudo apt-get dist-upgrade -y;\033[0m"
    .
or line by line: 

sudo apt-get  **remove**  --purge  **mysql***
sudo apt-get purge  **mysql***
sudo apt-get autoremove
sudo apt-get autoclean
sudo apt-get  **remove**  dbconfig-**mysql**
sudo apt-get dist-upgrade
sudo apt-get install  **mysql**-server

enter mysql 

    mysql -u root -p

    CREATE DATABASE [DB_USER];  
    CREATE USER '[DB_USER]'@'localhost' IDENTIFIED BY ' [DB_PASSWORD]';  
    GRANT ALL PRIVILEGES ON * . * TO '[DB_USER]'@'localhost';  
    FLUSH PRIVILEGES;



## Install Certbot

    sudo apt install certbot python3-certbot-nginx

### Adjust Firewall to Allow HTTPS Traffic
The next step is to adjust the firewall to allow HTTPS traffic.

If you followed the Nginx installation guide, you already enabled your firewall to allow Nginx HTTP. As you are adding Let’s Encrypt certificates, you need to configure the firewall for encrypted traffic.

1. To ensure your firewall is active and allows HTTPS traffic, run the command:

