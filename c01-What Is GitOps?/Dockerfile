FROM ghcr.io/wilsonify/library/ubuntu:24.04

RUN apt-get update &&  \
    apt-get upgrade -y &&  \
    apt-get install -y apache2 apache2-utils &&  \
    apt-get clean

EXPOSE 80
CMD ["apache2ctl", "-D", "FOREGROUND"]