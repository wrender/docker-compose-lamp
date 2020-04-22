FROM httpd:2.4
RUN apt update -y; \
    apt upgrade -y;
# Copy apache vhost file to proxy php requests to php-fpm container
COPY vhosts.apache.conf /usr/local/apache2/conf/vhosts.apache.conf
RUN echo "Include /usr/local/apache2/conf/vhosts.apache.conf" \
    >> /usr/local/apache2/conf/httpd.conf

RUN sed -i \
        -e 's/^#\(LoadModule .*mod_ssl.so\)/\1/' \
        -e 's/^#\(LoadModule .*mod_socache_shmcb.so\)/\1/' \
        conf/httpd.conf

# Enable ModSecurity
RUN apt install -y git libapache2-mod-security2 \
    && cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf \
    && echo 'LoadModule unique_id_module /usr/local/apache2/modules/mod_unique_id.so' >> /usr/local/apache2/conf/httpd.conf \
    && mkdir /var/log/apache2/ \
    && echo 'IncludeOptional /etc/apache2/mods-available/security2.load' >> /usr/local/apache2/conf/httpd.conf \
    && echo 'IncludeOptional /etc/modsecurity/modsecurity.conf' >> /usr/local/apache2/conf/httpd.conf

ADD modsecurity.conf /etc/modsecurity/modsecurity.conf

RUN mkdir -p modsecurity.d/owasp-modsecurity-crs

RUN git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git modsecurity.d/owasp-modsecurity-crs

ADD crs-setup.conf modsecurity.d/owasp-modsecurity-crs/crs-setup.conf

RUN echo 'IncludeOptional /usr/local/apache2/modsecurity.d/owasp-modsecurity-crs/crs-setup.conf' >> /usr/local/apache2/conf/httpd.conf \
    && echo 'IncludeOptional /usr/local/apache2/modsecurity.d/owasp-modsecurity-crs/rules/*.conf' >> /usr/local/apache2/conf/httpd.conf
