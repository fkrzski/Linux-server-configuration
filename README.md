# INFO
- Guide created using Debian 11 
- If you don't have `apt` package manager maybe you have [one of this package managers](https://www.geekboots.com/story/list-of-linux-package-manager-and-their-utility)
- If you have any suggestions create an Issue :D 


# PHP
#### 1. Update and upgrade apt package manager
```shell
sudo apt update && sudo apt upgrade -y
```

#### 2. Install temporary packages that help you to install PHP 8.1
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

#### 5. Update apt package manager
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


# Composer
#### 1. Update and upgrade apt
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

#### 5. Check Composer version
```shell
sudo composer --version  
```



# MySQL
Installation and basic configuration of MySQL and MySQL server
> ## Default version
1. Update and upgrade apt
```shell
sudo apt update && sudo apt upgrade -y
```

2. MySQL
```shell
sudo apt install mysql
```

3. Refresh `apt`
```shell
sudo apt update
```

4. Install MySQL server
```
sudo apt install mysql-server -y
```

> ## Specific version
1. Update and upgrade apt
```shell
sudo apt update && sudo apt upgrade -y
```

2. Install Gnupg and wget package
``` shell
sudo apt install gnupg wget -y
```

3. Select your MySQL version 
Select and copy link from [here](https://dev.mysql.com/downloads/)

4. Download MySQL
```shell
# Example link, you put your link after wget
wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
```

5. Install MySQL
Run your dpkg package with the `-i` flag, that indicates that you’d like to install from the specified file and `*` char depends all files that's name start from `mysql-apt-config`
```shell
sudo dpkg -i mysql-apt-config*
```
When you see screen with MySQL and MySQL tools select menu, press TAB and ENTER. 

6. Refresh `apt`
```shell
sudo apt update
```

7. Install MySQL server
```
sudo apt install mysql-server -y
```
During installation you must set root account password and select authentication plugin. I recommend `Strong password encryption`

> ## Configuration  of MySQL
1. Run secure installation
```shell
mysql_secure_installation
```
During installation, you need to select the password validation level, choose whether you want to change the root password (if set), delete anonymous users or not, prohibit remote root login, delete the test database and reload the permissions. <br>
I recommend: 2, No (if set), Yes, Yes, Yes, Yes

# phpMyAdmin
> ## Install phpMyAdmin
1. Update and upgrade apt
```shell
sudo apt update && sudo apt upgrade -y
```

2. Install phpMyAdmin and required packages
```shell
sudo apt install wget php8.1 php8.1-cgi php8.1-mysqli php8.1-pear php8.1-mbstring libapache2-mod-php php8.1-common php8.1-phpseclib php8.1-mysql phpmyadmin -y
```
You can choose your PHP version

> ## Resolve `Deprecated...` warnings and errors
1. Go to folder with phpMyAdmin
```shell
cd /usr/share
```

2. Create backup of phpMyAdmin
`-r` flag is `Recursive` attribute
```shell
sudo cp -r phpmyadmin/ phpmyadminold/
```

3. Remove olf folder
```shell
sudo rm -rf phpmyadmin
```

4. Choose phpMyAdmin version <br>
Go to [phpMyAdmin version list](https://www.phpmyadmin.net/files/), select wanted version and copy link.

5. Unzip downloaded folder
```shell
sudo unzip phpMyAdmin*
```

6. Copy downloaded folder
```shell
# X.X.X is your version of phpMyAdmin
cp -r phpMyAdmin-X.X.X-all-languages phpmyadmin
```

# Apache2 virtualhosts
1. Create a domain directory
```shell
sudo mkdir /var/www/html/domain.com
```

2. Change owner and mod of domain
```shell
sudo chown -R www-data:www-data /var/www/html/my_domain.com
sudo chmod -R 755 /var/www/html/my_domain.com
 ```
 
3. Create a configuration file for a domain
```shell
sudo nano /etc/apache2/sites-available/my_domain.com.conf
```

4. Put to .conf file this content
```shell
<VirtualHost *:80>
     ServerAdmin admin@my_doamin.com
     ServerName my_doamin.com
     ServerAlias www.my_doamin.com

     DocumentRoot /var/www/html/my_doamin.com/public/

     <Directory /var/www/html/my_doamin.com/public>
         Options Indexes FollowSymLinks
         AllowOverride All
         Require all granted
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
     
# SSL certificate, from HTTP to HTTPS
RewriteEngine on
RewriteCond %{SERVER_NAME} =my_doamin.com [OR]
RewriteCond %{SERVER_NAME} =www.my_doamin.com
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

5. Restart Apache2
```shell
sudo systemctl restart apache2
```

6. Enable site in `a2ensite`
```shell
sudo a2ensite my_domain.com.conf
```

7. (If enabled) disable default `a2ensite` website
```shell
sudo a2dissite 000-default.conf
```

8. RELOAD Apache2
```shell
sudo systemctl reload apache2
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



# Git
1. Update and upgrade apt
```shell
sudo apt update && sudo apt upgrade -y
```

2. Install Git
```shell
sudo apt install git
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


# Certbot
> ## Install Certbot
1. Update and upgrade apt
```shell
sudo apt update && sudo apt upgrade -y
```
2. Install snapd
```shell
sudo apt install snapd -y
```
3. Install snap core
```shell
sudo snap install core
```
4. Refresh shanp core
```shell
sudo snap refresh core
```
5. Install certbot
```shell
sudo snap install --classic certbot
```
5. Add symbolic link
```shell
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

> ## Add certificate to website
```shell
sudo certbot --apache --agree-tos --redirect -m email@email.com -d my_domain.com -d www.my_domain.com
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
