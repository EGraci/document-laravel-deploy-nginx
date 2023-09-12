### Configurate PHP and Composer

```
    sudo apt get-update
    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get install php[version] php[version]-cli php[version]-fpm
    sudo apt install php8.2-{mysql,cli,gd,zip,curl,mbstring,fileinfo,pdo-mysql,xml,json,bcmath }
    php -m
    nano /etc/php/[version]/cli/php.ini
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
    root /var/www/[pathfile];
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


### Configurate HTTPS

```
    sudo apt install certbot python3-certbot-nginx`
    sudo certbot --nginx
```

### Allow Route Laravel

### Edit sites-available/[siteName]

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

### Restart Server

```
    systemctl restart nginx
    systemctl restart php[version]-fpm
```
