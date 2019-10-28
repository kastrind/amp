ARG PHP_VERSION=""
FROM php:${PHP_VERSION:+${PHP_VERSION}-}fpm-alpine

RUN apk update; \
    apk upgrade;


RUN docker-php-source extract && \
    
    apk add --no-cache \
    --virtual .build-dependencies	\
        $PHPIZE_DEPS				\
        zlib-dev					\
		git							\
        autoconf					\
        g++							\
        libtool						\
        make						\
        pcre-dev					&& \
	
	apk add --no-cache              \
	tini                            \
	libintl                         \
	icu                             \
	icu-dev                         \
	libxml2-dev                     \
	postgresql-dev                  \
	freetype-dev                    \
	libjpeg-turbo-dev               \
	libpng-dev                      \
	gmp                             \
	gmp-dev                         \
	libmemcached-dev                \
	imagemagick-dev                 \
	libzip-dev                      \
	libssh2                         \
	libssh2-dev                     \
	libxslt-dev                  && \
	
	git clone https://github.com/php-memcached-dev/php-memcached.git /usr/src/php/ext/memcached/    && \
			
			docker-php-ext-configure memcached      &&  \
			
			docker-php-ext-configure gd                 \
				--with-freetype-dir=/usr/include/       \
				--with-jpeg-dir=/usr/include/       &&  \
			
			
	docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" \
		intl                                                \
		bcmath                                              \
		xsl                                                 \
		zip                                                 \
		soap                                                \
		mysqli                                              \
		pdo                                                 \
		pdo_mysql                                           \
		pdo_pgsql                                           \
		gmp                                                 \
		iconv                                               \
		gd                                                  \
		memcached                                       &&  \
		pecl install apcu \
			&& docker-php-ext-enable apcu
			
			
# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Instal Codeception
RUN composer require codeception/codeception --dev
