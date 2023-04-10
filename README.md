## Magento Test

### Try to build app locally

* First of all clone code from github, Run this command:
  
``` 
git clone https://github.com/roham96/magento2.git 
```

* Go to peoject``` /root``` directory. In this project root dir is magento2 
```
cd magento2
```

* Next step you need to install composer on you web application:
  
``` 
composer install
```

* After install all dependensies, Run commands:
```
php bin/magento setup:upgrade
php bin/magento setup:static-content:deploy
php bin/magento cache:clean
```

### Extra Help..


## #1
```
  sudo apt-get update
  sudo apt-get -y install git wget curl nano unzip sudo nano net-tools openssh-server
  sudo apt-get -y install nginx
  sudo service nginx start
  nginx -v
  sudo ufw app list
  sudo ufw allow 'Nginx HTTP'
  curl -4 icanhazip.com
```

  ## #2
```
sudo apt-get install software-properties-common
  sudo add-apt-repository ppa:ondrej/php
  sudo apt-get update
  sudo apt-cache search php7.2
  sudo apt-get install php7.2 libapache2-mod-php7.2 php7.2-common php7.2-gd php7.2-mysql php7.2-curl php7.2-intl php7.2-xsl php7.2-mbstring php7.2-zip php7.2-bcmath php7.2-soap php-xdebug php-imagick 
  sudo apt-get -y install php7.2-fpm
  
  
 sudo nano /etc/php/7.2/fpm/php.ini
 sudo nano /etc/php/7.3/fpm/php.ini
 sudo nano /etc/php/7.4/fpm/php.ini
 
  press ctrl + w for searching
  press ctrl + shift+v for past &  Enter Search
   
  memory_limit = 4G
  max_execution_time = 3600
  max_input_time = 1800
  upload_max_filesize = 10M
  zlib.output_compression = On
  max_input_vars = 81000

  press ctrl + O for saving or Ctrl+x
  
  sudo service php7.2-fpm start
```

  ## #3
```
 curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
  composer --version
```

  ## #4
```
 Install MySQL using the apt command below:
      sudo apt install mysql-server mysql-client -y
      enter the password for the root user: giaphugroup
      service mysql start
      mysql_secure_installation
      SHOW VARIABLES LIKE "%version%"; 
      sudo mysql -u root -p

 After the MySQL installation is complete, start the MySQL service and enable it to launch everytime at system boot.
      systemctl start mysql
      systemctl enable mysql
```

  ## #5
```
install PHPMyAdmin using the apt command below.
 sudo apt install phpmyadmin -y
 During the installation, it will ask you about the web server configuration for phpmyadmin.
 Choose none option and move the cursor to: 'OK'.
 For the phpmyadmin database configuration: choose 'Yes'
 And type new 'STRONG' phpmyadmin admin such as 'Hakaselabs001@#'.

 Enter a password: "PhpMyAdmin@123"
 Repeat the "PhpMyAdmin@123" password.
```

  ## #6
```
  sudo  nano /etc/nginx/sites-available/default
     Paste the following Nginx configuration for phpmyadmin inside the 'server {...}' bracket.

 location /phpmyadmin {
     root /usr/share/;
     index index.php;
     try_files $uri $uri/ =404;

 location ~ ^/phpmyadmin/(doc|sql|setup)/ {
     deny all;
     }

 location ~ /phpmyadmin/(.+\.php)$ {
     fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
     fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
     include fastcgi_params;
     include snippets/fastcgi-php.conf;
     }
 }
 Save and exit.
 
  Test the nginx configuration and restart the nginx service.
 nginx -t
 systemctl reload nginx
 And we've added the Nginx configuration for phpmyadmin.
 Login to the MySQL shell.

 mysql -u root -p
 Now create a new user using the MySQL queries below.

 create user phpmyadmin@'localhost' identified by 'PhpMyAdmin@123';
 grant all privileges on *.* to phpmyadmin@'localhost' identified by 'PhpMyAdmin@123';
 flush privileges;
 exit;

 Test Login PhpMyAdmin
 On the web browser, type the following phpmyadmin URL (replace the IP with your server IP).

 http://127.0.0.1/phpmyadmin/
```

  ## #7
```
- Create the Magento authentication keys: https://marketplace.magento.com/
- Create the new database named magento2_3_0: http://localhost:9000
- cd /var/www/html
-composer create-project --repository=https://repo.magento.com/ magento/project-community-edition <install-directory-name>
[ composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.3.x magento2.3.x]
- cd magento2.3.2
find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
chown -R :www-data .
chmod u+x bin/magento
php bin/magento setup:di:compile
- Create a new virtual host for accessing to the Magento2.3.x site
nano /etc/nginx/sites-available/magento2.3.x
upstream fastcgi_backend {
    server unix:/run/php/php7.2-fpm.sock;
}

server {
    listen 86;
    server_name localhost;
    set $MAGE_ROOT /var/www/html/magento2.3.2;
    include /var/www/html/magento2.3.2/nginx.conf.sample;
}
ln -s /etc/nginx/sites-available/magento2.3.2 /etc/nginx/sites-enabled
- Restart Nginx:
nginx -t
service nginx restart
netstat -plnt
86 is of the magento 2.3.2 site.

- Install Magento 2.3.2: http://localhost:86
- cd /var/www/html/magento2.3.2
php bin/magento setup:static-content:deploy -f
```

  ## #8
```
    --Multiple Subdomain of Nginx Server
      
      
      - cd /var/www/html
      composer create-project --repository=https://repo.magento.com/ magento/project-community-edition magento.lan
       cd magento.lan
      find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
      find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
      chown -R :www-data .
      chmod u+x bin/magento
      php bin/magento setup:di:compile
      
      
     /etc/nginx/sites-available/default. Let's take a look at it:
     *** Remove below code from all site except default otherwise show error**
     $ cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak
     $ sudo nano /etc/nginx/sites-available/default
     
     upstream fastcgi_backend {
           server unix:/run/php/php7.2-fpm.sock;
      }

      # Default server configuration
    
      server {
           listen 80 default_server;
           listen [::]:80 default_server;

           # SSL configuration
           #
           # listen 443 ssl default_server;
           # listen [::]:443 ssl default_server;
           #
           # Note: You should disable gzip for SSL traffic.
           # See: https://bugs.debian.org/773332
           #
           # Read up on ssl_ciphers to ensure a secure configuration.
           # See: https://bugs.debian.org/765782
           #
           # Self signed certs generated by the ssl-cert package
           # Don't use them in a production server!
           #
           # include snippets/snakeoil.conf;

           root /var/www/html;

           # Add index.php to the list if you are using PHP
           index index.php index.html index.htm index.nginx-debian.html;

           server_name _;

           location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
           }

           # pass PHP scripts to FastCGI server
           #
           #location ~ \.php$ {
           #	include snippets/fastcgi-php.conf;
           #
           #	# With php-fpm (or other unix sockets):
           #	fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
           #	# With php-cgi (or other tcp sockets):
           #	fastcgi_pass 127.0.0.1:9000;
           #}

           # deny access to .htaccess files, if Apache's document root
           # concurs with nginx's one
           #
           #location ~ /\.ht {
           #	deny all;
           #}
           location /phpmyadmin {
            root /usr/share/;
           index index.php;
            try_files $uri $uri/ =404;

           location ~ ^/phpmyadmin/(doc|sql|setup)/ {
           deny all;
                }

           location ~ /phpmyadmin/(.+\.php)$ {
           fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
           fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
           include fastcgi_params;
           include snippets/fastcgi-php.conf;
                }	
           }


      }
      
      $sudo nano /etc/nginx/sites-available/magento.lan 
      server {
          listen 80;
          server_name www.magento.lan;
          set $MAGE_ROOT /var/www/html/magento.lan;
          include /var/www/html/magento.lan/nginx.conf.sample;
           }
           
       $ sudo nginx -t  
       $ sudo nano /etc/hosts
       
       127.0.0.1	localhost
       Host File Content Like That:
       
           127.0.1.1	matin-U18 
           127.0.0.1	www.magento.lan 
       
      $ sudo ln -s /etc/nginx/sites-available/magento.lan /etc/nginx/sites-enabled
      
      [ sudo unlink /etc/nginx/sites-enabled/default ]
      
      # Resart ALL
      sudo systemctl restart nginx
      sudo service php7.2-fpm restart
      sudo service mysql restart
      sudo systemctl restart mysql.service 
      
      http://www.magento.lan/setup
      
      [In your browser Complete Install]
```
  ## #9
```
cd /var/www/html/magento.lan
php bin/magento sampledata:deploy
 Authentication required (repo.magento.com): if ask cmd
 Username: xxxxx
 Pasward: xxx

php bin/magento setup:upgrade
```
  ## #10
  For remove
```
php php bin/magento sampledata:remove
```

## #11

Ex.

```
 Public Key: 2c534578b8507d00e7f927af2591756d
 Private Key: 8819e8bf8336c7ada213ee2df18f5c15
```