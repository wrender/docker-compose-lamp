# docker-compose-lamp
A simple set of dockers for running a local Drupal or WordPress LAMP
- Apache 2.4
- PHP 7.0
- MariaDB 10.1
- phpMyAdmin

## General Usage Information

By default this mounts the /var/www/html directory to ~/public_html on your Linux OS.  From here you can install drupal using Drush or use any website files. It is intended to keep all of the web application files outside of the containers so you can easily work on the files. The database should go in the mariadb container included with this.

## Setup

If your running Ubuntu or Fedora as your local development environment you should create a user and group and folder to match the permission of the ~/public_html folder with the container's Apache user/group. The Apache runs as UID and GUID 33 (www-data).

`sudo useradd -u 33 www-data; sudo groupadd www-data -g 33; sudo usermod -g 33 www-data` (If you already have a group with GID 33, that is ok this command will fail)

`sudo usermod -aG 33 your_username;`

*now logout/login of linux as it is needed for the new permissions to take effect.

### To run the containers

`mkdir ~/public_html; sudo chown -R 33:33 ~/public_html; sudo chmod g+s ~/public_html`  (Sets the correct permissions on the folder)

`docker-compose up -d` (Starts the Containers)



## Example Fresh Drupal install:

`cd ~/public_html; drush dl drupal-7; mv ~/public_html/drupal*/* ~/public_html; mv ~/public_html/drupal*/.htaccess ~/public_html/drupal*/.gitignore ~/public_html; rm -rf drupal-*`

`sudo chown -R 33:33 ~/public_html`

`drush site-install standard --db-url='mysql://root:docker@172.17.0.2/drupaldb' --site-name=Example`

### Notes about Drush
* You need to have Drush, php-mysqlnd/php-mysql, php-gd, php, php-common, and MariaDB-client packages installed locally for Drush to be able to connect to the mariadb container.

* You need to change the IP address above 172.17.0.2 to the IP of your database container. You can do a `docker inspect your_database_container` to find the IP.

## Example import existing Drupal install:

-  Copy your Drupal site files to ~/public_html
-  In phpMyAdmin create a new user, and database which matches the credentials in your Drupal sites/default/settings.php file.
-  Import a dump of your database using phpMyAdmin

### Accessing the web server

Navigate to [http://localhost:8180](http://localhost:8180)

### Accessing phpMyadmin

Navigate [http://localhost:8181](http://localhost:8181)

