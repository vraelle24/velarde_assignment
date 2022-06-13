#!/bin/sh

# command to install wget
yum install -y wget

# command to install time and date
yum install -y ntpdate


# command will update date and time
ntpdate ntp.ubuntu.com


# command to install httpd
yum install -y httpd


# command will start apache on your VM
systemctl start httpd.service


# command will tell firewalld to allow traffic to our web server
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --permanent --add-port=443/tcp


# command will reload the firewall to apply the changes
firewall-cmd --reload

# commands will Install and update PHP Apache and its Modules that is necessary for installing LAMP and WordPress
yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm
yum install -y yum-utils
yum-config-manager --enable remi-php56
yum install -y php php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo


# command will enable apache to start on boot
systemctl enable httpd.service


# command will restart apache so that it recognizes the new module
systemctl restart httpd.service

# command will install MySQL (MariaDB)
yum install -y mariadb-server mariadb

# command will start MariaDB
systemctl start mariadb

# command will run mysql_secure_installation script
# script will remove some dangerous defaults and lock down access to our database system a little bit.
echo -e "\nY\nroot\nroot\nY\nY\nY\nY\n " | mysql_secure_installation

# command will enable MariaDB to start on boot
systemctl enable mariadb.service

#  to run lamp stack + wordpress using simple security script
echo "Running simple security script"
mysql_secure_installation <<EOF
# automatic type yes after creating mariadb user and pass
echo "Enter Yes"
Y
# for password
echo "Enter my password"
chocolate
# for confirm password
echo "Confirm my password"
chocolate
# yes or no
echo "Enter yes"
Y
# yes or no
echo "Enter yes"
Y
# yes or no
echo "Enter yes"
Y
# yes or no
echo "Enter yes"
Y
#EOF
echo "EOF operator"
EOF

# command to enable mariadb password and dbname
echo "mariadb enable"
#variables
passw=chocolate
dbname=wordpress

# command will create a database named "wordpress"(commands to input)
mysql -u root -e "CREATE DATABASE wordpress;"

# command will create a new MySQL user account and grants privileges to database "wordpress"(commands to input)
mysql -u root -e "CREATE USER wp_user@localhost IDENTIFIED BY 'cottoncandy';"
mysql -u root -e "GRANT ALL PRIVILEGES ON wordpress.* TO wp_user@localhost IDENTIFIED BY 'cottoncandy';"
mysql -u root -e "FLUSH PRIVILEGES;"

# command will bring you to root directory
cd ~

# command will download and install WordPress
wget http://wordpress.org/latest.tar.gz

# command will extract the archived file
tar xzvf latest.tar.gz

# command will install rsync
yum install -y rsync

# command will transfer the file to /var/www/html directory
rsync -avP ~/wordpress/ /var/www/html/

# command will add uploads directory
mkdir /var/www/html/wp-content/uploads

# command will assign correct ownership and permission to the wordpress files
chown -R apache:apache /var/www/html/*


# This command will bring you to apache root directory
cd /var/www/html/

# command  will create wp-config.php file by extracting wp-config-sample.php
#and configure the database information
cat wp-config-sample.php | sed -e 's/database_name_here/wordpress/g' | sed -e 's/username_here/wp_user/g' | sed -e 's/password_here/wp_thanthan/g' > wp-config.php

# add all changes to the git server
git add -A

# commit changes added to the git and add a message
git commit -m "added lampstackwordpress.sh"

# upload the script to github
git push origin main
