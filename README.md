# docker-compose-lamp
A simple set of dockers for running a local Drupal or WordPress LAMP
- Apache
- PHP 7.0
- MariaDB 10.1 

## To run it
'docker-compose up -d'

## Information

By default it mounts to ~/drupal-html.  From here you can install drupal using Drush.

Example drush install:

'drush site-install standard --db-url='mysql://root:docker@172.17.0.2/drupaldb' --site-name=Example'

