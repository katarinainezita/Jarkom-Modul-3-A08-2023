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

## Network Configuration
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

#### Laravel Worker

* Lawine
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
#### Solusi
Hal ini dilakukan dengan konfigurasi network untuk client yang sebelumnya sudah kita lakukan, yakni sebagai berikut:
```
auto eth0
iface eth0 inet dhcp
```

### Soal 2
Client yang melalui Switch3 mendapatkan range IP dari 10.3.16 - 10.3.32 dan 10.3.64 - 10.3.80

#### Solusi
Hal ini dilakukan dengan menambahkan konfigurasi pada DHCP server di file /etc/dhcp/dhcpd.conf untuk range ip nya dengan menggunakan subnet berikut ini:
```
subnet 10.3.3.0 netmask 255.255.255.0 {
    range 10.3.3.16 10.3.3.32;
    range 10.3.3.64 10.3.3.80;
    option routers 10.3.3.8;
    option broadcast-address 10.3.3.255;
    option domain-name-servers 10.3.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}
```
Pastikan DHCP server sudah install isc-dhcp-server dengan command berikut ini:
```
apt-get update
apt-get install isc-dhcp-server
```

### Soal 3
Client yang melalui Switch4 mendapatkan range IP dari 10.3.4.12 - 10.3.4.20 dan 10.3.4.160 - 10.3.4.168

#### Solusi
Sama seperti nomor sebelumnya, kita hanya perlu menambahkan konfigurasi di file dhcpd.conf untuk mengatur range ip menggunakan subnet dengan command berikut ini:
```
subnet 10.3.4.0 netmask 255.255.255.0 {
    range 10.3.4.12 10.3.4.20;
    range 10.3.4.160 10.3.4.168;
    option routers 10.3.4.8;
    option broadcast-address 10.3.4.255;
    option domain-name-servers 10.3.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
```

### Soal 4 
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

#### Solusi
Untuk menyelesaikan nomor ini, kita perlu menambahkan konfigurasi pada DNS Master (Heiter) untuk mem-forward ip nya ke ip internet, yakni 192.168.122.1 dengan command berikut ini:
```
echo 'options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```
Pastikan DNS Master sudah menginstall bind9 dengan command berikut ini:
```
apt update
apt install bind9 -y
```

Kemudian, baru kita dapat mengatur nameserver client agar mengarah ke ip DNS Master dengan command shell script sebagai berikut:
```
echo 'nameserver 10.3.1.2' > /etc/resolv.conf
```

### Soal 5
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit 

Berjalannya waktu, petualang diminta untuk melakukan deployment.

#### Solusi
Hal ini dapat diselesaikan dengan mengatur lease time dan max lease time sesuai dengan permintaan pada konfigurasi subnet pada DHCP Server yang telah kita gunakan untuk mengatur range ip sebelumnya seperti berikut:
```
subnet 10.3.3.0 netmask 255.255.255.0 {
    range 10.3.3.16 10.3.3.32;
    range 10.3.3.64 10.3.3.80;
    option routers 10.3.3.8;
    option broadcast-address 10.3.3.255;
    option domain-name-servers 10.3.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}
subnet 10.3.4.0 netmask 255.255.255.0 {
    range 10.3.4.12 10.3.4.20;
    range 10.3.4.160 10.3.4.168;
    option routers 10.3.4.8;
    option broadcast-address 10.3.4.255;
    option domain-name-servers 10.3.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
```


### Soal 6
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.

#### Solusi
Untuk menyelesaikan soal ini, kita dapat menggunakan nginx untuk melakukan konfigurasi virtual host website pada script seperti berikut:  

Setup worker (Lawine, Linie, Lugner) :

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

### Soal 7
Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
a. Lawine, 4GB, 2vCPU, dan 80 GB SSD.
b. Linie, 2GB, 2vCPU, dan 50 GB SSD.
c. Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

#### Solusi
Sebelum melakukan testing, kita perlu setting konfigurasi untuk load balancer (Eisen) dan client (bebas) untuk proses testingnya.

- Setup load balancer eisen : 
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

- Setup client (bebas)
```
echo 'nameserver 10.3.1.2' > /etc/resolv.conf
apt-get update && apt-get install lynx htop apache2-utils
```
Kemudian, barulah kita dapat melakukan testing request menggunakan command ab yang dapat kita pakai setelah install apache2-utils seperti berikut:
```
apt-get update && apt-get install lynx apache2-utils
``` 

Setelah itu, lakukan test request ke ip load balancer atau langsung ke domain yang telah kita config sebelumnya seperti berikut ini:
```
ab -n 200 -c 10 http://10.3.2.2/
ab -n 200 -c 10 http://granz.channel.a08.com/
```

Untuk mengecek apakah testing berhasil atau tidak, kita dapat melihat log pada masing - masing worker dengan script sebagai berikut:
```
echo '' > /var/log/nginx/jarkom_access.log
cat /var/log/nginx/jarkom_access.log| grep "GET" | wc -l
```

![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/deb2e74e-a0f1-421f-8de2-02db215c8186)

* Bukti di Worker

![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/18e446fe-6265-40bf-85e3-56b31cf2617c)
![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/645464ed-95c9-4d37-a96d-e933065bee23)
![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/9442da2e-d5a5-4dbf-ac5d-ae9c30b43f8d)

### Soal 8
Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
a. Nama Algoritma Load Balancer
b. Report hasil testing pada Apache Benchmark
c. Grafik request per second untuk masing masing algoritma. 
d. Analisis

- Round Robin
  
    ```
    echo 'nameserver 192.168.122.1' > /etc/resolv.conf
    apt-get update && apt-get install nginx apache2-utils -y
    echo 'nameserver 10.3.1.2' > /etc/resolv.conf

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
  
  <img width="751" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/5037ea23-5928-48de-978a-fd46e7db5f2d">
  <img width="957" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/32668ae9-0276-4a9d-bc58-452b3f8e0124">
  <img width="956" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/058d5fdd-7014-4df6-bb2e-c058e3f252ce">
  <img width="957" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/47c82b82-bb34-4bc3-b3e2-af30a10ffeb8">

- Least Connection
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

  <img width="808" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/7a81b4d7-b29e-427a-9d53-6ed82b7df438">
  <img width="958" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/d01e40e4-fbda-4211-aea8-d7e116915270">
  <img width="958" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/78de7192-832f-478a-a2ec-84c92531551a">
  <img width="958" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/334baf4b-0d95-4e24-a3ba-45f67cde6d81">

- IP Hash
    ```
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
    ```

  <img width="790" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/a2b1ebc3-b299-4fff-b691-4cdc2274ea0b">
  <img width="959" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/9af1f007-50ff-44e7-9d89-c72e428ee4aa">
  <img width="957" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/91ad54ee-e3a0-4c02-9553-03589dcbf7c6">
  <img width="957" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/7675ce4d-c5d0-47e2-9247-e3e3fa6694cf">

- Generic Hash
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

  <img width="755" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/4fa3efb2-60b9-4862-ae96-dd87ba40abe7">
  <img width="956" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/c5f74903-4805-4cbc-b174-977d1cef0bad">
  <img width="955" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/116d7f38-d882-4d67-92bb-a7dabbba52f0">
  <img width="956" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/623de2be-a383-43f8-b0b7-45057c84b7ee">

- Selanjutnya, kita dapat mengecek hasil testing di masing - masing worker dengan script sebagai berikut:
  ```
  echo '' > /var/log/nginx/jarkom_access.log
  cat /var/log/nginx/jarkom_access.log| grep "GET" | wc -l
  ```

- Hasil testing waktu praktikum:
  
  <img width="590" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/f70fd52c-7080-4b50-982f-ece95f766bfc">


### Soal 9
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.

#### Solusi
Untuk melakukan hal ini, kita dapat setting ulang konfigurasi nginx dengan konfigurasi default (round robin) pada load balancer (Eisen), kemudian jalankan command testing berikut ini di salah satu client:
```
ab -n 100 -c 10 http://granz.channel.a08.com/
```

* 3 Worker
  
  ![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/b2e12e04-cd36-48bb-b023-07b74e47afb9)
  ![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/5e25172d-9898-454b-bf92-88e9888581c0)
  ![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/1c8ee9da-fbd3-48d6-a5f0-44b938379e85)
  ![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/20fc36f2-10df-4bb5-b9bc-ccf4ee268958)

* 2 Worker 
  
  ![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/898e9006-6252-4c9f-b7e7-b3d51c88029d)
  ![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/fa1e1c40-7d84-4a0a-86aa-d33310f9b06a)
  ![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/2135da9e-b7d1-42e7-b9c2-5733ed0fe8c6)

* 3 Worker
  
  ![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/ff7937f7-1a6f-4dec-b682-2c98da83631c)
  ![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/dbb1642a-941d-41af-99f3-97454a1e35b5)

- Hasil testing waktu praktikum:  
  
  <img width="420" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/1be82fbb-61b5-40da-a53d-0c6647e8ab9f">

### Soal 10
Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

#### Solusi
Hal ini dapat dilakukan dengan menambahkan konfigurasi tambahan untuk authentifikasi pada load balancer (Eisen) seperti berikut:

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

Konfigurasi di atas adalah memberikan authentifikasi berupa username dan password saat kita akan mengakses granz.channel.a08.com dengan username netics dan password ajka08

Kemudian, test konfigurasi ini pada salah satu client dan hasilnya seperti ini:
<img width="934" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/b8dd4933-c9bd-4f6b-8c2b-b7890c1dfc70">
<img width="937" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/0db44a3d-610f-4e8d-b19e-80aefac8019d">
<img width="938" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/1f1f2577-d8c2-4b4d-992e-1cbda7e34b96">

### Soal 11
Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id untuk semua request yang mengandung /its.

#### Solusi
Hal ini dapat dilakukan dengan menambahkan konfigurasi pada load balancer (Eisen) berupa proxy_pass yang mengarah ke www.its.ac.id seperti script berikut ini:
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
Kemudian tes konfigurasi ini pada salah satu client (bebas) dan hasilnya seperti ini:
<img width="956" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/c2129eaa-fb5e-46b0-93c0-99d8319af0c0">
<img width="939" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/0502b92e-7b86-4d96-b7d3-c53a54f7387d">

### Soal 12
Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [10.3.3.69], [10.3.3.70], [10.3.4.167], dan [103.3.4.168].

Karena para petualang kehabisan uang, mereka kembali bekerja untuk mengatur granz.channel.yyy.com.

#### Solusi
Hal ini dapat dilakukan dengan menambahkan konfigurasi pada load balancer (Eisen) berupa allow command seperti berikut ini:

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

Kemudian tes konfigurasi ini pada salah satu client yang memiliki ip yang tidak termasuk pada ip yang di allow pada konfigurasi tersebut:

<img width="940" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/e3678501-b545-49b3-a567-1d474e818d7f">

Untuk tes yang bisa, kita dapat mengubah konfigurasi di load balancer untuk allow ip salah satu client yang akan kita buat testing. 
Untuk itu, cari tahu terlebih dahulu ip client dengan command `ip a`, kemudian allow ip tersebut pada konfigurasi di load balancer. Jika sudah, maka hasilnya akan seperti berikut:

<img width="927" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/aa661e3a-3cf2-4aca-87e7-9ebffa588b49">



### Soal 13
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. 
Frieren, Flamme, dan Fern memiliki Granz Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer
Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.

#### Solusi
Hal ini dilakukan dengan melakukan konfigurasi terlebih dahulu untuk setup database pada Denken seperti berikut ini: 
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
Kemudian lakukan setup database client untuk setiap worker laravel dengan script sebagai berikut:
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

Agar lebih mudah untuk connect ke database sewaktu - waktu, kita dapat membuatkan script untuk connect seperti berikut ini,
* Setup script untuk connect ke database :
```
mariadb --host=10.3.2.1 --port=3306 --user=kelompoka08 --password=ajka08
```

Cek pada worker, jika sudah berhasil maka tandanya akan seperti ini:

<img width="872" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/10acfea4-8fdd-4ec5-a597-cc6019eeac27">
  
Kemudian tes apakah db sudah berjalan atau belum dengan script connect yang sudah kita buat sebelumnya cek apakah database yang kita deklarasi di denken sudah ada atau belum seperti berikut:

<img width="901" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/70cfe5c6-5c04-40a7-a856-6a4714d2c17b">

### Soal 15
POST /auth/register
#### Solusi
Untuk mempermudah dalam menyelesaikan soal ini, kita dapat membuat file .json yang berisi data username dan password yang akan di register, seperti berikut ini:
```
{
  "username": "vron",
  "password": "bismillah"
}
```

Siapkan juga sebuah file .data kosong sebagai tempat menampung hasil data yang didapat untuk nantinya bisa digunakan untuk membuat graph hasil benchmark.

Kemudian lakukan testing menggunakan command ab dari apache2-utils dengan ketentuan jumlah request dan request per second yang diminta sebagai berikut:
```
ab -n 100 -c 10 -T 'application/json' -p auth_data.json -g register_result.data http://10.3.4.1:8001/api/auth/register
```
<img width="861" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/d5c1c90f-a4e5-421a-a534-11836d720e14">
<img width="863" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/e0e2b96d-0138-4c0a-be74-b098372e477d">

Dapat dilihat dari hasil di atas bahwa dari 100 request, 99 nya failed. Hal ini disebabkan oleh codingan yang ada di repo hanya mengizinkan username yang unique untuk dapat dimasukkan ke database sehingga 99 proses lainnya akan tidak diproses.

Kita juga dapat men-testing apakah deploy nginx kita juga berjalan atau tidak dengan menembak api tersebut menggunakan command seperti berikut:
```
curl -X POST -H "Content-Type: application/json" -d '{"username": "zen", "password": "bismillah"}' http://10.3.4.1:8001/api/auth/register
```
![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/5180c8d5-1cb8-4286-bc5b-89c4e836ad3f)

### Soal 16 
POST /auth/login
#### Solusi
Untuk mempermudah dalam menyelesaikan soal ini, kita dapat menggunakan file .json yang sama dengan yang kita gunakan sebelumnya untuk register. Siapkan juga sebuah file .data kosong sebagai tempat menampung hasil data yang didapat untuk nantinya bisa digunakan untuk membuat graph hasil benchmark.

kemudian lakukan testing menggunakan command ab dari apache2-utils dengan ketentuan jumlah request dan request per second yang diminta sebagai berikut:
```
ab -n 100 -c 10 -T 'application/json' -p auth_data.json -g login_result.data http://10.3.4.1:8001/api/auth/login
```
<img width="858" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/a73f7638-853c-4209-a4bb-4998ed865b7d">
<img width="863" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/943f7032-78ef-4651-aba0-c0519175aec3">

Dapat dilihat bahwa dari 100 request yang dijalankan, terdapat 40 request yang failed untuk dihandle oleh satu worker saja. Kita juga dapat men-testing apakah deploy nginx kita juga berjalan atau tidak dengan menembak api tersebut menggunakan command seperti berikut:
```
curl -X POST -H "Content-Type: application/json" -d '{"username": "zen", "password": "bismillah"}' http://10.3.4.1:8001/api/auth/login
```
![image](https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/99c7e6d8-fc07-41dd-bba0-bf8c08ea1cf2)

### Soal 17
GET /me
#### Solusi
Untuk mempermudah dalam menyelesaikan soal ini, kita dapat menggunakan token yang kita dapatkan sebelumnya dari testing api login dan menyimpannya ke dalam sebuah file .txt sehingga nantinya kita tinggal menggunakannya pada request get me ini. Untuk request get me dengan token yang sudah kita dapatkan dan simpan sebelumnya, kita dapat menggunakan command ab seperti berikut:
```
token=$(cat temp_token.txt); ab -n 100 -c 10 -H "Authorization: Bearer $token" http://10.3.4.1:8001/api/me
```
<img width="858" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/75f8a963-9c0d-4e82-b3ad-9647ce3c8078">
<img width="862" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/e34ee33f-f29d-46f7-897e-2310e50c5f76">

Dapat dilihat bahwa dari 100 request yang dijalankan, semuanya dapat dihandle dengan baik meski hanya dengan menggunakan 1 worker saja. Hal ini dibuktikan dengan tidak adanya failed request hasil benchmark yang telah dijalankan.

- Hasil testing waktu praktikum:  
  
  <img width="620" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/dff17b27-1436-4de5-aeb6-01eb97e54af6">
  

### Soal 18
Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Granz Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.

#### Solusi
Untuk menyelesaikan soal ini, kita perlu setup kembali load balancer untuk melakukan bind pada ip nya, yakni dengan konfigurasi seperti ini:
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

unlink /etc/nginx/sites-enabled/lb-laravel
ln -s /etc/nginx/sites-available/lb-laravel /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t
```
Kemudian lakukan testing salah satu endpoint pada salah satu client menggunakan command ab. Namun pastikan terlebih dahulu bahwa login credential yang digunakan, sudah ter-register pada database dengan melakukan pengecekan terlebih dahulu pada salah satu worker menggunakan command berikut ini:
```
mariadb --host=10.3.2.1 --port=3306 --user=kelompoka08 --password=ajka08
```
Kemudian masuk ke database dan query semua user yang sudah terdaftar menggunakan command seperti berikut ini:
```
USE dbkelompoka08;
SELECT * FROM users;
```
Jika akun user yang ingin kita tes menggunakan endpoint login sudah terdaftar, maka kita dapat melakukan testing dengan command ab seperti berikut ini:
```
ab -n 100 -c 10 -T 'application/json' -p login_data.json -g login_result.data http://riegel.canyon.a08.com/api/auth/login
```

<img width="855" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/fa8f8dd2-378a-417f-ac25-90d0a2c0138d">
<img width="864" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/d3aeaee4-c96d-429a-be98-69dd94d21d8c">
<img width="864" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/a3078bf1-449b-42cc-99f7-949ce9151545">
<img width="866" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/35989ac4-3f0d-407f-a3fd-e5df47cb7d1b">
<img width="865" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/2367aa7e-389b-4c4d-a3f2-32a8e34596b2">
<img width="856" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/1c1ebfb7-40c3-4369-8ca9-be93fc41b0d0">
<img width="854" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/d66dca51-6962-49a9-8505-bb94c3308e0a">

### Soal 19
Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.
#### Solusi
Untuk menyelesaikan permasalahan ini, kita dapat melakukan perubahan konfigurasi pada file /etc/php/8.0/fpm/pool.d/www.conf di masing - masing worker untuk memanipulasi kinerja PHP-FPM dengan menaikkan atribut - atribut yang sudah disebutkan dalam soal. 
Berikut ini adalah konfigurasi default PHP-FPM :
```
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

#### Konfigurasi Kenaikan 1 :
```
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 25
pm.start_servers = 5
pm.min_spare_servers = 3
pm.max_spare_servers = 10' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

#### Konfigurasi Kenaikan 2 :
```
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 50
pm.start_servers = 8
pm.min_spare_servers = 5
pm.max_spare_servers = 15' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

#### Konfigurasi Kenaikan 3 :
```
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

#### Konfigurasi Default
<img width="856" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/f11bdf13-f021-433a-a5f2-9b208c54c454">
<img width="865" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/8ffaaf93-5aba-4f0e-8868-76129055fce5">
<img width="864" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/bbb02315-066f-41d8-a45d-36dec60f79c2">
<img width="865" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/2739fd82-b38b-4760-9b3c-9ca3ac476844">


#### Kenaikan 1
<img width="861" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/10611f9b-afb5-4d82-abc2-8d5c90a5cfbb">
<img width="864" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/4e73e911-d4a0-4c6d-834d-559611e0f815">
<img width="863" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/07f18ff2-052c-416c-ab85-83e8ce2f785f">
<img width="864" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/85f3bdaf-9048-4ad6-8f28-25c1080a39ef">


#### Kenaikan 2
<img width="860" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/5695c959-fa71-4fa7-aae8-48cfc0042ec2">
<img width="864" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/08c68a5a-17fb-4ca4-aea2-d18656455a0a">
<img width="864" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/b2b7a224-e652-4627-b0a6-2defdb372210">
<img width="865" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/9748bf2e-a5ca-4575-87ba-92693f07ffac">

#### Kenaikan 3
<img width="859" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/85f55b33-e05d-40bb-983b-83b81662b7c7">
<img width="863" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/08daa3cd-4cba-4976-b7ca-d3e6655cea28">
<img width="863" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/e9652fcc-d016-4f38-bfa2-bda1789c31c5">
<img width="863" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/231fb7b8-98f4-4df8-a6ec-ac16b7555ee3">

- Grafik hasil testing waktu praktikum:  
  
  <img width="485" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/1e8108d9-41d7-4d59-9e86-26f64172ac45">

### Soal 20
Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.

#### Solusi
Untuk menyelesaikan permasalahan ini, kita dapat setup kembali load balancer eisen dengan konfigurasi untuk least connection seperti berikut :
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

unlink /etc/nginx/sites-enabled/lb-laravel
ln -s /etc/nginx/sites-available/lb-laravel /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t
```

Dengan konfigurasi PHP-FPM yang masih sama dengan konfigurasi kenaikan terakhir (yang ketiga) seperti berikut ini:
```
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```

Kemudian lakukan testing salah satu endpoint pada salah satu client menggunakan command ab seperti berikut ini:

<img width="859" alt="image" src="https://github.com/katarinainezita/Jarkom-Modul-3-A08-2023/assets/105977864/08a6a8f2-7e95-499e-bb09-b8817eb2ea74">

Dapat dilihat bahwa dengan menggunakan least connection dan konfigurasi PHP-FPM yang sudah ditingkatkan, total request per second juga semakin meningkat.