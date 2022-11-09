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
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/sury-php.list
```

#### 4. Sign GPG (apt-key) key
```shell
wget -qO - https://packages.sury.org/php/apt.gpg | sudo apt-key add -
```

#### 5. Update `apt` package manager
```shell
sudo apt update
```

#### 6. Install PHP
You can replace with any other version of PHP, for example `php5.6`
```shell
sudo apt install php8.1 -y 
```

#### 7. Check installation
```shell
php --version
```

## Configuration
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

     <Directory /websites/domain.com>
         Options Indexes FollowSymLinks
         AllowOverride All
         Require all granted
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
     
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



# NVM
1. Download NVM
```shell
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | sudo bash 
```

2. Use this (I don't know why, but without this not working xD)
```shell
source ~/.bashrc
```


# NodeJS and NPM
1. Install
```shell
nvm install 16.13.1  # Replace with wanted Node version
```





# Google 2 Factor Authentication (2FA)
> ## Install
1. Update and upgrade apt
```shell
sudo apt update && sudo apt upgrade -y
```

2. Install pacakge
```shell
sudo apt install -y libpam-google-authenticator
```


> ## Configure
1. Enable Google 2FA
```shell
google-authenticator
```
And configure package to you preferences

2. Configure SSH
```shell
sudo nano /etc/ssh/sshd_config

# And change or add these lines
#
UsePAM yes
ChallengeResponseAuthentication yes
PermitRootLogin yes
```

3. Configure SSH PAM
```shell
sudo nano /etc/pam.d/sshd

# Add or change these lines
#
@include common-auth
# two-factor authentication via Google Authenticator
auth   required   pam_google_authenticator.so
```

4. Restart SSH
```shell
sudo systemctl restart ssh
```


# Generate SSH key
```shell
ssh-keygen -t rsa
```

Get key:
```shell
cat /root/.ssh/id_rsa.pub
```


# Python 3.10
> ## Installation
1. Update and upgrade apt
```shell
sudo apt update && sudo apt upgrade -y
```

2. Install required package
```shell
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev -y
```

3. Download
```shell
wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz
```

4. Unpack
```shell
sudo tar -xvf Python-3.10.0.tgz
```

5. Enter folder
```shell
cd Python-3.10.0
```

6. Install
```shell
sudo ./configure --enable-optimizations
```

7. Make Python
```shell
sudo make -j 2
```

8. Nproc
```shell
nproc
```

9. Altinstall
```shell
sudo make altinstall
```


# PIP3
> ## Installation
```shell
sudo apt install python3-venv python3-pip -y

sudo -H pip3 install --upgrade pip
```
