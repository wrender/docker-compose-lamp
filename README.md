# docker-compose-lamp
A simple set of dockers for running a local Drupal or WordPress LAMP
- Apache 2.4
- PHP 7.0
- MariaDB 10.1
- phpMyAdmin

## Usage Information

By default this mounts to ~/public_html.  From here you can install drupal using Drush or use any website files.

If your running Ubuntu or Fedora as your local development environment you should create a user and group and folder to match the permission of the ~/public_html folder with the container's Apache user/group.

`sudo useradd www-data; sudo mkdir ~/public_html; sudo chown -R www-data:www-data ~/public_html; sudo chmod 774 ~/public_html; sudo usermod -a -G www-data $USER`

*now logout/login of linux as it is needed for the new permissions to take effect.

## To run the containers
`docker-compose up -d`

Example drush install:

`drush site-install standard --db-url='mysql://root:docker@172.17.0.2/drupaldb' --site-name=Example`

Once Drupal is up and running you will need to run `sudo chmod -R 774 ~/public_html/sites/default/files` to ensure the files directory is writable.

### Accessing the web server

Navigate to [http://localhost:8180](http://localhost:8180)

### Accessing phpMyadmin

Navigate [http://localhost:8181](http://localhost:8181)

username: root
password: docker
