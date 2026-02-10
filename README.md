# NCAE-CSUSB-LSXXC
2/4/2026




Nginx notes
sudo apt update
sudo apt install -y nginx
sudo systemctl enable --now nginx
sudo systemctl status nginx


----Testing config for errors------
sudo ngniux -t 

 Path                          Purpose               
 -----------------------------  --------------------- 
 `/etc/nginx/`                  Main config directory 
 `/etc/nginx/nginx.conf`        Main config file      
 `/etc/nginx/sites-available/`  Site configs (Ubuntu) 
 `/etc/nginx/sites-enabled/`    Enabled sites        
 `/var/www/html/`               Common web root       
 `/var/log/nginx/`             Access & error logs   


-----hiding Ngnix version-----

sudo nano /etc/nginx/nginx.conf


Add inside http {}

server_tokens off;

-----disable directory listing-----

inside a server or location block

autoindex off;

----Restrict access to hidden/sensititve files----

location ~ /\.(?!well-known) {
    deny all;
}



----Useing strong file permissions------


sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html






-----Rate limiting--------

Add to nginx.conf

limit_req_zone $binary_remote_addr zone=one:10m rate=10r/s;



using inside a server block 


location / {
    limit_req zone=one burst=20 nodelay;
}




log viewing
/var/log/nginx/access.log
/var/log/nginx/error.log

sudo tail -f /var/log/nginx/error.log



Adding HTTPS 

Install certbot and issue the certificate:

sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d moutainpeak.com -d www.moutainpeak.com


Test auto-renew:

sudo certbot renew --dry-run












Apache Notes
.htaccess-config file

sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
sudo systemctl reload apache2
sudo systemctl status apache2
sudo apachectl configtest-Run before restarting 


 Path                             Purpose                  
 -------------------------------  ------------------------ 
 `/etc/apache2/`                  Main config directory    
 `/etc/apache2/apache2.conf`      Main config file         
 `/etc/apache2/sites-available/`  Virtual host configs     
 `/etc/apache2/sites-enabled/`    Enabled sites (symlinks) 
 `/var/www/html/`                 Default website root     
 `/var/log/apache2/`              Access & error logs      



sudo nano /etc/apache2/conf-enabled/security.conf  ---

ServerTokens Prod
ServerSignature Off

<Directory /var/www/html>
    Options -Indexes
</Directory>

<FilesMatch "^\.">
    Require all denied
</FilesMatch>

--------Uses Stronger permissions----------
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
--------Enabling HTTP------------------
Sudo a2enmod ssl

----Enabling mod_secuirty-----------
sudo apt install libapach2-mod-security2


----Brute force attack stop-------
sudo apt install fail 2ban


-----Logs------
/var/log/apache2/access.log
/var/log/apache2/error.log

sudo tail -f /var/log/apache2/error.log
