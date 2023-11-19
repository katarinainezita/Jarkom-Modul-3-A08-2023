# Praktikum Modul 2 Jaringan Komputer

## Anggota Kelompok A08

| No.  | Nama Anggota       | NRP          |
|------|--------------------|--------------|
| 1    |Mohammad Zhafran Dzaky           | 5025211142   |
| 2    | Katarina Inezita Prambudi         | 5025211148   |


## Topologi Soal
<img width="364" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/2b956bbd-e2e8-4d8d-8ce0-37e22be2328f">

## Penjelasan Soal
### Soal 0
Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker PHP dan granz.channel.yyy.com untuk worker Laravel mengarah pada worker yang memiliki IP 10.x.1.

## Config
* Aura
```
auto eth0
iface eth0 inet dhcp
	up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.3.0.0/16

auto eth1
iface eth1 inet static
	address 10.3.1.8
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.3.2.8
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.3.3.8
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.3.4.8
	netmask 255.255.255.0
```

* Himmel

```
auto eth0
iface eth0 inet static
	address 10.3.1.1
	netmask 255.255.255.0
	gateway 10.3.1.8
```

* Heiter
```
auto eth0
iface eth0 inet static
	address 10.3.1.2
	netmask 255.255.255.0
	gateway 10.3.1.8
```

* Denken
```
auto eth0
iface eth0 inet static
	address 10.3.2.1
	netmask 255.255.255.0
	gateway 10.3.2.8
```

* Eisen
```
auto eth0
iface eth0 inet static
	address 10.3.2.2
	netmask 255.255.255.0
	gateway 10.3.2.8
```

#### PHP Worker

* Frieren
```
auto eth0
iface eth0 inet static
	address 10.3.4.1
	netmask 255.255.255.0
	gateway 10.3.4.8
```

* Flamme
```
auto eth0
iface eth0 inet static
	address 10.3.4.2
	netmask 255.255.255.0
	gateway 10.3.4.8
```

* Fern
```
auto eth0
iface eth0 inet static
	address 10.3.4.3
	netmask 255.255.255.0
	gateway 10.3.4.8
```

* Laravel
```
auto eth0
iface eth0 inet static
	address 10.3.3.1
	netmask 255.255.255.0
	gateway 10.3.3.8
```

* Linie

```
auto eth0
iface eth0 inet static
	address 10.3.3.2
	netmask 255.255.255.0
	gateway 10.3.3.8
```

* Lugner
```
auto eth0
iface eth0 inet static
	address 10.3.3.3
	netmask 255.255.255.0
	gateway 10.3.3.8
```

#### Client
* Stark, Sein, Revolte, Richter
```
auto eth0
iface eth0 inet dhcp
```

### Soal 1
Lakukan konfigurasi sesuai dengan peta yang sudah diberikan. 

Kemudian, karena masih banyak spell yang harus dikumpulkan, bantulah para petualang untuk memenuhi kriteria berikut :

Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.

#### Script 
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt update
apt install bind9 -y
echo 'zone "riegel.canyon.a08.com" {
    type master;
    file "/etc/bind/jarkom/riegel.canyon.a08.com";
};

zone "granz.channel.a08.com" {
    type master;
    file "/etc/bind/jarkom/granz.channel.a08.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.a08.com. root.riegel.canyon.a08.com. (
                        2023111401    ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL
;
@               IN      NS      riegel.canyon.a08.com.
@               IN      A       10.3.4.1 ; IP Frieren laravel' > /etc/bind/jarkom/riegel.canyon.a08.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.a08.com. root.granz.channel.a08.com. (
                        2023111401    ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL
;
@               IN      NS      granz.channel.a08.com.
@               IN      A       10.3.3.1 ; IP Lawine php' > /etc/bind/jarkom/granz.channel.a08.com

service bind9 restart
```

### Soal 2
Client yang melalui Switch3 mendapatkan range IP dari 10.3.16 - 10.3.32 dan 10.3.64 - 10.3.80

### Soal 3
Client yang melalui Switch4 mendapatkan range IP dari 10.3.4.12 - 10.3.4.20 dan 10.3.4.160 - 10.3.4.168

### Soal 4 
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

### Soal 5
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit 

Berjalannya waktu, petualang diminta untuk melakukan deployment.

### Soal 6
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.

Setup load balancer eisen : 
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install nginx -y
echo 'nameserver 10.3.1.2
nameserver 192.168.122.1' > /etc/resolv.conf

touch /etc/nginx/sites-available/lb-php

echo 'upstream worker {
        server 10.3.3.1;
        server 10.3.3.2;
        server 10.3.3.3;
}

server {
        listen 80;
        server_name granz.channel.a08.com;

        location / {
                proxy_pass http://worker;
        }
}' > /etc/nginx/sites-available/lb-php

ln -s /etc/nginx/sites-available/lb-php /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t
```

Setup worker (lawine, linie, lugner) :

```
echo 'nameserver 10.3.1.2' > /etc/resolv.conf
apt-get update && apt install wget unzip nginx php7.3 php7.3-fpm -y

mkdir /var/www/granz.channel.a08
wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O /var/www/granz.channel.a08/gc.zip
unzip /var/www/granz.channel.a08/gc.zip -d /var/www/granz.channel.a08

mv /var/www/granz.channel.a08/modul-3/* /var/www/granz.channel.a08
rm -rf /var/www/granz.channel.a08/gc.zip

touch /etc/nginx/sites-available/granz.channel.a08

echo ' server {

        listen 80;

        root /var/www/granz.channel.a08;

        index index.php index.html index.htm;
        server_name _;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
        }

 location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
 }' > /etc/nginx/sites-available/granz.channel.a08

ln -s /etc/nginx/sites-available/granz.channel.a08 /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service php7.3-fpm start
service php7.3-fpm restart
service nginx restart
nginx -t
```

Setup Client (bebas)

```
echo 'nameserver 10.3.1.2' > /etc/resolv.conf
apt-get update && apt-get install lynx htop apache2-utils
```


### Soal 7
Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
a. Lawine, 4GB, 2vCPU, dan 80 GB SSD.
b. Linie, 2GB, 2vCPU, dan 50 GB SSD.
c. Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

Tes Request
```
ab -n 200 -c 10 http://10.3.2.2/
ab -n 200 -c 10 http://granz.channel.a08.com/
```

Cek Pembagian Load
```
echo '' > /var/log/nginx/jarkom_access.log
cat /var/log/nginx/jarkom_access.log| grep "GET" | wc -l
```

<img width="225" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/6d8a8715-ae6f-4b05-bf52-8016ad40e80e">

* Bukti di Worker

<img width="502" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/f78439e8-b9c0-4691-8690-4734ad88aec5">

### Soal 8
Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
a. Nama Algoritma Load Balancer
b. Report hasil testing pada Apache Benchmark
c. Grafik request per second untuk masing masing algoritma. 
d. Analisis

* Tes Least Connection di lb eisen
```
echo 'upstream worker {
        least_conn;
        server 10.3.3.1;
        server 10.3.3.2;
        server 10.3.3.3;
}

server {
        listen 80;
        server_name granz.channel.a08.com;

        location / {
                proxy_pass http://worker;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }
}' > /etc/nginx/sites-available/lb-php

unlink /etc/nginx/sites-enabled/lb-php
ln -s /etc/nginx/sites-available/lb-php /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t
```

<img width="297" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/a0b0d2a9-8c25-40b0-baa7-d8e91f15dc03">
<img width="297" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/07258598-d37e-44f8-82b4-4e2373e22689">
<img width="296" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/106b26c8-7593-41e7-8113-0971df26347a">
<img width="297" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/5e8f641f-f4cf-4cdd-8259-fb003ec69a4a">

* Tes Least Connection Client
```
ab -n 200 -c 10 http://10.3.2.2/
```

* Cek Pembagian Load di Worker
```
cat /var/log/nginx/jarkom_access.log| grep "GET" | wc -l
```

<img width="297" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/db0c3155-805a-4779-932a-941c6caee2ec">
<img width="554" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/80226a7b-c62c-4b3c-8343-113458e9bcb4">
<img width="554" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/178f5031-1ba1-4c52-b01c-240b512ce419">
<img width="560" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/9f6e833b-4636-4bac-8592-02ad2ceec1f9">

* Tes IP Hash
echo 'upstream worker {
        ip_hash;
        server 10.3.3.1;
        server 10.3.3.2;
        server 10.3.3.3;
}

server {
        listen 80;
        server_name granz.channel.a08.com;

        location / {
                proxy_pass http://worker;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }
}' > /etc/nginx/sites-available/lb-php

unlink /etc/nginx/sites-enabled/lb-php
ln -s /etc/nginx/sites-available/lb-php /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t

* Tes IP hash
```
ab -n 200 -c 10 http://10.3.2.2/
```

* Cek Pembagian Load
```
cat /var/log/nginx/jarkom_access.log| grep "GET" | wc -l
```

<img width="297" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/13be4e2a-889f-4411-a525-75b2d69c9043">
<img width="556" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/0c3a48d7-5d3f-4279-bcdc-1d7da3b2bbde">
<img width="561" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/a08f847d-afc1-4b84-aae8-2d03dd5c7f93">
<img width="558" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/24c6b15d-a437-4192-9ae8-6b1558e124f2">

* Tes Generic Hash
```
echo 'upstream worker {
        hash $request_uri consistent;
        server 10.3.3.1;
        server 10.3.3.2;
        server 10.3.3.3;
}

server {
        listen 80;
        server_name granz.channel.a08.com;

        location / {
                proxy_pass http://worker;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }
}' > /etc/nginx/sites-available/lb-php

unlink /etc/nginx/sites-enabled/lb-php
ln -s /etc/nginx/sites-available/lb-php /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t

```

* Tes Generic Hash di Client
```
ab -n 200 -c 10 http://10.3.2.2/
```

* Cek Pembagian Load
```
cat /var/log/nginx/jarkom_access.log| grep "GET" | wc -l
```

<img width="299" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/4f73a220-bec6-4f1a-823e-e983416245da">
<img width="560" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/9193b77b-cfe1-4a84-8719-40e50f020b33">
<img width="556" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/f665382b-972b-4155-9125-52bbc0b0914f">
<img width="558" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/49e8a09b-1401-4ae9-b0be-5e506b1293d6">
<img width="560" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/e04ef965-305b-4d28-963f-c371298c556a">
<img width="555" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/82b98998-85af-49d7-b29b-2977836a1d74">
<img width="559" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/188c6c81-fe7d-4a78-9b5e-0c0627a2532e">



### Soal 9
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.

* 3 Worker
<img width="336" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/16f60f78-e51d-4ae8-957a-928ff8cea79e">

<img width="447" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/c3b230f9-44ba-4662-b06e-4cc4a60f3c44">

<img width="448" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/f0389f9d-099a-4267-8d13-1208205a5621">

<img width="447" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/e5bd972f-cf0e-4748-bc26-81c90687d1a8">

* 2 Worker 
<img width="352" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/ea98f4e6-2171-436c-ac5d-e22d4440b088">

<img width="446" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/1c380c16-0ee5-4198-8c45-bf45a8055bec">

<img width="448" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/faa854eb-f722-459d-a5a7-3ecde578d4b5">

* 1 Worker
<img width="332" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/859c0b62-dd8e-411f-98fd-1a8e70b3d3bc">

<img width="338" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/692efad2-cedd-423d-b408-b5b04eb271e8">


### Soal 10
Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

* Setup lb Eisen
```
echo 'upstream worker {
        server 10.3.3.1;
        server 10.3.3.2;
        server 10.3.3.3;
}

server {
        listen 80;
        server_name granz.channel.a08.com;

        location / {
                proxy_pass http://worker;
                auth_basic "Restricted Area";
                auth_basic_user_file /etc/nginx/rahasiakita/.htpasswd;
        }
        location ~ /\.ht {
                deny all;
        }
}' > /etc/nginx/sites-available/lb-php

mkdir /etc/nginx/rahasiakita/

htpasswd -c -b /etc/nginx/rahasiakita/.htpasswd netics ajka08

service nginx restart
nginx -t
```

### Soal 11
Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id.

* Setup lb eisen
```
echo 'upstream worker {
        server 10.3.3.1;
        server 10.3.3.2;
        server 10.3.3.3;
}

server {
        listen 80;
        server_name granz.channel.a08.com;

        location / {
                proxy_pass http://worker;
        }
        location ~* /its {
                proxy_pass https://www.its.ac.id;
        }
}' > /etc/nginx/sites-available/lb-php

service nginx restart
nginx -t
```

### Soal 12
Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [10.3.3.69], [10.3.3.70], [10.3.4.167], dan [103.3.4.168].

Karena para petualang kehabisan uang, mereka kembali bekerja untuk mengatur granz.channel.yyy.com.

* Setup lb Eisen
```
echo 'upstream worker {
        server 10.3.3.1;
        server 10.3.3.2;
        server 10.3.3.3;
}

server {
        listen 80;
        server_name granz.channel.a08.com;

        location / {
                proxy_pass http://worker;

                allow 10.3.3.69;
                allow 10.3.3.70;
                allow 10.3.4.167;
                allow 10.3.4.168;
                deny all;
        }
}' > /etc/nginx/sites-available/lb-php

service nginx restart
nginx -t
```

### Soal 13 & Soal 14
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. 
Frieren, Flamme, dan Fern memiliki Granz Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer
Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.

* Setup Database Denken

```
echo 'nameserver 10.3.1.2' > /etc/resolv.conf
apt-get update && apt-get install mariadb-server -y

service mysql start

echo '
[client-server]

[mysqld]
skip-networking=0
skip-bind-address
' >> /etc/mysql/my.cnf


echo '
# These groups are read by MariaDB server.
# Use it for options that only the server (but not clients) should see

# this is read by the standalone daemon and embedded servers
[server]

# this is only for the mysqld standalone daemon
[mysqld]

#
# * Basic Settings
#
user                    = mysql
pid-file                = /run/mysqld/mysqld.pid
socket                  = /run/mysqld/mysqld.sock
#port                   = 3306
basedir                 = /usr
datadir                 = /var/lib/mysql
tmpdir                  = /tmp
lc-messages-dir         = /usr/share/mysql


bind-address            = 0.0.0.0
query_cache_size        = 16M
log_error               = /var/log/mysql/error.log
expire_logs_days        = 10
character-set-server    = utf8mb4
collation-server        = utf8mb4_general_ci

[embedded]

[mariadb]

[mariadb-10.3]' > /etc/mysql/mariadb.conf.d/50-server.cnf

mysql << EOF

CREATE USER 'kelompoka08'@'10.3.4.%' IDENTIFIED BY 'ajka08';
CREATE USER 'kelompoka08'@'localhost' IDENTIFIED BY 'ajk08';
CREATE DATABASE dbkelompoka08;
GRANT ALL PRIVILEGES ON *.* TO 'kelompoka08'@'10.3.4.%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompoka08'@'localhost';
FLUSH PRIVILEGES;

EOF
```

* Jika sudah pernah membuat database dan isinya, reset script menggunakan script di bawah ini
```
mysql << EOF

DROP USER 'kelompoka08'@'10.3.4.%';
DROP USER 'kelompoka08'@'localhost';
DROP DATABASE dbkelompoka08;

EOF
```

* Setup Worker Laravel (Frieren, Flamme, Fern)

```
echo 'nameserver 10.3.1.2' > /etc/resolv.conf
rm /etc/apt/sources.list.d/php.list

apt-get update && apt-get install mariadb-client -y
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'

apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y

php --version

apt-get install nginx -y
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer

composer -V

apt-get install git -y
cd /var/www/
git clone https://github.com/martuafernando/laravel-praktikum-jarkom.git
composer install

cd /var/www/laravel-praktikum-jarkom
cp .env.example .env
echo 'APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=10.3.2.1
DB_PORT=3306
DB_DATABASE=dbkelompoka08
DB_USERNAME=kelompoka08
DB_PASSWORD=ajka08

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"' > .env

composer update

php artisan migrate:fresh
php artisan db:seed --class=AiringsTableSeeder
php artisan key:generate
php artisan jwt:secret
```

* Setup script untuk connect ke database :
```
mariadb --host=10.3.2.1 --port=3306 --user=kelompoka08 --password=ajka08
```

### Soal 15
POST /auth/register

* Test menggunakan AB :
```
ab -n 100 -c 10 -T 'application/json' -p register_data.json -g register_result.data http://10.3.4.1:8001/api/auth/register
```

<img width="337" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/6ab0e334-f5ae-4170-bc99-285f0c74a4fc">

* Test menggunakan curl
```
curl -X POST -H "Content-Type: application/json" -d '{"username": "zen", "password": "bismillah"}' http://10.3.4.1:8001/api/auth/register
```

<img width="335" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/ba9f430c-3679-4963-8724-d926e046e682">


### Soal 16 
POST /auth/login

* Test Menggunakan AB
```
ab -n 100 -c 10 -T 'application/json' -p login_data.json -g login_result.data http://10.3.4.1:8001/api/auth/login
```

<img width="451" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/a74b145f-8a6c-4925-a4b3-cc13e8fd344a">

* Test Menggunakan Curl
```
curl -X POST -H "Content-Type: application/json" -d '{"username": "zen", "password": "bismillah"}' http://10.3.4.1:8001/api/auth/login
```

<img width="404" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/29b7a133-da2a-489e-a4d3-87c45405df1a">

### Soal 17
GET /me

* Test Menggunakan AB :
```
token=$(cat temp_token.txt); ab -n 100 -c 10 -H "Authorization: Bearer $token" http://10.3.4.1:8001/api/me
```

<img width="560" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/9d7b6226-c1ea-4a2c-a58b-8ebe53e83ecc">

* Cek di database (Frieren)

<img width="300" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/f63d954d-46fa-4078-a89a-3cdde93fa3bf">


### Soal 18
Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Granz Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.

### Soal 19
Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.

* Setup lb Eisen untuk Deploy Riegel menggunakan worker laravel :
```
touch /etc/nginx/sites-available/lb-laravel

echo 'upstream laravel {
        server 10.3.4.1:8001;
        server 10.3.4.2:8002;
        server 10.3.4.3:8003;
}

server {
        listen 80;
        server_name riegel.canyon.a08.com;

        location / {
                proxy_bind 10.3.2.2;
                proxy_pass http://laravel;
        }
}' > /etc/nginx/sites-available/lb-laravel

ln -s /etc/nginx/sites-available/lb-laravel /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t
```

* Hasil Work 1
<img width="299" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/085d8347-98ae-47dd-a382-62300bba4e1e">


* Hasil Work 2
<img width="360" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/d2e322d9-a263-491b-9fdd-b92791079f8a">


* Hasil Work 3
<img width="361" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/b5155353-e0dc-4a46-b541-46b87c2b8c1e">

* Hasil Work 4
<img width="361" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/f03a7e27-981f-441a-b4e8-4426cb8e0b9a">

### Soal 20
Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.

* Setup lb Eisen menggunakan Least Connection :
```
touch /etc/nginx/sites-available/lb-laravel

echo 'upstream laravel {
        least_conn;
        server 10.3.4.1:8001;
        server 10.3.4.2:8002;
        server 10.3.4.3:8003;
}

server {
        listen 80;
        server_name riegel.canyon.a08.com;

        location / {
                proxy_pass http://laravel;
        }
}' > /etc/nginx/sites-available/lb-laravel

ln -s /etc/nginx/sites-available/lb-laravel /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t
```

* Testing menggunakan work 3 sebelumnya
<img width="298" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/f85c7634-3565-4982-8009-cf7e0fcaab88">


<img width="295" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/b1af2b9f-a080-4d24-813a-e0bbae1c3b13">


<img width="286" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/78c1e1c7-22f7-4f12-8d8e-56d238c0f797">


<img width="258" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/109232320/3779a50f-e96f-4ea6-a7c2-d7bad66e0993">


![Uploading image.png…]()

