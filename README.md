# INFO
- Guide created using Debian 11 
- If you don't have `apt` package manager maybe you have [one of this package managers](https://www.geekboots.com/story/list-of-linux-package-manager-and-their-utility)
- If you have any suggestions create an Issue :D 


# PHP
Installation of PHP package
> ## Default version
1. Update and upgrade apt
```shell
sudo apt update && sudo apt upgrade -y
```

2. Install PHP
```shell
sudo apt install php
```
<br>

> ## Specific version
1. Update and upgrade apt
```shell
sudo apt update && sudo apt upgrade -y
```

2. Install temporary packages that help you to install PHP 8.1
```shell
sudo apt install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
```

3. Add Ondrej Surý repository to sources list
```shell
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/sury-php.list
```

4. Sign GPG key
```shell
wget -qO - https://packages.sury.org/php/apt.gpg | sudo apt-key add -
```

5. Update apt
```shell
sudo apt update
```

6. Install PHP
```shell
sudo apt install php8.1 -y # You can replace with any other version of PHP i. e. php5.6
```


# Composer
> ## Installation
1. Update and upgrade apt
```shell
sudo apt update && sudo apt upgrade -y
```

2. Install required PHP packages
```shell
sudo apt install wget php-cli php-zip unzip -y
```

3. Download installer
```
wget -O composer-setup.php https://getcomposer.org/installer
```

4. Install composer globally
```
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

5. Update composer
```
sudo composer self-update  
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
