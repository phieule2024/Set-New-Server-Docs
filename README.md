# Apache Installation and Configuration Guide

## Installation

1. **Update Package Index**: Ensure your package index is up-to-date.
    ```bash
    sudo apt update
    ```

2. **Install Apache**: Use apt to install Apache.
    ```bash
    sudo apt install apache2
    ```

## Start and Enable Apache

1. **Start Apache**: If Apache doesn't start automatically after installation, start it manually.
    ```bash
    sudo systemctl start apache2
    ```

2. **Enable Apache**: Enable Apache to start automatically on boot.
    ```bash
    sudo systemctl enable apache2
    ```

## Firewall Configuration

1. **Adjust Firewall**: If you're using UFW, allow Apache traffic.
    ```bash
    sudo ufw allow 'Apache'
    ```

## Basic Configuration

1. **Main Configuration File**: Modify Apache's main configuration file if needed.
    - Location: `/etc/apache2/apache2.conf`

## Virtual Hosts

1. **Create Virtual Hosts**: Set up virtual hosts for hosting multiple websites.
    - Configuration files located in `/etc/apache2/sites-available/`
    - Enable virtual hosts using `a2ensite` command.

## Restart Apache

1. **Restart Apache**: After making configuration changes, restart Apache.
    ```bash
    sudo systemctl restart apache2
    ```

## Testing

1. **Test Apache**: Ensure Apache is running correctly by accessing your server's IP address or domain name in a web browser.


## Upload foder 

# Virtual Host Configuration for vnpee.com

## HTTP (Port 80)

```
<VirtualHost *:80>
    ServerName vnpee.com
    DocumentRoot /var/www/html/vnpee/public
    
    <Directory "/var/www/html/vnpee/public">
        AllowOverride all
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/vnpee_error.log
    CustomLog ${APACHE_LOG_DIR}/vnpee_access.log combined
</VirtualHost>
```
## HTTPS (Port 443)
```
<VirtualHost *:443>
    DocumentRoot /var/www/html/vnpee/public
    ServerName vnpee.com
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/certificate.txt
    SSLCertificateKeyFile /etc/ssl/private/private_key.txt
    # If you have a certificate chain file, concatenate it with your certificate file
    # SSLCertificateChainFile /etc/ssl/rootca.txt
</VirtualHost>

```

# Refresh again

```
bash
    sudo systemctl restart apache2
```
# Configurations for Creating MariaDB User, Firewall, PHP Version, and SSL Module

## Create MariaDB User

1. Log in to MariaDB:
    ```bash
    mysql -u root -p
    ```

2. Execute the following commands to create a new user and grant privileges:
    ```sql
    CREATE USER 'new_user'@'%' IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON *.* TO 'new_user'@'%';
    FLUSH PRIVILEGES;
    EXIT;
    ```

3. Verify that the user has been created:
    ```sql
    SELECT User FROM mysql.user;
    ```

## Firewall Configuration

1. Allow ports 3306, 80, and 443:
    ```bash
    sudo ufw allow 3306/tcp
    sudo ufw allow 80/tcp
    sudo ufw allow 443/tcp
    sudo ufw reload
    ```

2. Check the status of the firewall:
    ```bash
    sudo ufw status
    sudo ufw status | grep 3306
    sudo ss -tuln | grep 3306
    sudo ufw enable
    ```

## MariaDB Configuration

1. Edit the MariaDB configuration file:
    ```bash
    sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
    ```

2. Change the bind-address to allow connections from any IP address:
    ```
    bind-address = 0.0.0.0
    ```

3. Restart MariaDB:
    ```bash
    sudo systemctl restart mysql
    ```

## PHP Version and Extensions

1. Install PHP 7.4 and required extensions:
    ```bash
    sudo apt install php7.4
    sudo apt install php7.4-mysql php7.4-xml php7.4-mbstring php7.4-json php7.4-curl php7.4-zip
    ```

2. Switch PHP version if needed:
    ```bash
    sudo update-alternatives --config php
    ```

3. Verify PHP version:
    ```bash
    php -v
    ```

4. Restart Apache:
    ```bash
    sudo systemctl restart apache2
    ```

## Apache Configuration

1. Check Apache configuration for errors:
    ```bash
    sudo apache2ctl configtest
    ```

2. Enable SSL module:
    ```bash
    sudo a2enmod ssl
    ```

3. Restart Apache:
    ```bash
    sudo systemctl restart apache2
    ```

Feel free to reach out if you have any questions or need further assistance.
