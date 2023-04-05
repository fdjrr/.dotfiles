## Packages

| Name | Description |
| ---- | -------------------------------- |
| aria2 | Download utility that supports HTTP(S), FTP, BitTorrent, and Metalink |
| vim | Vi Improved, a highly configurable, improved version of the vi text editor |
| zsh | Very advanced and programmable command interpreter (shell) for UNIX |
| php | PHP (recursive acronym for PHP: Hypertext Preprocessor) is a widely-used open source general-purpose scripting language that is especially suited for web development and can be embedded into HTML. |
| composer | A Dependency Manager for PHP |
| nodejs | Node.js® is an open-source, cross-platform JavaScript runtime environment. |
| npm | Relied upon by more than 17 million developers worldwide, npm is committed to making JavaScript development elegant, productive, and safe. |\
| python | Python is powerful... and fast; plays well with others; runs everywhere; is friendly & easy to learn; is Open. |
| virtualenv | virtualenv is a tool to create isolated Python environments. |
| mysql | MySQL is a database management system. |
| apache2 | Apache is the most widely used webserver software |
| yt-dlp | yt-dlp tries to parse the external downloader outputs into the standard progress output if possible (Currently implemented: aria2c ). |
| rabbitmq | RabbitMQ is the most widely deployed open source message broker. |

## Installation

Install required packages

```
$ sudo su
$ apt-get update && apt-get upgrade -y
$ apt-get install aria2c vim zsh php8.1 nodejs npm python3 python3-pip apache2 mysql-server unzip zip
$ apt-get update && apt-get install -y software-properties-common
$ apt-get update

# Composer
# Visit : https://getcomposer.org/download/
# Following the Command-line installation

$ reboot
```

## Configuration

```
# php configuration

# if your distro is Debian, Following the step.

$ echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/sury-php.list
$ curl -fsSL  https://packages.sury.org/php/apt.gpg| sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/sury-keyring.gpg

# if your distro is Ubuntu

$ sudo apt-get install php8.1-intl php8.1-gd php8.1-dom php8.1-zip php8.1-curl php8.1-opcache php8.1-mbstring php8.1-mysqli php8.1-dev php-pear

# If you want connect MS-SQL

# If you using Ubuntu 

if ! [[ "16.04 18.04 20.04 22.04" == *"$(lsb_release -rs)"* ]];
then
    echo "Ubuntu $(lsb_release -rs) is not currently supported.";
    exit;
fi

sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

curl https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install -y msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install -y mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install -y unixodbc-dev

# If you using Debian

sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Debian 9
curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 10
curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 11
curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install -y msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install -y mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install -y unixodbc-dev
# optional: kerberos library for debian-slim distributions
sudo apt-get install -y libgssapi-krb5-2

sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.1/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.1/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 8.1 sqlsrv pdo_sqlsrv

# Restart the apache
```

```
# aria2c configuration 

# Dont forget to backup the aria2.conf file.

$ sudo mv -f /etc/aria2.conf /etc/aria2.conf.bak
$ sudo ln -s aria2.conf /etc/aria2.conf
```

```
# vim configuration

# Dont forget to backup the .vimrc file.

$ sudo mv -f ~/.vimrc ~/.vimrc.bak
$ sudo ln -s .vimrc ~/.vimrc  
```

```
# zsh configuration

# Dont forget set default sh to zsh.

$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# change theme to agnoster
ZSH_THEME="agnoster"

# enable correction
ENABLE_CORRECTION="true"

# export PATH python
export PATH=~/.local/bin:$PATH

# uncomment the line
alias zshconfig="vim ~/.zshrc"
alias ohmyzsh="vim ~/.oh-my-zsh"
```

```
# mysql configuration 

# First change the authentication plugin in my.cnf file

$ nano /etc/mysql/mysql.conf/mysqld.cnf

[mysqld]

default_authentication_plugin=mysql_native_password

# Restart the mysql service
$ sudo systemctl restart mysql

# Run this command on mysql
$ sudo mysql
$ SET GLOBAL validate_password.LENGTH = 4;
$ SET GLOBAL validate_password.policy = 0;
$ SET GLOBAL validate_password.mixed_case_count = 0;
$ SET GLOBAL validate_password.number_count = 0;
$ SET GLOBAL validate_password.special_char_count = 0;
$ CREATE USER '{username}'@'{host}' IDENTIFIED WITH mysql_native_password BY '{password}';
$ GRANT ALL PRIVILEGES ON *.* TO '{username}'@'{host}';
$ FLUSH PRIVILEGES;
```

```
# apache2 configuration

# enable a2enmod rewrite
$ sudo a2enmod rewrite

# either change the root directory of Apache or move the project to `/var/www/html`.

# to change Apache's root directory, run:

$ cd /etc/apache2/sites-available

# then open the 000-default.conf file using the command:

$ sudo vim 000-default.conf

<VirtualHost *:80>
  ServerName {subdomain}
  DocumentRoot "{path}"
  <Directory />
    Options Indexes FollowSymLinks Includes ExecCGI
    AllowOverride All
    Require all granted
  </Directory>
</VirtualHost>

# then restart the apache server:

$ sudo systemctl restart apache2

# restart apache

$ sudo systemctl restart apache2
```

```
# yt-dlp configuration

# Dont forget to backup config file
$ mv ~/.config/yt-dlp/config ~/.config/yt-dlp/config.bak

$ mkdir .config/yt-dlp
$ cp yt-dlp/config ~/.config/yt-dlp/config
```

```
# rabbitmq-server configuration

$ sudo apt-get install -y erlang-base erlang-asn1 erlang-crypto erlang-eldap erlang-ftp erlang-inets erlang-mnesia erlang-os-mon erlang-parsetools erlang-public-key erlang-runtime-tools erlang-snmp erlang-ssl erlang-syntax-tools erlang-tftp erlang-tools erlang-xmerl
$ sudo apt-get install rabbitmq-server -y --fix-missing

# enable management plugin

$ sudo rabbitmq-plugins enable rabbitmq_management

# create user

$ sudo rabbitmqctl add_user deltamas Delta@123!
$ sudo rabbitmqctl set_user_tags deltamas administrator
```