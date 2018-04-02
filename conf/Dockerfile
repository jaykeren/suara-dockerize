FROM php:5.6-fpm

RUN apt-get update
RUN apt-get install -y autoconf pkg-config libssl-dev vim

RUN pecl install mongodb-1.2.2
RUN docker-php-ext-install pdo_mysql bcmath
RUN echo "extension=mongodb.so" >> /usr/local/etc/php/conf.d/mongodb.ini

RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# Install Laravel dependencies
RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev
		
RUN docker-php-ext-install iconv mcrypt mbstring \
    && docker-php-ext-install zip \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd
	
RUN apt-get install -y supervisor && \
	sed -i 's/^\(\[supervisord\]\)$/\1\nnodaemon=true/' /etc/supervisor/supervisord.conf
	
COPY php.ini /usr/local/etc/php/conf.d/40-custom.ini

# Install Phalcon
RUN apt-get install -y git
RUN git clone https://github.com/phalcon/cphalcon \
	&& cd cphalcon \
	&& git checkout 3.2.x \
	&& cd build \
	&& ./install \
	&& echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini
