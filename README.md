# docker-compose-lamp
A simple set of dockers for running a local Drupal or WordPress LAMP
- Apache
- PHP 7.0
- MariaDB 10.1 
- phpMyAdmin

## To run it
'docker-compose up -d'

## Usage Information

By default it mounts to ~/public_html.  From here you can install drupal using Drush.

Example drush install:

`drush site-install standard --db-url='mysql://root:docker@172.17.0.2/drupaldb' --site-name=Example`

### Accessing the web server

Navigate to http://localhost:8180

### Accessing phpMyadmin

Navigate to http://localhost:8181  

username: root  password: docker