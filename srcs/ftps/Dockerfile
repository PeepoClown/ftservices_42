FROM alpine:latest
LABEL maintainer="wupdegra@student.21-school.ru"

# update & install packages
RUN apk update --no-cache && \
	apk upgrade --no-cache && \
	apk add vsftpd \
			openssl --no-cache

# generate ssl cert
RUN openssl req -x509 -nodes -days 90 -newkey rsa:2048 \
	-subj '/C=RU/ST=Moscow/L=Moscow/O=ecole42/OU=21School/CN=wupdegra' \
	-keyout /etc/ssl/certs/vsftpd.key \
	-out /etc/ssl/certs/vsftpd.crt && \
	chmod -R 755 /etc/ssl/certs/vsftpd*

# configure vsftpd
RUN adduser -D ftp_user && echo 'ftp_user:passwd' | chpasswd && \
	mkdir -p /var/ftp_user && \
	chown -R ftp_user:ftp_user /var/ftp_user
COPY srcs/vsftpd.conf /etc/vsftpd/vsftpd.conf
COPY srcs/welcome.mp4 /var/ftp_user/welcome.mp4

EXPOSE 21 21000

# run container
COPY srcs/setup.sh /var/setup.sh
RUN chmod 755 /var/setup.sh

ENTRYPOINT [ "/var/setup.sh" ]