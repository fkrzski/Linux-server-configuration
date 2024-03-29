# INFO
- Guide created using Debian 11 
- If you don't have `apt` package manager maybe you have [one of this package managers](https://www.geekboots.com/story/list-of-linux-package-manager-and-their-utility)
- If you have any suggestions create an Issue :D 


# PHP
## Installation

#### 1. Update and upgrade `apt` package manager
```shell
sudo apt update && sudo apt upgrade -y
```

#### 2. Install required packages
```shell
sudo apt install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
```

#### 3. Add Ondrej Surý repository to sources list to be able to download and install PHP 8.*
```shell
sudo add-apt-repository ppa:ondrej/php
```

#### 4. Update `apt` package manager
```shell
sudo apt update
```

#### 5. Install PHP
You can replace with any other version of PHP, for example `php5.6`
```shell
sudo apt install php8.1 -y 
```

#### 6. Check installation
```shell
php --version
```

#### 7. Optional most used packages
```shell
sudo apt install php8.1-{bcmath,xml,fpm,mysql,zip,intl,ldap,gd,cli,bz2,curl,mbstring,pgsql,opcache,soap,cgi} -y
```

## Optional configuration
All changes are in `php.ini` file. If you want set changes for CLI you need to update `.ini` file from `/etc/php/8.1/cli/` directory. But if you want changes for websites you need to change `.ini` file from `/etc/php/8.1/apache2/` directory
```shell
sudo nano /etc/php/8.1/cli/php.ini
```

After changing in `apache2` directory you must ***ALWAYS*** restart Apache2 server
```shell
sudo systemctl restart apache2
```

In `nano` you can go to specific line using CTRL + SHIFT + - , typing line number and pressing ENTER

#### Bigger POST upload size
```shell
post_max_size = 16M # Line 698
```

#### Bigger file upload size
```shell
upload_max_filesize = 16M # Line 850
```

#### More memory limit
```shell
memory_limit = 256M # Line 430
```


# Composer
#### 1. Update and upgrade `apt` package manager
```shell
sudo apt update && sudo apt upgrade -y
```

#### 2. Install required PHP packages
Replace `php8.1` with yours PHP version
```shell
sudo apt install wget php8.1-cli php8.1-zip unzip -y
```

#### 3. Download Composer installer
```shell
wget -O composer-setup.php https://getcomposer.org/installer
```

#### 4. Install Composer globally (For all users)
You can change installation directory by changing `/usr/local/bin` in `--install-dir=` parameter
```shell
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

#### 5. Update Composer
```shell
sudo composer self-update  
```

#### 6. Check Composer version
```shell
sudo composer --version  
```


# MySQL
Installation and basic configuration of MySQL and MySQL server

## Installation
#### 1. Update and upgrade `apt` package manager
```shell
sudo apt update && sudo apt upgrade -y
```

#### 2. Install required packages
``` shell
sudo apt install gnupg wget -y
```
#### 3. Download MySQL `.deb` package
You can download another version of MySQL form [here](https://dev.mysql.com/downloads/)
```shell
# Example link, you put your link after wget
wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
```

#### 4. Add downloaded `.deb` package to package manager
Run your dpkg package with the `-i` flag, that indicates that you’d like to install from the specified file and `*` char depends all files that's name start from `mysql-apt-config`
```shell
sudo dpkg -i mysql-apt-config*
```
When you see screen with MySQL and MySQL tools select menu, select mysql-8 and press ENTER, press TAB and ENTER. 

#### 5. Update `apt` package manager
```shell
sudo apt update
```

#### 6. Install MySQL server
```shell
sudo apt install mysql-server -y
```
During installation, you must set root account password and select authentication plugin. I recommend `Strong password encryption`

#### 7. Check installation
```shell
sudo mysql --version
```

## Configuration
#### 1. Run secure installation and configure MySQL
```shell
sudo mysql_secure_installation
```
During installation, you need to configure the password validation level, choose whether you want to change the root password (if set), delete anonymous users or not, prohibit remote root login, delete the test database and reload the permissions. <br>
I recommend: 2, No (if set), Yes, Yes, Yes, Yes


# phpMyAdmin
## Install phpMyAdmin
#### 1. Update and upgrade `apt` package manager
```shell
sudo apt update && sudo apt upgrade -y
```

#### 2. Install `phpMyAdmin` and required packages
Change `php8.1` to your version, for example `php5.6`
```shell
sudo apt install wget php8.1 php8.1-cgi php8.1-mysqli php8.1-mbstring libapache2-mod-php php8.1-common php8.1-mysql phpmyadmin -y
```
During installation, you need to select phpMyAdmin type (Apache or lighthttpd). Press SPACE to select, TAB and ENTER.
Next you must set up password to phpMyAdmin user and confirm it with root password (Set up in MySQL section)

## Configure phpMyAdmin and resolve `Deprecated...` warnings and errors
#### 1. Go to folder with phpMyAdmin
```shell
cd /usr/share
```

#### 2. Create backup of phpMyAdmin
```shell
sudo mv phpmyadmin/ phpmyadminold/
```

#### 3. Choose phpMyAdmin version and download
Go to [phpMyAdmin version list](https://www.phpmyadmin.net/files/), select wanted version and copy link.
```shell
wget https://files.phpmyadmin.net/phpMyAdmin/5.2.0/phpMyAdmin-5.2.0-all-languages.zip
```

#### 4. Unzip downloaded folder
```shell
sudo unzip phpMyAdmin*
```

#### 5. Copy downloaded directory and rename
```shell
sudo cp -r phpMyAdmin-5.2.0-all-languages phpmyadmin

# Optionally you can remove old directory
sudo rm -rf phpmyadminold
```

#### 6. Create `/tmp` directory for phpMyAdmin
```shell
sudo mkdir /usr/share/phpmyadmin/tmp
```

#### 7. Change permission to created directory
```shell
sudo chmod -R 777 /usr/share/phpmyadmin/tmp
```

#### 8. Copy config file for `phpMyAdmin`
```shell
sudo cp /usr/share/phpmyadmin/config.sample.inc.php /usr/share/phpmyadmin/config.inc.php 
```

#### 9. Set up `blowfisz_secret` encrypting property
```shell
sudo nano /usr/share/phpmyadmin/config.inc.php
```

Go to line 16 (Press CTRL + SHIFT + -, type `16` and press ENTER) and set up variable (Must be 32 characters long!)
```shell
$cfg['blowfish_secret'] = '11112222333344445555666677778888'; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */
```


# Apache2 virtualhosts
You need to rename all `domain.com` with yours domain name

#### 1. Create a domain directory
The `-p` flag create a `/websites` folder if you don't have it
```shell
sudo mkdir -p /websites/domain.com
```
 
#### 2. Create a configuration file for a domain
```shell
sudo nano /etc/apache2/sites-available/domain.com.conf
```

#### 3. Configure Virtual Host for your website
```shell
<VirtualHost *:80>
    ServerAdmin admin@domain.com
    ServerName domain.com
    ServerAlias www.domain.com
    
    DocumentRoot /websites/domain.com
    
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    
    <Directory /websites/domain.com>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
     
    # SSL certificate, from HTTP to HTTPS
    RewriteEngine on
    RewriteCond %{SERVER_NAME} =domain.com [OR]
    RewriteCond %{SERVER_NAME} =www.domain.com
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

#### 4. Restart Apache2 server
```shell
sudo systemctl restart apache2
```

#### 5. Enable site using `a2ensite`
```shell
sudo a2ensite domain.com.conf
```

#### 6. Enable `rewrite` module in `a2ensite`
```shell
sudo a2enmod rewrite
```

#### 7. Restart Apache2
```shell
sudo systemctl reload apache2
```

#### 8. Setting up default Virtual Host to works correct
If you want host something on default host (`000-deafult.conf`) - for example: *RoundCube* - you need to set `<Directory>` tag in this Virtual Host like this:
```shell
sudo nano /etc/apache2/sites-available/000-default.conf
```

And under `CustomLog ${APACHE_LOG_DIR}/access.log combined` line you must put this lines
```shell
<Directory /var/www/html>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```


# Certbot
## Installing Certbot

#### 1. Update and upgrade `apt` package manager
```shell
sudo apt update && sudo apt upgrade -y
```

#### 2. Install `snapd` package for another package managing
```shell
sudo apt install snapd -y
```

#### 3. Install `core` package using `snap`
```shell
sudo snap install core
```

#### 4. Refresh `core` using `snap`
```shell
sudo snap refresh core
```

#### 5. Install Let'sEncrypt `certbot` using `snap` package
```shell
sudo snap install --classic certbot
```

#### 6. Add symbolic link between snap installation folder and user sharable folder
```shell
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

## Working with certificates
#### Adding certificate do domain
You need to replace all `domain.com` with your domain name and `email@email.com` with your email address <br><br>
***You need to add new certificate for all subdomains!***
```shell
sudo certbot --apache --agree-tos --redirect -m email@email.com -d domain.com -d www.domain.com
```

If this is your first certificate on machine you need to agree wih TOS - type `Y` and press ENTER


# Git
#### 1. Update and upgrade `apt` package manager
```shell
sudo apt update && sudo apt upgrade -y
```

#### 2. Install Git
```shell
sudo apt install git -y
```

#### 3. Check installation
```shell
sudo git --version
```


# SSH keys
#### 1. Generate SSH key
```shell
sudo ssh-keygen -t rsa
```

#### 2. Get SSH key:
```shell
sudo cat /root/.ssh/id_rsa.pub
```


# NodeJS and NPM
#### 1. Add `.deb` package to package manager
Replace `16.x` with wanted version of NodeJS
```shell
curl -s https://deb.nodesource.com/setup_16.x | sudo bash
```

#### 2. Install NodeJS using `apt` package manager
```shell
sudo apt install nodejs -y
```

#### 3. Check installation
```shell
sudo node -v
```


# Angular CLI
#### 1. Install Angular CLI globally using `-g` flag
```shell
sudo npm install -g @angular/cli
```

#### 2. Check installation
```shell
sudo ng version
```


# Python
## Installation
#### 1. Update and upgrade `apt` package manager
```shell
sudo apt update && sudo apt upgrade -y
```

#### 2. Install required packages
```shell
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev -y
```

#### 3. Download Python from official website
You can select another version from [here](https://www.python.org/ftp/python/)
```shell
wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz
```

#### 4. Unzip downloaded package to install 
```shell
sudo tar -xvf Python-3.10.0.tgz
```

#### 5. Enter Python folder
```shell
cd Python-3.10.0
```

#### 6. Run configure script for Python 
```shell
sudo ./configure --enable-optimizations
```

#### 7. Make Python installation
```shell
sudo make -j 2
```

#### 8. Make alt-install for Python
```shell
sudo make altinstall
```

#### 9. Check installation
```shell
sudo python3.10 --version
```

## PIP3
#### 1. Install required packages
```shell
sudo apt install python3-venv python3-pip -y
```

#### 2. Install PIP and automatically upgrade
```shell
sudo pip3 install --upgrade pip
```

## Change default version of Python
#### 1. Update alternatives for Python
You need to change `/usr/local/bin/python3.10` to path to Python version that you want to set as default. <br>
`1` number is a priority. `1` is the highest.
```shell
sudo update-alternatives --install /usr/bin/python python /usr/local/bin/python3.10 1
```


# Other packages
## Sudo
#### 1. Update and upgrade `apt` package manager
```shell
apt update && apt upgrade -y
```

#### 2. Install `sudo` package
```shell
apt install sudo -y
```

## Firefox
#### 1. Update and upgrade `apt` package manager
```shell
apt update && apt upgrade -y
```

#### 2. Install browser using `apt` package manager
```shell
sudo apt install firefox-esr -y
```


## Geckodriver
#### 1. Update and upgrade `apt` package manager
```shell
apt update && apt upgrade -y
```

#### 2. Download `tar` archive with Geckodriver
```shell
wget https://github.com/mozilla/geckodriver/releases/download/v0.32.0/geckodriver-v0.32.0-linux64.tar.gz
```

#### 3. Extract archive 
```shell
sudo tar -xvzf geckodriver-v0.32.0-linux64.tar.gz
```

#### 4. Make file executable
```shell
sudo chmod +x geckodriver
```
