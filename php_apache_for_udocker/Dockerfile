FROM php:apache

# Fix Apache logs for udocker / proot
RUN rm -f /var/log/apache2/*.log && \
    touch /var/log/apache2/access.log \
          /var/log/apache2/error.log \
          /var/log/apache2/other_vhosts_access.log && \
    chmod 666 /var/log/apache2/*.log

# Suppress ServerName warning
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
