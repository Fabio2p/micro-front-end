FROM node:18.15.0-bullseye-slim

WORKDIR /home/www/setup

COPY . .

RUN apt update -y \
    && npm i -g @angular/cli@15.2.4 \
    && ng analytics off \
    && npm i --legacy-peer-deps \
    && ng build \
    && apt install wget -y \
    && apt install apache2 libapache2-mod-security2 -y \
    && a2enmod security2 \
    && wget https://dl-ssl.google.com/dl/linux/direct/mod-pagespeed-stable_current_amd64.deb \
    && dpkg -i mod-pagespeed-stable_current_amd64.deb && apt-get -f install \
    && a2enmod rewrite \
    && a2enmod ssl \
    && a2enmod headers \
    && a2enmod expires

COPY ./config_apache/*.conf /etc/apache2

ADD ./config_apache/sites/*.conf /etc/apache2/sites-available

RUN a2ensite p2ca.com.br.conf \
    && a2dissite 000-default.conf \
    && a2dissite default-ssl.conf

EXPOSE 4200

CMD ["apachectl", "-D", "FOREGROUND"]
