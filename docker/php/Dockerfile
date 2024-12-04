FROM php:8.2-fpm

# Set working directory
WORKDIR /var/www

# Remove default /var/www/html directory if needed (optional)
RUN rm -rf /var/www/html

# Install necessary dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg-dev \
    libfreetype-dev \
    locales \
    zip \
    jpegoptim \
    optipng \
    pngquant \
    gifsicle \
    vim \
    unzip \
    git \
    apt-utils \
    libonig-dev \
    libzip-dev && \
    rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install zip pdo_mysql mbstring exif

# Configure and install GD extension with support for JPEG and FreeType
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd

# Install Redis PHP extension via PECL
RUN mkdir -p /usr/src/php/ext/redis && \
    curl -fsSL https://pecl.php.net/get/redis -o /usr/src/php/ext/redis.tar.gz && \
    tar xfz /usr/src/php/ext/redis.tar.gz -C /usr/src/php/ext/redis --strip 1 && \
    docker-php-ext-install redis

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Create a group and user for Laravel
RUN groupadd -g 1000 laravel && \
    useradd -u 1000 -ms /bin/bash -g laravel user

# Copy the application files and set permissions
COPY --chown=user:laravel . /var/www

# Ensure correct permissions
RUN chown -R user:laravel /var/www

# Switch to the non-root user
USER user

# Expose port 9000
EXPOSE 9000

# Start PHP-FPM
CMD ["php-fpm"]
