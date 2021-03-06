FROM        jamesnesbitt/wunder-base
MAINTAINER  james.nesbitt@wunderkraut.com

### NGINX --------------------------------------------------------------------

# Install the nginx signing key
ADD root/nginx_signing.key /root/nginx_signing.key
RUN /usr/bin/rpm --verbose --import http://nginx.org/keys/nginx_signing.key

# Install nginx (make it run as the nginx user)
ADD etc/yum.repos.d/nginx.repo /etc/yum.repos.d/nginx.repo
RUN /usr/bin/yum install --assumeyes --verbose nginx

ADD etc/nginx/nginx.conf /etc/nginx/nginx.conf
ADD etc/nginx/conf.d /etc/nginx/conf.d

# give ownership of logs to the nginx user
RUN /usr/bin/mkdir -p /app/log/nginx && \
    /usr/bin/chown -R nginx:nginx /app/log/nginx && \
    /usr/bin/chmod -R ug+rwX /app/log/nginx

# make sure that the nginx user, created, has access to app user files
RUN /usr/bin/gpasswd -a nginx app

# As a command run the nginx server in foreground mode.
# IF nginx.conf does not have it, add this: "-g 'daemon off'"
ENTRYPOINT ["/usr/sbin/nginx"]
CMD ["-c/etc/nginx/nginx.conf"]
EXPOSE 80 443

### /NGINX -------------------------------------------------------------------
