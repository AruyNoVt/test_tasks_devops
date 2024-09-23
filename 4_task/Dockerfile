FROM php:8.3-fpm as base

RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
 && docker-php-ext-install pdo pdo_mysql \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html

COPY . .

USER root
ARG STATIC_VERSION \
STATIC_STAGING
ENV STATIC_STAGING=${STATIC_STAGING} \
STATIC_VERSION=${STATIC_VERSION}

RUN curl -L -O "https://artifactory.com/artifactory/project_repo/${STATIC_STAGING}/project-${STATIC_VERSION}.tar.gz" && \
    tar -xzf project-${STATIC_VERSION}.tar.gz ./build/static --strip-components 2 && \
    mv static ./webroot && \
    rm project-${STATIC_VERSION}.tar.gz && \
    composer install --no-dev --optimize-autoloader && \
    php ./bin/console cache:warmup && \
    chown -R www-data:www-data /var/www/html/webroot/static /var/www/html/var

USER www-data
