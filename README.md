# permisionPHP
1. User Group
Pastikan user/group www-data tersebut sudah ada


 
$ id www-data
uid=33(www-data) gid=33(www-data) groups=33(www-data)
ganti www-data dengan user yang ingin di cek.

caranya tambah useradd -g www-data foo

2. User nginx
Buka file /etc/nginx/nginx.conf, ubah bagian

user  nginx;
menjadi

user  www-data;
lalu restart nginx

service nginx restart
3. User php-fpm
PHP 7
buka file /etc/php/7.0/fpm/pool.d/www.conf, sesuaikan dengan nilai dibawah ini

user = www-data
group = www-data
listen = /run/php/php7.0-fpm.sock
listen.owner = www-data
listen.group = www-data
listen.mode = 0660

 
PHP 5
buka file /etc/php5/fpm/pool.d/www.conf, sesuaikan dengan nilai dibawah ini

user = www-data
group = www-data
listen = /var/run/php5-fpm.sock
listen.owner = www-data
listen.group = www-data
listen.mode = 0660
lalu restart php-fpm

# php 7.0
service php7.0-fpm restart
# php5
service php5-fpm restart
4. Server Block alias Virtualhost
Sesuaikan alamat socket php-fpm, contoh di server block nginx
PHP 7

  location ~ \.php$ {
    try_files $uri =404;
    fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
  }
PHP 5

  location ~ \.php$ {
    try_files $uri =404;
    fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
  }
