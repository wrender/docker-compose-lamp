# docker-compose-lamp
A simple set of dockers for running a Drupal or WordPress in Linux, Apache, MariaDB, and PHP (LAMP)
- Apache 2.4 with SSL
- ModSecurity 2.9
- PHP-FPM 7.3
- MariaDB
- phpMyAdmin

## General Usage Information

First create a temporary container and generate your website SSL certs with LetsEncrypt Certbot. Second startup the main docker-compose.yml. It is intended to keep all of the web application files outside of the containers so you can easily work on the files. The databases go in the mariadb container included with this.

## Setup

#### Creating LetsEncrypt Certificates
* Setup domains you want to host in docker-compose-basic.yml and in 000-default-basic.conf
* Create a local directory to hold your certs. For example: `~/web-hosting/certbot/`
* Modify docker-compose-basic.yml to map your local certs directory to this volume to /var/www/html
* Run `docker-compose -f docker-compose-basic.yml up`. This starts up a basic webserver to generate the website certificates
* Run a certbot/certbot with these commands to generate certs for each domain:
```
sudo docker run -it --rm \
-v /web-hosting/certbot/docker-volumes/etc/letsencrypt:/etc/letsencrypt \
-v /web-hosting/certbot/docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
-v /web-hosting/letsencrypt-site:/var/www/html \
-v "/docker-volumes/var/log/letsencrypt:/var/log/letsencrypt" \
certbot/certbot \
certonly --webroot \
--register-unsafely-without-email --agree-tos \
--webroot-path=/data/letsencrypt \
--staging \
-d website1.com -d www.website1.com
```
* Stop the temporary container with: `docker-compose -f docker-compose-basic.yml stop`

#### Start Containers with Certificates
* On your docker host: Setup local paths where you will host each websites files from. For example: ~/domain.com/public_html 
* Edit the apache/vhosts.apache.conf and docker-compose.yml to include the websites you are hosting
* On your docker host: Do a `sudo chown -R 33` on the website files folders so that Apache can read/write from within the container
* `docker-compose up` (This Builds and starts the Containers. To run in the background add "-d")

Note: To enable ModSecurity edit apache/modsecurity.conf and change "SecRuleEngine DetectionOnly" to "SecRuleEngine On"

## Updating Certs with Certbot
- Still need to do this.

## Example import existing Drupal install:

-  Copy your Drupal site files into the folder you defined in apache/vhosts.apache.conf
-  In phpMyAdmin create a new user, and database which matches the credentials in your Drupal sites/default/settings.php file.
-  Import a mysqldump of your database using phpMyAdmin

### Accessing the web server

Navigate to [https://localhost](https://localhost)

### Accessing phpMyadmin

Navigate [http://localhost:8181](http://localhost:8181)

