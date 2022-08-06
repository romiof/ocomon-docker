FROM php:8.1-apache-bullseye


ARG DEBIAN_FRONTEND=noninteractive


# Update geral do linux
RUN apt-get -y update --fix-missing && \
    apt-get upgrade -y && \
    apt-get --no-install-recommends install -y apt-utils && \
    rm -rf /var/lib/apt/lists/*


# Instalar libs necessarias no Linux
RUN apt-get -y update && \
    apt-get -y --no-install-recommends install openssl \
    libcurl4-openssl-dev \
    libssl-dev \
    libc-client-dev \
    libkrb5-dev \
    zlib1g-dev \
    libpng-dev \
    libjpeg-dev \
    libldb-dev \
    libldap2-dev \
    libfreetype6-dev \
    && rm -r /var/lib/apt/lists/*


# Modulos e extensoes do PHP8
RUN docker-php-ext-install pdo_mysql && \
    docker-php-ext-configure imap --with-kerberos --with-imap-ssl && docker-php-ext-install imap && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && docker-php-ext-install gd && \
    docker-php-ext-install ldap


# Enable apache modules
RUN a2enmod rewrite headers

# Criação de pastas para cada site ter sua sessoes do PHP e configuração das permissões nas pastas para o usuário do apache ( www-data )
RUN cd /tmp && \
    mkdir ocomon sapp && \
    chown www-data ocomon/ sapp/ && \
    chgrp www-data ocomon/ sapp/

# Cleanup
RUN rm -rf /usr/src/*
