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

## Installation

Install required packages

```
$ sudo su
$ apt-get update && apt-get upgrade -y
$ apt-get install aria2c vim zsh php8.1 nodejs npm python3 python3-pip apache2 mysql-server unzip zip

# Composer
# Visit : https://getcomposer.org/download/
# Following the Command-line installation

$ reboot
```

## Configuration

```
# php configuration

sudo apt-get install php8-1-intl php8-1-gd php8-1-dom php8-1-zip php8-1-curl php8-1-opcache php8-1-mbstring php8-1-mysqli
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
$ CREATE USER `{username}`@`{host}` IDENTIFIED WITH mysql_native_password BY `{password}`
$ GRANT ALL PRIVILEGES ON *.* TO '{username}'@'{host}';
$ FLUSH PRIVILEGES; 
```

```
# apache2 configuration

# enable a2enmod rewrite
$ sudo a2enmod rewrite
```

Either change the root directory of Apache or move the project to `/var/www/html`.

To change Apache's root directory, run:

```
$ cd /etc/apache2/sites-available
```

Then open the 000-default.conf file using the command:

```
$ nano 000-default.conf
```

Edit the DocumentRoot option:

```
DocumentRoot /home/{username}
```

Then restart the apache server:

```
$ sudo service apache2 restart
```

If you get Forbidden You don't have permission to access / on this server after changing the root of apache then do follow these steps

```
$ sudo cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.bak
$ sudo vim /etc/apache2/apache2.conf

# It should look like this 

<Directory />
  Options Indexes FollowSymLinks
  AllowOverride All
  Require all denied
</Directory>

# Change it to

<Directory />
  Options Indexes FollowSymLinks Includes ExecCGI
  AllowOverride All
  Require all granted
</Directory>

# And remove the line

<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride None
  Require all granted
</Directory>

# Restart apache

$ sudo systemctl restart apache2
```

```
# yt-dlp configuration

# Dont forget to backup config file
$ mv ~/.config/yt-dlp/config ~/.config/yt-dlp/config.bak

$ mkdir .config/yt-dlp
$ cp yt-dlp/config ~/.config/yt-dlp/config
```