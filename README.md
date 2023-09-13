### Configurate PHP

```
    apt get-update
    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get install php[version] php[version]-cli php[version]-fpm
    sudo apt install php[version]-{mysql,cli,gd,zip,curl,mbstring,fileinfo,pdo-mysql,xml,bcmath }
    php -m
```
### Instalasion SSL for SSO
Download <a href='https://curl.se/docs/caextract.html'>CA Certificat</a> 
<br>
Select Newest and copy link addres
<br>

```
    wget -P /etc/php/[version] -O cacert.pem [pasteAddresLink] 
    nano /etc/php/[version]/cli/php.ini
```
### Edit PHP.ini
find and replace code below
```
    curl.cainfo=/etc/php/[version]/cacert.pem
    openssl.cafile=/etc/php/[version]/cacert.pem
    extension=curl
    extension=fileinfo
    extension=mbstring
    extension=mysqli
    extension=pdo-mysql
    extension=openssl
```

### Configurate Composer

```
    sudo apt-get install composer
    curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

### Default Configurate Nginx

```
    apt install nginx
    rm /etc/nginx/sites-enabled/default
    nano /etc/nginx/sites-available/[siteName]
```

```
server {
    listen 80;
    server_name mydomain.com www.mydomain.com;
    root /var/www/[pathFolder];
    index index.html index.htm index.php;
    location / {
        try_files $uri $uri/ =404;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php[version]-fpm.sock;
    }
}
```

### Allow Route Laravel

```
    location / {
                try_files $uri $uri/ /index.php$is_args$args;
    }
```

```
    location ~ \.php$ {
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                fastcgi_split_path_info ^(.+?\.php)(/.+)$;
                include fastcgi_params;
                fastcgi_pass unix:/run/php/php[version]-fpm.sock;
        }
```

### Finish Nginx

```
    sudo ln -s /etc/nginx/sites-available/[siteName] /etc/nginx/sites-enabled/
    sudo ufw allow http
    sudo ufw allow ssh
    sudo ufw allow https
    sudo ufw enable
    sudo ufw status
    sudo chmod -R 755 /var/www/[pathFolder]
    sudo chown -R $USER:$USER /var/www/[pathFolder]
```

### Restart Server

```
    systemctl restart nginx
    systemctl restart php[version]-fpm
```

### Configurate HTTPS

```
    sudo apt install certbot python3-certbot-nginx`
    sudo certbot --nginx
```
