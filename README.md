Install wordpress on CENTOS 7

Step One — Create a MySQL Database and User for WordPress

```
mysql -u root -p

```

You will be prompted for the password that you set for the root account when you installed MySQL. Once that password is submitted, you will be given a MySQL command prompt.

First, we’ll create a new database that WordPress can control. You can call this whatever you would like, but I will be calling it wordpress for this example.

```
CREATE DATABASE wordpress;

```

Note: Every MySQL statement or command must end in a semi-colon (;), so check to make sure that this is present if you are running into any issues.

Next, we are going to create a new MySQL user account that we will use exclusively to operate on WordPress’s new database. Creating one-function databases and accounts is a good idea, as it allows for better control of permissions and other security needs.

I am going to call the new account wordpressuser and will assign it a password of password. You should definitely use a different username and password, as these examples are not very secure.

```
CREATE USER wordpressuser@localhost IDENTIFIED BY 'password';

```

At this point, you have a database and user account that are each specifically made for WordPress. However, the user has no access to the database. We need to link the two components together by granting our user access to the database.

```
GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost IDENTIFIED BY 'password';

```
Now that the user has access to the database, we need to flush the privileges so that MySQL knows about the recent privilege changes that we’ve made:

```
FLUSH PRIVILEGES;

```

Step Two — Install apache

```
yum install httpd -y

```
Start httpd
```
systemctl start httpd

```
enable httpd

```
systemctl enable httpd

```
to check status

```
systemctl status httpd

```

## Step 3 install PHP 7.3

## Step 3.1: Add PHP 7.3 Remi repository

PHP 7.3 is available for CentOS 7 and Fedora distributions from the Remi repository. Add it to your system by running

```
yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm

yum -y install epel-release yum-utils

```

## Step 3.2: Disable repo for PHP 5.4

By default, the enabled repository is for PHP 5.4. Disable this repo and enable on for PHP 7.3

```
yum-config-manager --disable remi-php54

yum-config-manager --enable remi-php73

```

## Step 3.3: Install PHP 7.3 on CentOS 7 / Fedora

Once the repo has been enabled, install php 7.3 on CentOS 7 or Fedora using the command

```
sudo yum -y install php php-cli php-fpm php-mysqlnd php-zip php-devel php-gd php-mcrypt php-mbstring php-curl php-xml php-pear php-bcmath php-json

```

Check version installed

```
php -v

```

Step 4 insatll WORDPRESS

```
#!/bin/bash
amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
yum install httpd -y
cd /var/www/html
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
cp -r wordpress/* /var/www/html/
rm -rf wordpress
rm -rf latest.tar.gz
chown -R apache /var/www
chgrp -R apache /var/www
chmod 2775 /var/www
find /var/www -type d -exec sudo chmod 2775 {} \;
find /var/www -type f -exec sudo chmod 0664 {} \;

service httpd start
systemctl restart httpd
```

```
cp wp-config-sample.php wp-config.php

```

```
vim wp-config.php

```

The only modifications we need to make to this file are to the parameters that hold our database information. We will need to find the section titled MySQL settings and change the DB_NAME, DB_USER, and DB_PASSWORD variables in order for WordPress to correctly connect and authenticate to the database that we created.

Fill in the values of these parameters with the information for the database that you created. It should look like this:

![alt text](https://github.com/anjanpaul/Wordpress-doc/blob/main/output/Screenshot%202022-02-07%20at%204.09.25%20PM.png)


```
service httpd start
systemctl restart httpd

```
