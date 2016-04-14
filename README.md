# docker-compose-lamp
A simple set of dockers for running a local Drupal or WordPress LAMP
- Apache 2.4
- PHP 7.0
- MariaDB 10.1
- phpMyAdmin

## To run it
`docker-compose up -d`

## Usage Information

By default it mounts to ~/public_html.  From here you can install drupal using Drush or use any website files.

If your running Ubuntu or Fedora as your local development environment you should match the permission of the ~/public_html folder with the container's Apache user/group.

Run `sudo useradd www-data; sudo chown -R www-data:www-data ~/public_html; sudo usermod -a -G www-data $USER`

Example drush install:

`drush site-install standard --db-url='mysql://root:docker@172.17.0.2/drupaldb' --site-name=Example`

### Accessing the web server

Navigate to [http://localhost:8180](http://localhost:8180)

### Accessing phpMyadmin

Navigate [http://localhost:8181](http://localhost:8181)

username: root
password: docker
