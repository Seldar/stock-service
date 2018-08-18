FROM php:7.2.1-fpm

LABEL maintainer "Rafael Dias <n3xu5.0@gmail.com>"

RUN docker-php-ext-install pdo pdo_mysql

RUN apt-get update

RUN apt-get --assume-yes install nginx

RUN apt-get --assume-yes install mysql-server