FROM nextcloud:apache

# Update and install SMB client and dependencies
RUN apt-get update && apt-get install -y \
    smbclient \
    libsmbclient-dev \
    && pecl install smbclient \
    && docker-php-ext-enable smbclient \
    && rm -rf /var/lib/apt/lists/*
