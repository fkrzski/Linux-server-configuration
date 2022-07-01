# Linux server configuration


## PHP installation
```shell
# Update and upgrade apt
sudo apt update
sudo apt upgrade -y

# Install temporary packages that help you to install PHP 8.1
sudo apt install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2

# Add Ondrej Surý repository to sources list
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/sury-php.list

# Sign GPG key
wget -qO - https://packages.sury.org/php/apt.gpg | sudo apt-key add -

# Update apt
sudo apt update

# Install PHP
sudo apt install php8.1 # You can replace with any other version of PHP
```
