## Link de com installar RockyServerDVD

https://www.youtube.com/watch?v=sG-WNFR8z90

![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/f55cce48-91db-48bc-a65d-d1e5c9e13c66)
![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/37c651a3-2a68-49af-92b5-ba4fe2970737)
![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/2f060cb9-45b6-475f-8660-4ae1460aa81f)
![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/5538370c-dd74-4a45-8d5b-955d6c9ea6ba)
![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/501eaa41-4e4b-4011-85c0-208e62b0e8dd)

Server amb entorn gràfic:
![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/95e507e3-8dc0-43b9-88e4-6daa88c2dff2)

Server:
![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/5a6eba08-e9e7-4cd3-8812-a4b060ae242e)


![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/73fe5ebe-c9eb-4cdf-8ca0-47f8dfce3f1a)
![20231003_185007](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/7e54b701-99df-4b12-9e67-6031bdbc174d)

## Que root pugui fer login remote

![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/77b01b96-1ae5-47d2-a991-42bfa2b4103c)

![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/fbf4f98b-916e-424e-98c5-b7cd957004b3)


## Configurar Rocky per installar WordPress

IP: 192.168.1.xxx

MAQUINA VIRTUAL Rocky Linux 9.2 (25/05/2023)

*******************************
user: root

pass: 123456789

user: maria

pass: 123456789

Cero un Usuari(per usar winSCP, no caldria activar passwd):
***********************************************************
useradd maria -d /home/maria -m -s /usr/bin/bash
passwd maria

vim /etc/sudoers (comprovar que es wheel o sudo)
usermod -aG wheel maria (posem maria en el grup wheel)

Pasar de usuario normal a root:
*******************************
[maria@localhost ~]$ su -

Contraseña:
[maria@localhost ~]#


Ver la IP que tenemos:
**********************
[root@localhost ~]# ip addr


Editar la configuración de red: (Y EL HOSTNAME)
*******************************
[root@localhost ~]# nmtui


Ver el hostname:
****************
[root@localhost ~]# hostnamectl 




Desactivar el SELINUX:
**********************
vi /etc/sysconfig/selinux


Posem el SELINUX=disabled

Guardamos el fichero y reiniciamos la máquina.

Podemos ver el estado al reiniciar con:

[root@localhost ~]# sestatus

SELinux status:                 disabled

[root@localhost ~]#


Lo primero un update system:

$ sudo yum update

Updating the system

# sudo dnf -y update


Install EPEL Repository:
************************
First, enable the CRB.

[root@rockylinux9-vm-tonet ~]# sudo dnf config-manager --set-enabled crb

[root@rockylinux9-vm-tonet ~]# sudo dnf install \
    https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm \
    https://dl.fedoraproject.org/pub/epel/epel-next-release-latest-9.noarch.rpm


Instalar también el repositorio REMI para PHP 7.4:
**************************************************
[root@rockylinux9-vm-tonet ~]# sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-9.rpm -y


Install Apache server.
----------------------
[root@localhost ~]# yum -y install httpd

Add to start:

[root@localhost ~]# systemctl enable httpd

[root@localhost ~]# yum install mod_ssl

systemctl start httpd

systemctl stop httpd

systemctl restart httpd

systemctl status httpd


Start FIREWALL
**************

[root@rockylinux9-vm-tonet ~]# systemctl start firewalld


Open firewall port:
-------------------

[root@localhost ~]# sudo firewall-cmd --permanent --add-port="80"/tcp

success


[root@localhost ~]# sudo firewall-cmd --permanent --add-port="443"/tcp

success


[root@localhost ~]# sudo firewall-cmd --reload

success


[root@localhost ~]#


Instalación del MySQL
*********************

[root@rockylinux9-vm-tonet ~]# sudo dnf install mysql-server



Enable and Start MySQL

[root@rockylinux9-vm-tonet ~]# sudo systemctl enable mysqld

Created symlink /etc/systemd/system/multi-user.target.wants/mysqld.service → /usr/lib/systemd/system/mysqld.service.

systemctl start mysqld

systemctl stop mysqld

systemctl restart mysqld

systemctl status mysqld


Secure MySQL in Rocky Linux

[root@rockylinux9-vm-tonet ~]# sudo mysql_secure_installation (dir que no a tot)

root
(el que tu quieras)

Connect to MySQL in Rocky Linux

[root@rockylinux9-vm-tonet ~]# sudo mysql -u root -p

Enter password:

Welcome to the MySQL monitor.  Commands end with ; or \g.

Your MySQL connection id is 10

Server version: 8.0.26 Source distribution

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SELECT VERSION ();
+------------+
| VERSION () |
+------------+
| 8.0.32     |
+------------+
1 row in set (0.00 sec)

mysql>


Install PHP 7.4 on on Rocky Linux
---------------------------------
Check PHP Installation in Rocky Linux

[root@localhost html]# cd /var/www/html/

[root@localhost html]# chown apache:apache wp-fimba/ -R

[root@localhost html]# php -v

bash: php: no se encontró la orden...


Thankfully, Rocky Linux AppStream repositories provide PHP versions from PHP 7.2 which is enabled by default. To get a list of all the hosted PHP modules, run the command.

sudo dnf module list php

To install PHP 7.4, first enable the module as provided.

[root@rockylinux9-vm-tonet ~]# sudo dnf module list php
Last metadata expiration check: 0:01:34 ago on Thu May 25 14:06:06 2023.
Rocky Linux 9 - AppStream
Name                     Stream                       Profiles                                       Summary
php                      8.1                          common [d], devel, minimal                     PHP scripting language

Remi's Modular repository for Enterprise Linux 9 - x86_64
Name                     Stream                       Profiles                                       Summary
php                      remi-7.4                     common [d], devel, minimal                     PHP scripting language
php                      remi-8.0                     common [d], devel, minimal                     PHP scripting language
php                      remi-8.1                     common [d], devel, minimal                     PHP scripting language
php                      remi-8.2                     common [d], devel, minimal                     PHP scripting language

Hint: [d]efault, [e]nabled, [x]disabled, [i]nstalled
[root@rockylinux9-vm-tonet ~]# == [root@localhost ~]#

Activamos la versión 7.4:
[root@rockylinux9-vm-tonet ~]# sudo dnf module enable php:remi-7.4 -y


Una vez activado la versión 7.4, podemos listar de nuevo y lo verificamos.

Iniciar la instalación del PHP 7.4:
[root@rockylinux9-vm-tonet ~]# sudo dnf install php php-cli php-gd php-curl php-zip php-mbstring

Install other commonly required PHP extensions, php-mysqlnd creates a connection to the database server.
[root@rockylinux9-vm-tonet ~]# dnf install php-mysqlnd php-gd php-intl

Ver la versión instalada:
[root@rockylinux9-vm-tonet ~]# php -v
PHP 7.4.33 (cli) (built: May 11 2023 10:38:29) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.33, Copyright (c), by Zend Technologies
[root@rockylinux9-vm-tonet ~]#


Allow execution of files by httpd in the /var/www/html/ directory with an SE Linux policy exception.
# chcon -R -t httpd_sys_content_t /var/www/html/


Install Certbot in Rocky Linux
******************************
[root@rockylinux9-vm-tonet ~]# sudo dnf install certbot python3-certbot-apache



WORDPRESS TEST xxx
********************
mysql -u root -p
mysql> CREATE DATABASE wptest CHARACTER SET utf8 COLLATE utf8_bin;
mysql> CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'elquetuquieras';
mysql> GRANT ALL privileges on wptest.* to wpuser@localhost;

* Importar la BB.DD.:
mysql -u wpuser -p wptest < fimba.sql (pass: elquetuquieras

url admin: http://wptest.bylapera.com/wp-admin/
user: administrador
pass: (elquetuquieras)


WORDPRESS TEST SQUIDFY
**********************
mysql -u root -p

mysql> CREATE DATABASE wpsquidfy CHARACTER SET utf8 COLLATE utf8_bin;

mysql> CREATE USER 'maria'@'localhost' IDENTIFIED BY '123456789';

mysql> GRANT ALL privileges on wpsquidfy.* to maria@localhost;


mysql-name: wpsquidfy

mysql-user: maria

mysql-pass: 123456789


WORDPRESS TEST 2
****************
mysql -u root -p

mysql> CREATE DATABASE wptest2 CHARACTER SET utf8 COLLATE utf8_bin;

mysql> GRANT ALL privileges on wptest2.* to wpuser@localhost;


INSTALL WORDPRESS
*****************
mysql -u root -p

mysql> CREATE DATABASE wp;

mysql> CREATE USER 'wpuser'@'localhost' IDENTIFIED BY '123456789';

mysql> GRANT ALL ON wp.* TO 'wpuser'@'localhost';

mysql> FLUSH PRIVILEGES;

mysql> EXIT;



$ wget https://wordpress.org/latest.tar.gz -O wordpress.tar.gz

$ tar -xvf wordpress.tar.gz

$ sudo cp -R wordpress /var/www/html/


$ sudo chown -R apache:apache /var/www/html/wordpress

$ sudo chmod -R 775 /var/www/html/wordpress

$ sudo semanage fcontext -a -t httpd_sys_rw_content_t "/var/www/html/wordpress(/.*)?"

$ sudo restorecon -Rv /var/www/html/wordpress


crear fitxer
************
$ sudo vim /etc/httpd/conf.d/wordpress.conf

Enganxem:

<VirtualHost *:80>
    
    ServerName localhost
    
    ServerAdmin root@localhost
    
    DocumentRoot /var/www/html/wordpress


    <Directory "/var/www/html/wordpress">

        Options Indexes FollowSymLinks
        
        AllowOverride all
        
        Require all granted

    </Directory>

    ErrorLog /var/log/httpd/wordpress_error.log
    
    CustomLog /var/log/httpd/wordpress_access.log common
    
</VirtualHost>

$ sudo systemctl restart httpd

$ sudo systemctl status httpd


allow HTTP and HTTPS:

$ sudo firewall-cmd --permanent --zone=public --add-service=http 

$ sudo firewall-cmd --permanent --zone=public --add-service=https

$ sudo firewall-cmd --reload


Set Up WordPress from a Browser
*******************************
http://192.168.1.xxx


# Plugins
https://youtu.be/Z7QfH-s-15s
