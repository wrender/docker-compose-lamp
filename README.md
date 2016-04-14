# docker-compose-lamp
A simple set of dockers for running a local Drupal or WordPress LAMP
- Apache 2.4
- PHP 7.0
- MariaDB 10.1
- phpMyAdmin

## Usage Information

By default this mounts to ~/public_html.  From here you can install drupal using Drush or use any website files.

If your running Ubuntu or Fedora as your local development environment you should create a user and group and folder to match the permission of the ~/public_html folder with the container's Apache user/group. The Apache runs as UID and GUID 33 (www-data).

`sudo groupadd www-data -g 33; sudo useradd -u 33  --no-create-home --system --no-user-group www-data; sudo usermod -g www-data www-data` (If you already have a group with GID 33, that is ok this command will fail)

`sudo usermod -aG 33 your_username;`

*now logout/login of linux as it is needed for the new permissions to take effect.

## To run the containers

`mkdir ~/public_html; sudo chown -R 33:33 ~/public_html; sudo chmod g+s ~/public_html`  (Sets the correct permissions on the folder)

`docker-compose up -d` (Starts the Containers)



Example drush install:

`cd ~/public_html; drush dl drupal-7; mv ~/public_html/drupal*/* ~/public_html; mv ~/public_html/drupal*/.htaccess ~/public_html/drupal*/.gitignore ~/public_html; rm -rf drupal-*`

`drush site-install standard --db-url='mysql://root:docker@172.17.0.2/drupaldb' --site-name=Example`

### Notes about Drush
* You need to have Drush installed locally, and as well have PHP, and mariadb_client packages installed for it to be able to connect to the container.
* You need to change the IP address above 172.17.0.2 to the IP of your database container. You can do a `docker inspect your_database_container` to find the IP.

### Accessing the web server

Navigate to [http://localhost:8180](http://localhost:8180)

### Accessing phpMyadmin

Navigate [http://localhost:8181](http://localhost:8181)

