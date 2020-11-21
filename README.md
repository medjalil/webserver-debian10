[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=flat-square&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/mohamed-abdeljalil-70280362/

<img src="https://www.debian.org/logos/openlogo-100.jpg" align="right">
<img src="https://symfony.com/logos/symfony_black_03.png" align="right" width="100px">

[![LinkedIn][linkedin-shield]][linkedin-url]
# Install Web Server on Debian 10 for Symfony App
<!-- TABLE OF CONTENTS -->
 ## Table of Contents
* [Installation](#installation)
  * [Update password root](#update-password-root)
  * [Update debian](#update-debian)
  * [Install apache2](#install-apache2)
  * [Install php7.4](#install-php74)
  * [Install mariadb 10.4](#install-mariadb-104)
  * [Install phpmyadmin](#install-phpmyadmin)
  * [Install composer](#install-composer)
  * [Install git](#install-git)
  * [Install npm & yarn](#install-npm--yarn)
  * [Install symfony CLI](#install-symfony-cli)
* [Configuration](#configuration)
  * [virtualhost apache2 configuration](#virtualhost-apache2-configuration)
  * [access configuration](#access-configuration)
  * [Install firewall](#install-firewall)
  * [HTTPS with Lent's Encrypt](#https-with-lents-encrypt)

## Installation
### Update password root
```sh
sudo passwd root
su root
```
### Update debian
```sh
apt-get -y update 
apt-get -y upgrade
apt-get -y dist-upgrade
apt-get -y autoremove
```
### Install apache2
```sh
apt-get -y install apache2
```
### Install php7.4
```sh
apt-get -y install lsb-release apt-transport-https ca-certificates
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
apt-get -y update
apt-get -y install php7.4
apt-get -y install php7.4-{bcmath,bz2,intl,gd,mbstring,mysql,zip,xml}
```
You can create test file ``` nano /var/www/html/info.php ```

Add this code ```php <?php phpinfo(); ?> ```

Go to : ip_server/info.php 

Done :+1: 

### Install mariadb 10.4
```sh
sudo apt -y update
sudo apt -y install software-properties-common gnupg2
sudo apt -y upgrade

sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xF1656F24C74CD1D8
sudo add-apt-repository 'deb [arch=amd64] http://mariadb.mirror.liquidtelecom.com/repo/10.4/debian buster main'

sudo apt update
sudo apt install mariadb-server mariadb-client

sudo mysql_secure_installation
```
### Install phpmyadmin
```sh
cd /var/www/html/
wget https://files.phpmyadmin.net/phpMyAdmin/5.0.4/phpMyAdmin-5.0.4-english.tar.gz
tar xvf phpMyAdmin-5.0.4-english.tar.gz 
rm phpMyAdmin-5.0.4-english.tar.gz
mv phpMyAdmin-5.0.4-english /var/www/html/phpmyadmin
sudo service apache2 restart
```
### Install composer
```sh
sudo apt update
sudo apt install wget php-cli php-zip unzip
wget -O composer-setup.php https://getcomposer.org/installer
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
sudo composer self-update
```
### Install git
```sh
sudo apt update
sudo apt install git
git --version
git config --global user.name "Your Name"
git config --global user.email "youremail@yourdomain.com"
git config --list
```
### Install npm & yarn
```sh
sudo apt update
sudo apt install build-essential curl
curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -
sudo apt install nodejs
node --version
sudo apt-get update 
sudo apt-get install yarn
```
### Install symfony CLI
```sh
wget https://get.symfony.com/cli/installer -O - | bash
mv /root/.symfony/bin/symfony /usr/local/bin/symfony
symfony check:requirements
```
## Configuration
### virtualhost apache2 configuration
```sh
cd /etc/apache2/sites-available/
sudo nanao yourdomain.conf
```
```apacheconf
<VirtualHost *:80>
        ServerName yourdomaine
        ServerAlias www.yourdomaine
    
        DocumentRoot /var/www/html/yourdomaine
        DirectoryIndex /index.php
    
        <Directory /var/www/html/yourdomaine>
            AllowOverride None
            Order Allow,Deny
            Allow from All
    
            FallbackResource /index.php
        </Directory>
        ErrorLog /var/log/apache2/project_error.log
        CustomLog /var/log/apache2/project_access.log combined
    </VirtualHost>
```
```sh
sudo a2ensite yourdomaine
sudo service apache2 restart
```
### access configuration 
```sh
 cd /var/www/
 sudo chown -Rv root:$USER .
 sudo chmod -Rv g+rw .
```
### install firewall
```sh
sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow ssh
sudo ufw allow 22
sudo ufw allow 2222
sudo ufw enable

sudo ufw allow 'OpenSSH'
sudo ufw allow 'WWW Full'
sudo ufw delete allow 'WWW'
```
### HTTPS with Lent's Encrypt
```sh
sudo apt update
sudo apt install snapd
sudo snap install core
sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo systemctl reload apache2

sudo certbot --apache -d your_domain -d www.your_domain

sudo certbot renew --dry-run

```





