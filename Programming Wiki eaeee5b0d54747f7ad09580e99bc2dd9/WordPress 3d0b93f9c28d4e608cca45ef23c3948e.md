# WordPress

# Install SSL Certificate

## How to remove the Certbot

1. Remove Certbot

    ```
    sudo certbot delete

    ```

2. Remove Certbot's Apache package

    ```
    sudo apt purge python-certbot-apache

    ```

3. Disable the SSL config file created by certbot

    ```
    sudo a2dissite 000-default-le-ssl.conf

    ```

4. Remove certbot files manually

    ```
    sudo rm -rf /etc/letsencrypt/
    sudo rm -rf /var/lib/letsencrypt/
    sudo rm -rf /var/log/letsencrypt/

    ```

5. Make sure the repo is updated and autoremove

    ```
    sudo apt update
    sudo apt upgrade
    sudo apt autoremoves
    ```

6. **Cautious:** Additionally you can also reinstall apache2 if needed for fresh config files

    ```
    sudo apt purge apache2
    sudo service apache2 restart

    ```

    If `mods-available` folder also completely get purged during apache2 removal process then PHP will not be executed and code will be displayed on the browser. Please follow the below process in such case,

    ```
    sudo apt purge libapache2-mod-php7.2
    sudo apt install libapache2-mod-php7.2
    sudo a2enmod php7.2
    sudo apachectl configtest
    sudo service apache2 restart
    ```