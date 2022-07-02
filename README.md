# PHP installation
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
sudo apt install php8.1 # You can replace with any other version of PHP i. e. php5.6
```
