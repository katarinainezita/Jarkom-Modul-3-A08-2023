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

* Tes Least Connection
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



### Soal 9
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.

### Soal 10
Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

### Soal 11
Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id.

### Soal 12
Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP.3.69], [Prefix IP.3.70], [Prefix IP.4.167], dan [Prefix IP.4.168].

Karena para petualang kehabisan uang, mereka kembali bekerja untuk mengatur granz.channel.yyy.com.

### Soal 13 
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern.

### Soal 14 
Frieren, Flamme, dan Fern memiliki Granz Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer

Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.

### Soal 15
POST /auth/register

### Soal 16 
POST /auth/login

### Soal 17
GET /me

### Soal 18
Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Granz Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.

### Soal 19
Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.

### Soal 20
Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.
