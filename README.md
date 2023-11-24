# GLPI-PROJECT

## GLPI Installation Guide on Debian

This guide provides detailed instructions for installing and configuring GLPI (IT and asset management software) on a Debian-based system.

## Table of Contents

- [Prerequisites](#prerequisites)
- [VirtualBox configuration](#virtualbox-configuration)
- [Update the System](#update-the-system)
- [Install Apache Web Server](#install-apache-web-server)
- [Install MySQL](#install-mysql)
- [Install PHP and Extensions](#install-php-and-extensions)
- [Configure Database](#configure-database)
- [Download and Install GLPI](#download-and-install-glpi)
- [Apache Configuration](#apache-configuration)
- [Finalizing GLPI Installation](#finalizing-glpi-installation)
- [Support and Documentation](#support-and-documentation)

## Prerequisites

- A Debian-based server
- Root or sudo privileges
- Access to a terminal/command line


## VirtualBox configuration

![](https://github.com/Skunsara/GLPI-PROJECT/blob/main/Screenshots/1.png)

![](https://github.com/Skunsara/GLPI-PROJECT/blob/main/Screenshots/2.png)

![](https://github.com/Skunsara/GLPI-PROJECT/blob/main/Screenshots/3.png)

![](https://github.com/Skunsara/GLPI-PROJECT/blob/main/Screenshots/4.png)

![](https://github.com/Skunsara/GLPI-PROJECT/blob/main/Screenshots/5.png)



## Update the System

![](https://github.com/Skunsara/GLPI-PROJECT/blob/main/Screenshots/8.png)

Ensure your system packages are updated.

```sh
sudo apt update
sudo apt upgrade -y
```

## Install Apache Web Server

GLPI requires a web server to function; Apache is a popular choice.

```sh
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
```

## Install MySQL

GLPI stores its data in a SQL database; MySQL or MariaDB can be used.

```sh
sudo apt install default-mysql-server
sudo systemctl start mysql
sudo systemctl enable mysql
```

## Install PHP and Extensions

Install PHP along with the necessary extensions.

```sh
sudo apt install php php-cgi libapache2-mod-php php-common php-pear php-mbstring php-gd php-intl php-mysql php-ldap php-apcu php-xmlrpc php-imap php-curl php-zip php-xml php-cli php-symfony-polyfill-intl-idn -y
```

## Configure Database
![](https://github.com/Skunsara/GLPI-PROJECT/blob/main/Screenshots/9.png)

Login to your database server and create a GLPI database and user.

```sh
sudo mysql -u root -p
```

Run the following SQL commands:

```sh
CREATE DATABASE glpidb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'skunsara'@'localhost' IDENTIFIED BY 'Password1';
GRANT ALL PRIVILEGES ON glpidb.* TO 'skunsara'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
Remember to replace 'password1' with a strong password.

## Download and Install GLPI
![](https://github.com/Skunsara/GLPI-PROJECT/blob/main/Screenshots/Capture%20d'%C3%A9cran%202023-11-24%20000329.png)

Fetch the latest release of GLPI and set up.

```sh
wget https://github.com/glpi-project/glpi/releases/download/10.0.10/glpi-10.0.10.tgz
tar -zxvf glpi-10.0.10.tgz
sudo mv glpi /var/www/html/
```

Set the appropriate permissions for the GLPI directory.

```sh
sudo chown -R www-data:www-data /var/www/html/glpi
sudo chmod -R 755 /var/www/html/glpi
```

## Apache Configuration

Configure Apache to serve the GLPI site.

Create a new Apache site configuration:

```sh
sudo nano /etc/apache2/sites-available/glpi.conf
```

Add the following configuration:

```sh
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/glpi
    ServerName your_domain_or_IP

    <Directory /var/www/html/glpi>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Enable the site and restart Apache:

```sh
sudo a2ensite glpi.conf
sudo systemctl restart apache2
```

## Finalizing GLPI Installation

![](https://github.com/Skunsara/GLPI-PROJECT/blob/main/Screenshots/12.png)

Navigate to your GLPI installation via a web browser to complete the setup process using the web interface.

```sh
http://your_domain_or_IP

```

Follow the on-screen instructions to complete the GLPI installation.

## Support and Documentation

For further assistance, consult the GLPI documentation or reach out to the GLPI community forums.

https://glpi-project.org/documentation/
