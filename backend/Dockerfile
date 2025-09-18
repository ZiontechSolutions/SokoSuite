FROM php:8.3-fpm

# Install system deps & PHP extensions
RUN apt-get update && apt-get install -y \
    git \
    curl \
    ca-certificates \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    libpq-dev \
    libzip-dev \
    gnupg2 \
    && docker-php-ext-install pdo pdo_pgsql mbstring exif pcntl bcmath gd zip

# Install node (for asset builds)
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - \
    && apt-get install -y nodejs

# Install composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

# Copy composer files first (speeds up builds when dependencies don't change)
COPY composer.json composer.lock ./

RUN composer install --no-dev --no-interaction --optimize-autoloader || true

# Copy rest of the application
COPY . .

# Ensure vendor exists if it wasn't created earlier
RUN composer install --no-dev --no-interaction --optimize-autoloader

# Expose PHP-FPM port (not strictly necessary for this compose)
EXPOSE 9000

CMD ["php-fpm"]
