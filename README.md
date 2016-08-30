# test1

#install LEMP stack drush and git
apt-get update -y
apt-get -y install nginx
service nginx start
apt-get install -y php5-fpm php5-cli php5-gd php5-mcrypt php5-mysql php5-curl
#Using default password for root as password1    
apt install -y mysql-server
mysql_install_db
service mysql restart
mysql_secure_installation
apt-get install -y php5
apt-get install -y drush
apt-get install -y git
#use drush to download drupal-7 and copy all files including hidden to the default nginx folder
drush dl drupal-7
cp -R  drupal-7.50/* /usr/share/nginx/html/
cp drupal-7.50/.* /usr/share/nginx/html/
sed -i 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' /etc/php5/fpm/php.ini
grep cgi.fix_pathinfo=0 /etc/php5/fpm/php.ini
service php5-fpm restart
#copy paste this to check php info of the server.
#<?php
# phpinfo();
#?>
vi /usr/share/nginx/html/info.php
#edit default nginx config
vi /etc/nginx/sites-available/default
#check config
nginx -t
service nginx restart
service php5-fpm restart
#make files dir in drupal
mkdir /usr/share/nginx/html/sites/default/files
#copy default drupal settings
cp /usr/share/nginx/html/sites/default/default.settings.php sites/default/settings.php
chown -R www-data:www-data /usr/share/nginx/html/sites
chown www-data:www-data /usr/share/nginx/html/sites/default
chmod 755 /usr/share/nginx/html/sites/default
chmod a+w /usr/share/nginx/html/sites/default/settings.php
chmod a+w /usr/share/nginx/html/sites/default/files
mysql -u root -p
mysql> CREATE DATABASE drupaldb;
mysql> GRANT ALL ON drupaldb.* to 'drupal'@'localhost' identified by 'password1';
mysql>    CREATE DATABASE civicrm;
mysql> GRANT ALL ON civicrm.* to 'civicrm'@'localhost' identified by 'password1';
mysql> flush privileges;
service mysql restart
mkdir /usr/share/nginx/private
chown www-data:www-data /usr/share/nginx/private

chmod 700 /usr/share/nginx/html/sites/default/files/civicrm/upload

service nginx restart
service php5-fpm restart
service mysql restart
