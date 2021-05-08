
**Setup Ubuntu 20.04.2 LTS for Laravel 8**

    sudo apt install software-properties-common

>  It provides some useful scripts for adding and removing PPAs.

Install PHP 8

    sudo add-apt-repository ppa:ondrej/php
    sudo apt update
    sudo apt ugrade
    sudo apt install php8.0-fpm
    php -v
    sudo service php8.0-fpm status

Install  Laravel 8 depedencies

    apt-get install php8.0-mysql php8.0-mbstring php8.0-xml php8.0-bcmath


