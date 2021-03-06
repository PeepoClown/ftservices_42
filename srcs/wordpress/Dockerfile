FROM alpine:latest
LABEL maintainer="wupdegra@student.21-school.ru"

# update & install packages
RUN apk update --no-cache && \
	apk upgrade --no-cache && \
	apk add	nginx \
			wget \
			php7 php7-fpm php7-mysqli \
			php7-common php7-cli php7-mbstring \
			php7-json php7-curl php7-gd \
			php7-xml php7-xmlrpc php7-zip \
			supervisor

# configure nginx
RUN adduser -D -g 'www' www && \
	mkdir -p /run/nginx /run/php && \
	rm -rf /etc/nginx/conf.d/default.conf

# copy sources
COPY srcs/nginx.conf /etc/nginx/conf.d/nginx.conf
COPY srcs/supervisord.conf /etc/supervisord.conf

# install wordpress
COPY srcs/wordpress.tar.gz ./wordpress.tar.gz
RUN	tar -xzvf ./wordpress.tar.gz && \
	mv wordpress /www/ && \
	chown -R www:www /www && \
	chmod -R 755 /www && \
	rm -rf ./wordpress.tar.gz

EXPOSE 5050

# run container
COPY srcs/setup.sh /var/setup.sh
RUN chmod 755 /var/setup.sh

ENTRYPOINT [ "/var/setup.sh" ]
