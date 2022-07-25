# docker-compose-lamp
A simple set of dockers for running a Drupal or WordPress in Linux, Apache, MariaDB, and PHP (LAMP)
- Apache 2.4 with SSL
- ModSecurity 2.9
- PHP-FPM 7.3
- MariaDB
- phpMyAdmin
- Composer
- NodeJS, NPM and Yarn

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
-v ~/web-hosting/certbot/docker-volumes/etc/letsencrypt:/etc/letsencrypt \
-v ~/web-hosting/certbot/docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
-v ~/web-hosting/certbot/letsencrypt-site:/var/www/html \
-v "~/web-hosting/certbot/var/log/letsencrypt:/var/log/letsencrypt" \
certbot/certbot \
certonly --webroot \
--register-unsafely-without-email --agree-tos \
--webroot-path=/data/letsencrypt \
--staging \
-d website1.com -d www.website1.com
```
Note. Once this runs correctly, take out the --staging, and put in your real email.
* Stop the temporary container with: `docker-compose -f docker-compose-basic.yml stop`

#### Start Containers with Certificates
* On your docker host: Setup local paths where you will host each websites files from. For example: ~/domain.com/public_html 
* Edit the apache/vhosts.apache.conf and docker-compose.yml to include the websites you are hosting
* On your docker host: Do a `sudo chown -R 33` on the website files folders so that Apache can read/write from within the container
* `docker-compose up` (This Builds and starts the Containers. To run in the background add "-d")

Note: To enable ModSecurity edit apache/modsecurity.conf and change "SecRuleEngine DetectionOnly" to "SecRuleEngine On"

## Updating Certs with Certbot
This needs work, but it was the best way I could get this to work for now.
1.  Adjust your firewall rule to redirect incoming port 80 traffic to another port, for example 8111
2.  Run something like this to re-issue the certs on the system:
```
sudo docker run -it --rm --name certbot \
-v "/data-ssd/kubestorage/var/www/certbot/docker-volumes/etc/letsencrypt:/etc/letsencrypt" \
-v "/data-ssd/kubestorage/var/www/certbot/docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt" \
-p 8111:80 \
certbot/certbot certonly \
-d mydomain.com -d www.mydomain.com
```
This seems to work as long as there aren't numerous symlinks:
```
sudo docker run -it --rm --name certbot \
-v "/data-ssd/kubestorage/var/www/certbot/docker-volumes/etc/letsencrypt:/etc/letsencrypt" \
-p 8111:80 \
certbot/certbot renew
```
3. When certbot runs, you can press "1" to start a standalone instance to renew the domain.
4. Revert the web server port back to what it was.

## Using Composer, NodeJS, NPM and Yarn
- Exec into the php container and run these tools.

## Example import existing Drupal install:

-  Copy your Drupal site files into the folder you defined in apache/vhosts.apache.conf
-  In phpMyAdmin create a new user, and database which matches the credentials in your Drupal sites/default/settings.php file.
-  Import a mysqldump of your database using phpMyAdmin

### Accessing the web server

Navigate to [https://localhost](https://localhost)

### Accessing phpMyadmin

Navigate [http://localhost:8181](http://localhost:8181)

