# Base image
FROM yiisoftware/yii2-php:8.4-apache

# Set working directory
WORKDIR /app

# Copy custom Apache config
COPY laravel-apache-https.conf /etc/apache2/sites-available/000-default.conf

# Enable required Apache modules
RUN a2enmod rewrite headers

# Ensure Laravel directories are writable
RUN mkdir -p /app/storage /app/bootstrap/cache \
    && chown -R www-data:www-data /app/storage /app/bootstrap/cache

# Expose port 80
EXPOSE 80

# Start Apache
CMD ["apache2-foreground"]
