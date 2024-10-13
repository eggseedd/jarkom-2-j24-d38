[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/99wpTe72)
| Name           | NRP        | Kelas     |
| ---            | ---        | ----------|
| Raditya Yusuf Annaafi' | 5025231033 | Jaringan Komputer (D) |
| Aqila Zahira Naia Puteri Arifin | 5025231138 | Jaringan Komputer (D) |

## Find your topology here!

- Link: https://drive.google.com/drive/folders/1ECQD6-cQkg0DzyflG-jSxJZaGaxg0KSU?usp=sharing

- Topology distribution for groups: https://docs.google.com/spreadsheets/d/1QKEZjixTStNbdXznOalJoJS0UQ6ed23o51pP8t8eAIM/edit?gid=1757558734#gid=1757558734

## Put your topology config image here!

![3](https://github.com/user-attachments/assets/1dd6cb6e-5edc-4f39-9813-54429f034e7a)

## File GNS3 Project

https://drive.google.com/file/d/1rBo7mWKW-RwUyTiHZofqMvHfebJ2ZvT5/view?usp=sharing

<br>

## Soal 1

> Topologi terdiri dari node Wortel yang berupa DNS Master*. Selain itu, terdapat pula node Pokcoy sebagai DNS Slave*, yang bertugas sebagai cadangan dari node Wortel.
> <br> </br>
> Selanjutnya terdapat node Tomat dan Taoge yang bekerja sebagai Client*, tiga buah Web Server* yaitu Bayam, Buncis, dan Brokoli, serta Mayur sebagai Router*. Buatlah topologi sesuai dengan pembagian topologi [di sini](https://docs.google.com/spreadsheets/d/1QKEZjixTStNbdXznOalJoJS0UQ6ed23o51pP8t8eAIM/edit?usp=sharing) dan konfigurasi topologi [di sini](https://drive.google.com/drive/folders/1ECQD6-cQkg0DzyflG-jSxJZaGaxg0KSU?usp=sharing). Pastikan bahwa setiap node dapat terhubung ke Internet.

> _The topology consists of a Wortel node which is a DNS Master*. In addition, there is also a Pokcoy node as a DNS Slave*, which serves as a backup for the Wortel node._
> <br> </br>
> _Furthermore, there are Tomat and Taoge nodes that work as Client*, three Web Servers*, namely Bayam, Buncis, and Brokoli, then finally Mayur as Router*. Make a topology according to the topology division [here](https://docs.google.com/spreadsheets/d/1QKEZjixTStNbdXznOalJoJS0UQ6ed23o51pP8t8eAIM/edit?usp=sharing) and the topology configuration [here](https://drive.google.com/drive/folders/1ECQD6-cQkg0DzyflG-jSxJZaGaxg0KSU?usp=sharing). Make sure that each node can connect to the Internet._

**Answer:**

- Screenshot

 ![image](https://github.com/user-attachments/assets/1203c72b-af40-4ad2-853a-3fcb5ca8ea27)


- Explanation

  Buat topologi sesuai yang telah diberikan. Kemudian, buat konfigurasi seperti ini:
  <br>
  Node Mayur
  ```
  auto eth1
  iface eth1 inet dhcp
  
  auto eth0
  iface eth0 inet static
  	address 10.183.1.1
  	netmask 255.255.255.0
  
  auto eth2
  iface eth2 inet static
  	address 10.183.2.1
  	netmask 255.255.255.0
  ```
  Node yang terhubung dengan switch 1 (selain Mayur)
  ```
  auto eth0
  iface eth0 inet static
  	address 10.183.1.[2 s/d 4]
  	netmask 255.255.255.0
  	gateway 10.183.1.1
  ```
  Node yang terhubung dengan switch 2 (selain Mayur)
  ```
  auto eth0
  iface eth0 inet static
  	address 10.183.2.[2 s/d 5]
  	netmask 255.255.255.0
  	gateway 10.183.1.1
  ```
  Setelah itu, masukkan command berikut pada /root/.bashrc untuk node:
  <br>
  Mayur
  ```
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.183.0.0/16
  ```
  Lainnya
  ```
  echo nameserver 192.168.122.1 > /etc/resolv.conf
  ```


<br>

## Soal 2

> Tambahkan konfigurasi untuk domain bayam.yyy.com yang mengarah ke IP node Bayam di DNS Master. Dengan cara yang sama, buat konfigurasi domain brokoli.yyy.com yang mengarah ke IP node Brokoli dan domain buncis.yyy.com yang mengarah ke IP node Buncis. Simpan semua konfigurasi dalam folder Jarkom. Selama pengerjaan soal, ubah yyy menjadi kode kelompok masing-masing (contoh: A02).
> <br> </br>
> Jangan lupa update konfigurasi kedua client agar dapat berkomunikasi dengan semua domain tersebut.


> _Add a configuration for bayam.yyy.com domain that points to the Bayam node IP in the DNS Master. In the same way, create a brokoli.yyy.com domain configuration that points to the Brokoli node IP and a buncis.yyy.com domain that points to the Buncis node IP. Save all configurations in a folder called Jarkom. For this practicum, substitute yyy with the code of each group (ex: A02).
> <br> </br> 
> Don't forget to update the configuration of both clients so that they can communicate with the domains._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/792c6feb-a247-4450-93e1-d9d5a6674d73)
  ![Screenshot 2024-10-04 115322](https://github.com/user-attachments/assets/0c278764-69a3-48ad-98aa-0bea0d8c1622)

- Explanation

  **Install bind9 pada WortelDNSMaster**
  ```
  apt-get update

  apt-get install bind9 -y
  ```

  **Buat 3 domain yang diminta**

  Run command
  ```
  nano /etc/bind/named.conf.local
  ```
  Isi dengan
  ```
  zone "bayam.d38.com" {
    type master;
    file "/etc/bind/jarkom/bayam.d38.com";
  };
  
  zone "brokoli.d38.com" {
    type master;
    file "/etc/bind/jarkom/brokoli.d38.com";
  };

  zone "buncis.d38.com" {
    type master;
    file "/etc/bind/jarkom/buncis.d38.com";
  };
  ```
  Buat folder `jarkom`
  ```
  mkdir /etc/bind/jarkom
  ```
  Copy `db.local` 3 kali untuk masing-masing domain
  ```
  cp /etc/bind/db.local /etc/bind/jarkom/bayam.d38.com
  cp /etc/bind/db.local /etc/bind/jarkom/brokoli.d38.com
  cp /etc/bind/db.local /etc/bind/jarkom/buncis.d38.com
  ```
  Edit file `bayam.d38.com`, `brokoli.d38.com`, dan `buncis.d38.com` seperti ini
  ![image](https://github.com/user-attachments/assets/268a4af9-a18f-4553-8616-91ade65f9e8b)
  ![image](https://github.com/user-attachments/assets/98eb2cac-7def-404e-bb8b-7e45720cfcd0)
  ![Screenshot 2024-10-04 114555](https://github.com/user-attachments/assets/ebfdce4d-89b7-40b8-8ce7-b5746e509c4e)
  Restart bind9 dengan
  ```
  service bind9 restart
  ```
  
  **Test pada TomatClient**
  
  Tambahkan `nameserver 10.183.1.3(IP WortelDNSMaster)` pada `/etc/resolv.conf` node TomatClient. Dan jadikan `nameserver 192.168.122.1` yang sudah ditambahkan pada awal konfigurasi sebagai comment.
  Setelah itu, ping kedua domain dengan
  ```
  ping bayam.d38.com -c 3
  ping brokoli.d38.com -c 3
  ping buncis.d38.com -c 3
  ```

<br>

## Soal 3

> Tambahkan domain alias berupa www.bayam.yyy.com pada alamat bayam.yyy.com dan www.brokoli.yyy.com pada alamat brokoli.yyy.com.

> _Add a domain alias in the form of www.bayam.yyy.com to the bayam.yyy.com address and www.brokoli.yyy.com to the brokoli.yyy.com address._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/7316ad93-275e-4a70-b078-d8ce7735b582)

- Explanation

  **Tambahkan alias di WortelDNSMaster**
  
  Buka file `bayam.d38.com` dan `brokoli.d38.com` pada folder jarkom dan tambahkan line code seperti berikut
  ![image](https://github.com/user-attachments/assets/12768e8e-01ed-4c00-a096-48073aa08667)
  ![image](https://github.com/user-attachments/assets/e7da39ad-f2d9-4d10-8081-e5a509e0f3c1)
  Restart bind9 dengan
  ```
  service bind9 restart
  ```

  **Test di TomatClient**

  Lakukan 2 command di bawah ini pada TomatClient
  ```
  ping www.bayam.d38.com -c 3
  ping www.brokoli.d38.com -c 3
  ```

<br>

## Soal 4

> Tambahkan record reverse domain untuk domain brokoli.yyy.com dan buncis.yyy.com.

> _Add a reverse domain record for brokoli.yyy.com and buncis.yyy.com domains._

**Answer:**

- Screenshot

  ![Screenshot 2024-10-04 131701](https://github.com/user-attachments/assets/e6a8f473-9932-46ab-a0d9-895a5fd48161)

- Explanation

  **Buat record PTR**
  
  Run command
  ```
  nano /etc/bind/named.conf.local
  ```
  Isi dengan
  ```
  zone "2.183.10.in-addr.arpa" {
      type master;
      file "/etc/bind/jarkom/2.183.10.in-addr.arpa";
  };
  ```
  Copy `db.local` sebagai template
  ```
  cp /etc/bind/db.local /etc/bind/jarkom/2.183.10.in-addr.arpa
  ```
  Ubah isi file `2.183.10.in-addr.arpa` menjadi seperti berikut
  ![Screenshot 2024-10-04 131345](https://github.com/user-attachments/assets/7d412817-09ba-4a62-8150-7d5cb09a03a6)

  Restart bind9
  ```
  service bind9 restart
  ```

  **Test di TomatClient**

  Install DNSUtils
  ```
  apt-get update
  apt-get install dnsutils
  ```
  Test dengan menjalankan
  ```
  host -t PTR 10.183.2.3
  host -t PTR 10.183.2.4
  ```
  

<br>

## Soal 5

> Ubah record SOA dari domain bayam.yyy.com sesuai dengan ketentuan berikut:
> - Lama waktu server slave menunggu untuk mengecek salinan baru server master adalah sebesar 2 jam.
> - Field yang mengatur revisi file zona ini diubah menjadi tanggal awal praktikum (format YYYYMMDD) kemudian diikuti dengan nomor kelompok (contoh untuk kelompok A02 maka nomornya 02).
> - Lamanya waktu server harus menunggu untuk meminta pembaruan lagi dari nameserver master yang tidak responsif sebesar 30 menit.
> - Lama waktu nama domain di-cache secara lokal sebelum kadaluarsa dan kembali ke nameserver otoritatif untuk informasi terbaru sebesar 12 jam.
> - Jika server slave tidak mendapatkan respons dari server master dalam waktu 2 minggu, server tersebut harus berhenti merespons kueri untuk zona tersebut.

> _Change the SOA record of the bayam.yyy.com domain according to the following conditions:_
> - The length of time the slave server waits to check for a new revision of the master server is 2 hours.
> - The field that regulates the revision of this zone file is changed to the start date of the practicum (YYYYMMDD format) then followed by the group number (ex: for A02 the group number would be 02).
> - The length of time the server has to wait to request another update from an unresponsive master nameserver is 30 minutes.
> - The length of time a domain name is cached locally before it expires and returns to an authoritative nameserver for up-to-date information is 12 hours.
> - If the slave server does not get a response from the master server within 2 weeks, it must stop responding to queries for that zone.

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/206cfd1f-3698-4eed-9b36-aff78e41cffb)

- Explanation

  Buka file `bayam.d38.com` di folder `jarkom` dengan
  ```
  nano /etc/bind/jarkom/bayam.d38.com
  ```
  Kemudian, ubah SOA sesuai yang diminta soal
  <br>
  
  Serial menjadi tanggal awal praktikum diikuti dengan nomor kelompok (38)
  ```
  2024100138        ; Serial
  ```
  Refresh menjadi 2 jam (7200 detik)
  ```
  7200        ; Refresh
  ```
  Retry menjadi 30 menit (1800 detik)
  ```
  1800        ; Retry
  ```
  Expire menjadi 2 minggu (1209600 detik)
  ```
  1209600        ; Expire
  ```
  Negative Cache TTL menjadi 12 jam (43200 detik)
  ```
  43200        ; Negative Cache TTL
  ```

<br>

## Soal 6

> Untuk menangani request yang berlebih dari client ke ketiga alamat yang tadi dibuat, konfigurasikan node Pokcoy sebagai DNS Slave yang bekerja untuk DNS Master Wortel.

> _To handle excess requests from the client to the three addresses created, configure the Pokcoy node as the DNS Slave that works for Wortel DNS Master._

**Answer:**

- Screenshot
   ![Screenshot 2024-10-04 125139](https://github.com/user-attachments/assets/8be53526-17fd-4990-9509-58c335f1b72d)
   ![image](https://github.com/user-attachments/assets/77c7409f-de0c-4de4-a10b-b903d2a508f2)

- Explanation

  **Konfigurasi pada DNSMaster**

  Edit `/etc/bind/named.conf.local` menjadi dengan syntax berikut pada setiap zone agar menjadi seperti di gambar
  ```
  zone "[Nama zone]" {
    type master;
    also-notify { 10.183.1.2; }; // IP PokcoyDNSSlave
    allow-transfer { 10.183.1.2; }; // IP PokcoyDNSSlave
    file "/etc/bind/jarkom/[Nama zone]";
  };
  ```
  ![Screenshot 2024-10-02 151041](https://github.com/user-attachments/assets/f4b1a5c8-5330-4fb8-a2a2-91479db0b324)
  ![image](https://github.com/user-attachments/assets/88f56f2f-1bc8-44d8-8bb9-9dfa6219de24)
  ![Screenshot 2024-10-04 123708](https://github.com/user-attachments/assets/f7c19de4-861d-42ad-9c99-fe1348d7ee03)
  ![image](https://github.com/user-attachments/assets/be978dc2-4ce6-4e23-9171-9743cae0c47c)

  Restart bind9
  ```
  service bind9 restart
  ```

  **Konfigurasi pada DNSSlave**

  Install bind9 pada `PokcoyDNSSlave`
  ```
  apt-get update
  apt-get install bind9 -y
  ```
  Tambahkan syntax berikut pada `/etc/bind/named.conf.local`
  ```
  zone "[Nama zone]" {
    type slave;
    masters { 10.183.1.3; }; // IP WortelDNSMaster
    file "/var/lib/bind/[Nama zone]";
  };
  ```
  ![image](https://github.com/user-attachments/assets/1aecf34d-4404-499e-8c1c-0b8593572f0c)
  ![image](https://github.com/user-attachments/assets/2b08f159-1478-47ff-9244-879c7c36c057)
  ![Screenshot 2024-10-04 124424](https://github.com/user-attachments/assets/d153ee5e-90b9-423e-9edf-3cbe8297e65c)
  ![image](https://github.com/user-attachments/assets/1a7a9b09-9396-4de7-b558-442a3382f5bb)

  Restart bind9
  ```
  service bind9 restart
  ```

  **Testing**

  Matikan DNSMaster dengan melakukan syntax berikut pada WortelDNSMaster
  ```
  service bind9 stop
  ```
  Arahkan TomatClient kepada `nameserver 10.183.1.2(IP DNSSlave)` dengan menambahkan syntax berikut pada `/etc/resolv.conf`
  ```
  nameserver 10.183.1.2
  ```
  Lakukan syntax-syntax berikut untuk menguji
  ```
  ping bayam.d38.com -c 3
  ping brokoli.d38.com -c 3
  ping buncis.d38.com -c 3
  host -t PTR 10.183.2.4 10.183.1.2
  ```

<br>

## Soal 7

> Karena membutuhkan tempat untuk menyimpan resep brokoli, buatlah subdomain berupa vitamin.brokoli.yyy.com dengan alias www.vitamin.brokoli.yyy.com dengan mendelegasikannya dari Wortel ke Pokcoy dengan alamat IP menuju Brokoli yang diatur di folder Vitamin.

> _Since we need a place to store Brokoli recipes, create a subdomain in the form of vitamin.brokoli.yyy.com with an alias of www.vitamin.brokoli.yyy.com by delegating it from Wortel to Pokcoy with an ip to the Brokoli node in a folder called Vitamin._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/83680c88-ec9e-4c58-8d8d-1df1e8ff5eb9)

- Explanation

  **Buat subdomain pada Wortel dan delegasikan ke Pokcoy**

  Pada Wortel, edit file `/etc/bind/jarkom/brokoli.d38.com` dan ubah menjadi seperti di bawah ini
  ```
  nano /etc/bind/jarkom/brokoli.d38.com
  ```
  ![image](https://github.com/user-attachments/assets/f89a0d4a-114c-40f0-873d-add619233c12)
  Kemudian, edit file `/etc/bind/named.conf.options` pada Wortel.
  ```
  nano /etc/bind/named.conf.options
  ```
  Lalu, comment **dnssec-validation auto;** dan tambahkan baris berikut pada `/etc/bind/named.conf.options`
  ```
  allow-query{any;};
  ```
  Restart bind9 pada Wortel
  ```
  service bind9 restart
  ```

  **Konfigurasi Pokcoy**

  Pada Pokcoy edit file `/etc/bind/named.conf.options`
  ```
  nano /etc/bind/named.conf.options
  ```
  Kemudian, comment **dnssec-validation auto;** dan tambahkan baris berikut pada `/etc/bind/named.conf.options`
  ```
  allow-query{any;};
  ```
  Lalu, edit file `/etc/bind/named.conf.local` dengan menambahkan seperti gambar di bawah
  ![image](https://github.com/user-attachments/assets/d95b3370-6dd8-41d4-9f92-10896436c3d8)
  Kemudian buat direktori dengan nama `vitamin` dan copy `db.local` ke direktori tersebut dan edit namanya menjadi `vitamin.brokoli.d38.com`
  ```
  mkdir /etc/bind/vitamin
  cp /etc/bind/db.local /etc/bind/vitamin/vitamin.brokoli.d38.com
  ```
  Kemudian edit file `vitamin.brokoli.d38.com` menjadi seperti dibawah ini
  ![image](https://github.com/user-attachments/assets/7bc4f7da-1395-4284-979f-7349833da764)
  Restart bind9 pada Pokcoy
  ```
  service bind9 restart
  ```

  **Testing pada TomatClient**
  Lakukan ping ke `vitamin.brokoli.d38.com` dan `www.vitamin.brokoli.d38.com` dari `TomatClient`
  ```
  ping vitamin.brokoli.d38.com -c 3
  ping www.vitamin.brokoli.d38.com -c 3
  ```  

<br>

## Soal 8

> Buatlah subdomain khusus untuk kandungan brokoli dengan akses k1.vitamin.brokoli.yyy.com dengan alias www.k1.vitamin.brokoli.yyy.com yang mengarah ke IP brokoli dan diatur di folder k1.  

> _Create a special subdomain for Brokoli content called k1.vitamin.brokoli.yyy.com with an alias called www.k1.vitamin.brokoli.yyy.com that point to Brokoli node and are organized in a folder called k1._

**Answer:**

- Screenshot

  ![Screenshot 2024-10-04 065354](https://github.com/user-attachments/assets/7f36ef87-3ba2-411e-8461-4ef340616efd)
  
- Explanation
  
  **Buat subdomain pada Wortel dan delegasikan ke Pokcoy**

  Pada Wortel, edit file `/etc/bind/jarkom/brokoli.d38.com` dan ubah menjadi seperti di bawah ini
  ```
  nano /etc/bind/jarkom/brokoli.d38.com
  ```
  ![image](https://github.com/user-attachments/assets/c00d8477-569a-4fc6-9c14-d4f83b6cb7df)
  Kemudian, edit file `/etc/bind/named.conf.options` pada Wortel.
  ```
  nano /etc/bind/named.conf.options
  ```
  Lalu, comment **dnssec-validation auto;** dan tambahkan baris berikut pada `/etc/bind/named.conf.options`
  ```
  allow-query{any;};
  ```
  Restart bind9 pada Wortel
  ```
  service bind9 restart
  ```

  **Konfigurasi Pokcoy**

  Pada Pokcoy edit file `/etc/bind/named.conf.options`
  ```
  nano /etc/bind/named.conf.options
  ```
  Kemudian, comment **dnssec-validation auto;** dan tambahkan baris berikut pada `/etc/bind/named.conf.options`
  ```
  allow-query{any;};
  ```
  Lalu, edit file `/etc/bind/named.conf.local` dengan menambahkan seperti gambar di bawah
  ![image](https://github.com/user-attachments/assets/80cfae61-2f3e-452c-a1f8-44b37569c667)
  Kemudian buat direktori dengan nama `k1` dan copy `db.local` ke direktori tersebut dan edit namanya menjadi `k1.vitamin.brokoli.d38.com`
  ```
  mkdir /etc/bind/k1
  cp /etc/bind/db.local /etc/bind/k1/k1.vitamin.brokoli.d38.com
  ```
  Kemudian edit file `k1.vitamin.brokoli.d38.com` menjadi seperti dibawah ini
  ![image](https://github.com/user-attachments/assets/ed30c0a2-13e2-4aeb-a55e-cdb56ba5a733)
  Restart bind9 pada Pokcoy
  ```
  service bind9 restart
  ```

  **Testing pada TomatClient**
  Lakukan ping ke `k1.vitamin.brokoli.d38.com` dan `www.k1.vitamin.brokoli.d38.com` dari `TomatClient`
  ```
  ping k1.vitamin.brokoli.d38.com -c 3
  ping www.k1.vitamin.brokoli.d38.com -c 3
  ```  

<br>

## Soal 9

> Bayam, Brokoli, dan Buncis masing-masing berfungsi sebagai web server nginx yang menyajikan resep khusus untuk jenis sayuran yang mereka tangani. Untuk mengaktifkan web server pada masing-masing worker, lakukan deployment website menggunakan sumber yang tersedia di sayur_webserver_nginx. Tambahkan konfigurasi untuk log error ke file /var/log/nginx/error.log dan log access ke file /var/log/nginx/access.log.

> _Bayam, Brokoli, and Buncis each function as nginx web servers that serve special recipes for the type of vegetables they handle. To activate the web server on each worker, do the deployment using the resources available in sayur_webserver_nginx. Add configuration for error log to the file /var/log/nginx/error.log and access log to the file /var/log/nginx/access.log._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/6c36aa08-ddf9-447f-94e5-25735ccfdfe1)

- Explanation

  **Setup nginx pada webserver**

  Install nginx, unzip, dan php pada `bayam, brokoli, dan buncis`
  ```
  apt-get update
  apt-get install nginx
  apt-get install php php-fpm -y
  apt-get install unzip -y
  ```
  Nyalakan nginx
  ```
  service nginx start
  ```
  Download file `sayur_webserver_nginx`
  ```
  wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1tFDk7pKRQLd3BMUcyvfAfEL-drvIxdSl' -O sayur_webserver_nginx.zip
  ```
  Unzip file `sayur_webserver_nginx`
  ```
  unzip sayur_webserver_nginx.zip
  ```
  Buat direktori `[Nama webserver]`
  ```
  mkdir var/www/[Nama webserver]
  ```
  Kemudian, masuk ke direktori hasil unzip dan pindahkan isinya ke `/var/www/[Nama webserver]`
  ```
  cd sayur_webserver_nginx
  mv [Nama file] /var/www/[Nama webserver]/
  ```
  Pergi ke direktori `/etc/nginx/sites-available` dan buat file `[Nama webserver]` yang berisi
  ```
  server {
  
  	listen 80;
  
  	root /var/www/[Nama webserver];
  
  	index index.php index.html index.htm;
  	server_name $[Nama webserver].d38.com;
  
  	location / {
  			try_files $uri $uri/ /index.php?$query_string;
  	}
  
  	# pass PHP scripts to FastCGI server
  	location ~ \.php$ {
  	include snippets/fastcgi-php.conf;
  	fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
  	}
  
  location ~ /\.ht {
  			deny all;
  	}
  
  	error_log /var/log/nginx/error.log;
  	access_log /var/log/nginx/access.log;
  }
  ```
  Buat `symlink` dengan
  ```
  ln -s /etc/nginx/sites-available/[Nama webserver] /etc/nginx/sites-enabled
  ```
  Pergi ke direktori `/etc/nginx/sites-enabled` lalu hapus file `default`
  ```
  rm default
  ```
  *Yang saya alami jika tidak menghapus file `default` adalah yang terbuka merupakan isi dari `/var/www/html` walaupun root pada file `bayam` sudah diubah menjadi seperti di atas.

  **Testing pada Taugeclient**

  Install lynx
  ```
  apt-get update
  apt-get install lynx
  ```
  Akses web menggunakan lynx
  ```
  lynx [Nama webserver].d38.com
  ```
  
<br>

## Soal 10

> Pada masing masing worker nginx, akan terdapat beberapa hal yang perlu diperbaiki pada resource yang diberikan untuk bisa menampilkan resep saat halaman dimuat. Analisis kesalahan yang ada di resource melalui file /var/log/nginx/error.log dan perbaiki hingga halaman bisa menampilkan resep sesuai dengan worker nya.

> _On each nginx worker, there will be several things that need to be fixed in the resources provided to be able to display recipes when the page is loaded. Analyze the errors in the resource through the /var/log/nginx/error.log file and fix it until the page can display recipes according to its worker._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/b8a71ce5-9e11-4db1-83b6-1e3dccd257e7)
  ![image](https://github.com/user-attachments/assets/dde60d27-ef51-4372-a250-187a0d8b1894)

- Explanation

  Buka file `/var/log/error.log`
  ```
  nano /var/log/error.log
  ```
  Di sana akan tertera error `Hostname tidak ditemukan!` seperti pada gambar
  ![image](https://github.com/user-attachments/assets/0483e6da-e007-4ad1-a2cc-a02f1bbd0554)
  Saat dicek pada file `index.php` yang diberikan, switch case nya salah karena yang hostname hanya berupa `namaSayur (Bayam)` bukan `namaSayurWebServer (BayamWebServer)`, sedangkan pada awal pengerjaan host diberi nama `namaSayurWebServer (BayamWebServer)`.
  Oleh karena itu, switch case pada `index.php` harus diubah menjadi seperti pada gambar berikut
  ![Screenshot 2024-10-04 165834](https://github.com/user-attachments/assets/5963fa50-c077-4b9b-8c1b-cc46abb79095)
  Setelah, itu lakukan command berikut pada `TaugeClient`
  ```
  lynx [Nama server].d38.com
  ```

<br>

## Soal 11

> Setelah website berhasil dideploy pada masing-masing worker (Bayam, Brokoli, dan Buncis) dan halaman dapat menampilkan resep sayuran yang sesuai,  buatlah custom access log ke file /var/log/nginx/access.log di masing-masing web server worker menggunakan format log tertentu seperti di bawah:
> - Tanggal dan waktu akses dalam format standar log.
Nama worker yang sedang dilayani (misalnya: Bayam, Brokoli, atau Buncis).
> - Alamat IP klien yang mengakses website.
> - Metode HTTP dan URI yang diakses oleh klien.
> - Status respons HTTP yang diberikan oleh server.
> - Jumlah byte yang dikirimkan dalam respons.
> - Waktu yang dihabiskan oleh server untuk menangani permintaan.
> <br> </br>
> Contoh format log yang sesuai: 
[01/Oct/2024:11:30:45 +0000] Jarkom Node Bayam Access from 192.168.1.15 using method "GET /resep/bayam HTTP/1.1" returned stat


> _After successfully deploying the website on each worker (Bayam, Brokoli, and Buncis) and ensuring the pages display the appropriate vegetable recipes, create a custom access log file at /var/log/nginx/access.log on each web server worker using a specific log format as described below:_
> - _Access date and time in standard log format._
> - _Name of the worker serving the request (e.g., Bayam, Brokoli, or Buncis)._
> - _Client IP address accessing the website._
> - _HTTP method and URI accessed by the client._
> - _HTTP response status provided by the server.__
> - _Number of bytes sent in the response.
> - _Time taken by the server to handle the request._
> <br> </br>
> _Example of the appropriate log format:
[01/Oct/2024:11:30:45 +0000] Jarkom Node Bayam Access from 192.168.1.15 using method "GET /resep/bayam HTTP/1.1" returned status 200 with 2567 bytes sent in 0.038 seconds_


**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/b6494460-a06f-473e-a2a8-de572a767be5)![image](https://github.com/user-attachments/assets/4686fa13-e4bf-411c-b0bd-51d69775d21f)

- Explanation

  Buka file `/etc/nginx/sites-available/[Nama webserver]`
  ```
  nano /etc/nginx/sites-available/[Nama webserver]
  ```
  Tambahkan command berikut sebelum bagian server untuk membuat custom log
  ```
  log_format custom_format '[$time_local] Jarkom Node $hostname Access from $remote_addr '
                 'using method "$request" returned stat $status '
                 'bytes $body_bytes_sent request time "$request_time"';
  ```
  ![image](https://github.com/user-attachments/assets/7be21b8f-8a10-4c42-bc88-8437f7eae2bf)
  Restart bind9
  ```
  service bind9 restart
  ```
  
<br>

## Soal 12

> Informasi vitamin pada sayur brokoli akan ditampilkan pada subdomain vitamin.brokoli.yyy.com di node brokoli, buatlah DocumentRoot yang disimpan pada /var/www/vitamin.brokoli.yyy. Konfigurasikan webserver dengan nama server vitamin.brokoli.yyy.com dan server alias www.vitamin.brokoli.yyy.com. Lakukan konfigurasi Apache Web Server pada Brokoli dengan menggunakan sumber yang tersedia di [sini](https://docs.google.com/uc?export=download&id=1QbGkKXo3jt4c68AdVAkl1hD4LolTUPg2).

> _For information on vitamins in brokoli will be displayed on the vitamin.brokoli.yyy.com subdomain on the brokoli node, create a DocumentRoot stored in /var/www/vitamin.brokoli.yyy. Configure the web server with the server name vitamin.brokoli.yyy.com and server alias www.vitamin.brokoli.yyy.com. Configure the Apache Web Server on Brokoli using [this resource](https://docs.google.com/uc?export=download&id=1QbGkKXo3jt4c68AdVAkl1hD4LolTUPg2)._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/2cd0ab1d-d988-402a-9fa8-1cbbb25b9acc)

- Explanation

  Install apache2 dan php pada `BrokoliWebServer`
  ```
  apt-get install apache2
  apt-get install php
  ```
  Matikan nginx jika masih menyala
  ```
  service nginx stop
  ```
  Run apache2
  ```
  service apache2 start
  ```
  Buat folder `/var/www/vitamin.brokoli.d38`
  ```
  mkdir /var/www/vitamin.brokoli.d38
  ```
  Download konfigurasi yang sudah diberikan
  ```
  wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1NhsaTLD4Zk06BZJCqdN_oqoxB3uIg2C7' -O apache_web.zip
  ```
  Unzip `apache_web.zip`
  ```
  unzip apache_web.zip
  ```
  Masuk ke direktori hasil unzip `apache_web.zip`
  ```
  cd vitamin.brokoli.yyy.com
  ```
  Pindahkan semua isinya ke folder `/var/www/vitamin.brokoli.d38`
  ```
  mv [Nama file/folder] /var/www/vitamin.brokoli.d38
  ```
  Masuk ke direktori `/etc/apache2/sites-available`, kemudian buka file `000-default.conf` ubah menjadi seperti pada gambar
  ![image](https://github.com/user-attachments/assets/6a1d3575-2298-4e37-bf70-b73b53c96fa2)
  Restart apache2
  ```
  service apache2 restart
  ```
  Akses web dari `TaugeClient`
  ```
  lynx vitamin.brokoli.d38.com
  ```

<br>

## Soal 13

> Pada subdomain vitamin.brokoli.yyy.com, terdapat subfolder /nutrisi yang menyediakan informasi tentang berbagai vitamin dalam brokoli, seperti Vitamin A, C, dan K. Aktifkan directory listing untuk folder /nutrisi, dan buatlah rewrite rule di Apache untuk memperbaiki URL agar halaman seperti www.vitamin.brokoli.yyy.com/nutrisi/vitamin_a.php dapat diakses hanya dengan www.vitamin.brokoli.yyy.com/nutrisi/vitamin_a. Pastikan setiap halaman vitamin dapat diakses langsung melalui url yang telah disederhanakan.

> _On the vitamin.brokoli.yyy.com subdomain, there is a /nutrisi subfolder that provides information about various vitamins in brokoli, such as Vitamin A, C, and K. Activate directory listing for the /nutrisi folder, and create a rewrite rule in Apache to fix the URL so that pages like www.vitamin.brokoli.yyy.com/nutrisi/vitamin_a.php can be accessed only with www.vitamin.brokoli.yyy.com/nutrisi/vitamin_a. Make sure each vitamin page can be accessed directly through the simplified url._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/e09f437f-f5ed-4eaa-8500-a7b3347498c4)

- Explanation

  Pindah ke directory `/etc/apache2/sites-available` kemudian buka file `000-default.conf` dan tambahkan
  ```
  <Directory /var/www/vitamin.brokoli.d38/nutrisi>
     Options +Indexes
  </Directory>
  ```
  ![image](https://github.com/user-attachments/assets/b308e39b-71a0-48e0-ad0d-8bd8a0261cf3)
  Jalankan perintah `a2enmod rewrite` untuk mengaktifkan *module rewrite*
  Restart apache dengan perintah `service apache2 restart`
  ![image](https://github.com/user-attachments/assets/dfba4afa-d9ee-4e2f-9200-e09df9f5acaa)
  Pindah ke directory `/var/www/vitamin.brokoli.d38` dan buat file `.htaccess` dengan isi file
  ```
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule ^([^\.]+)$ $1.php [NC,L]
  ```
  Pindah ke directory `/etc/apache2/sites-available` kemudian buka file `000-default.conf` dan tambahkan
  ```
  <Directory /var/www/vitamin.brokoli.d38>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
  </Directory>
  ```
  Restart apache dengan perintah
  ```
  service apache2 restart
  ```
  Akses web dari client
  ```
  lynx vitamin.brokoli.d38.com/nutrisi/vitamin_a
  ```

<br>

## Soal 14

> Tambahkan alias untuk folder /public/images/ pada subdomain www.vitamin.brokoli.yyy.com agar folder tersebut dapat diakses langsung melalui url www.vitamin.brokoli.yyy.com/img.

> _Add an alias for the /public/images/ folder on the www.vitamin.brokoli.yyy.com subdomain so that the folder can be accessed directly through the url www.vitamin.brokoli.yyy.com/img._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/e2640737-4e8e-4cdf-9b8c-6bc2a1180d50)

- Explanation

  Pindah ke directory `/etc/apache2/sites-available` kemudian buka file `000-default.conf` dan tambahkan
  ```
  <Directory /var/www/vitamin.brokoli.d38/public/images>
     Options +Indexes
  </Directory>

  Alias "/img" "/var/www/vitamin.brokoli.d38/public/images"
  ```
  ![image](https://github.com/user-attachments/assets/531da4be-0a7c-472f-a0c6-bf83952e4504)
  Restart apache2 dengan
  ```
  service apache2 restart
  ```
  Akses web dari client
  ```
  lynx vitamin.brokoli.d38.com/img
  ```

<br>

## Soal 15

> Karena terdapat resep rahasia di file /secret/recipe_secret.txt pada subdomain www.vitamin.brokoli.yyy.com, konfigurasikan folder /secret agar tidak dapat diakses oleh pengguna (dengan menampilkan 403 Forbidden).

> _Because there is a secret recipe in the /secret/recipe_secret.txt file on the www.vitamin.brokoli.yyy.com subdomain, configure the /secret folder so that it cannot be accessed by users (by displaying 403 Forbidden)._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/7e231f7c-f148-4750-a564-278466f8964b)

- Explanation

  Pindah ke directory `/etc/apache2/sites-available` kemudian buka file `000-defaul.conf` dan tambahkan
  ```
  <Directory /var/www/vitamin.brokoli.d38/secret>
     Options -Indexes
  </Directory>
  ```
  ![image](https://github.com/user-attachments/assets/76f5fbbc-5062-4239-a5cf-4cd26f4194a8)
  Restart apache dengan perintah
  ```
  service apache2 restart
  ```
  Akses web dari client
  ```
  lynx vitamin.brokoli.d38.com/secret
  ```

<br>

## Soal 16

> Karena dinilai terlalu panjang coba ubah konfigurasi www.vitamin.brokoli.yyy.com/public/js menjadi www.vitamin.brokoli.yyy.com/js

> _Since it is considered too long, change the configuration from www.vitamin.brokoli.yyy.com/public/js to www.vitamin.brokoli.yyy.com/js._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/b2476e27-01ab-405e-8f10-3c00e3ade1b7)

- Explanation
  
  Pindah ke directory `/etc/apache2/sites-available` kemudian buka file `000-default.conf` dan tambahkan
  ```
  <Directory /var/www/vitamin.brokoli.d38/public/js>
     Options +Indexes
  </Directory>

  Alias "/js" "/var/www/vitamin.brokoli.d38/public/js"
  ```
  ![image](https://github.com/user-attachments/assets/59aa2ce4-aa11-4423-8195-1509c707ae45)
  Restart apache2 dengan
  ```
  service apache2 restart
  ```
  Akses web dari client
  ```
  lynx vitamin.brokoli.d38.com/js
  ```

<br>

## Soal 17

> Supaya Web kita aman terkendali maka ubah konfigurasi www.k1.vitamin.brokoli.yyy.com menjadi hanya bisa di akses oleh port 9696 dan 8888

> _To keep our web secure, configure www.k1.vitamin.brokoli.yyy.com to only be accessible through ports 9696 and 8888._

**Answer:**

- Screenshot
  ![image](https://github.com/user-attachments/assets/c30f7839-7209-4d0a-a932-d4a4d3fc958a)
  ![image](https://github.com/user-attachments/assets/2e04c200-4520-4477-b7f9-c24f99ad4cef)

- Explanation

  Buat direktori untuk `k1.vitamin.brokoli.d38` yang memiliki isi yang sama dengan `vitamin.brokoli.d38`
  ```
  cp -r /var/www/vitamin.brokoli.d38 /var/www/k1.vitamin.brokoli.d38
  ```
  Pindah ke directory `/etc/apache2/sites-available` kemudian copy file `000-default.conf`
  ```
  cp 000-default.conf default-8888-9696.conf
  ```
  Ubah isi file `default-8888-9696.conf` menjadi seperti ini
  ![image](https://github.com/user-attachments/assets/50553dff-5a94-4991-8d7a-f744cb74a29f)
  ![image](https://github.com/user-attachments/assets/cc590099-3a0c-4dcc-bc8e-d19be6d2b922)
  Tambahkan virtual host di bawah ini agar `k1.vitamin.brokoli.d38.com` tidak dapat diakses melalui port 80
  ```
  <VirtualHost *:80>
    ServerName k1.vitamin.brokoli.d38.com
    <Location />
        Order deny,allow
        Deny from all
    </Location>
  </VirtualHost>
  ```
  Enable file `default-8888-9696.conf` dengan
  ```
  a2ensite default-8888-9696.conf
  ```
  Kemudian, buka file `/etc/apache2/ports.conf` dan tambahkan portnya seperti yang diminta
  ![image](https://github.com/user-attachments/assets/15d1cd3d-4a2a-464a-ab8f-a35c7588c879)
  Restart apache
  ```
  service apache2 restart
  ```
  Akses web dari client
  ```
  lynx k1.vitamin.brokoli.d38.com:80
  lynx k1.vitamin.brokoli.d38.com:8888
  lynx k1.vitamin.brokoli.d38.com:9696
  ```

<br>

## Soal 18

> Lanjutkan dari nomor sebelumnya buatlah autentikasi dengan username “Seblak” dan password “sehatyyy” dengan yyy adalah kode kelompok. Letakkan Document Root pada /var/www/k1.vitamin.brokoli.yyy.

> _Continuing from the previous point, create authentication with the username “Seblak” and the password “sehatyyy” where yyy is the group code. Set the Document Root to /var/www/k1.vitamin.brokoli.yyy._

**Answer:**

- Screenshot

  ![Screenshot 2024-10-13 002419](https://github.com/user-attachments/assets/5fef0437-9c42-4054-a693-81b65425bffd)

- Explanation
  
  Buat file password dengan username *`Seblak`*
  ```
  htpasswd -c /etc/apache2/.htpasswd Seblak
  ```
  Masukkan *`sehatd38`* sebagai password<br>
  Tambahkan command di bawah pada `/var/www/k1.vitamin.brokoli.d38/.htaccess`
  ```
  AuthType Basic
  AuthName "Restricted Area"
  AuthUserFile /etc/apache2/.htpasswd
  Require valid-user
  ```
  Restart apache
  ```
  service apache2 restart
  ```
  Akses web menggunakan dari client
  ```
  lynx k1.vitamin.brokoli.d38.com:8888
  ```
  Masukkan username *`Seblak`* dan password *`sehatd38`*

<br>

## Soal 19

> Konfigurasikan agar setiap kali IP Brokoli diakses dengan lynx, secara otomatis akan dialihkan ke www.brokoli.yyy.com (alias).

> _Configure it so that every time Brokoli's IP is accessed using lynx, it is automatically redirected to www.brokoli.yyy.com (alias)._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/c298c77c-4d26-4af8-9753-8b61c1bc916a)
  ![image](https://github.com/user-attachments/assets/4bd6f6ad-ac87-46a8-a9bf-ca46a07978b5)

- Explanation

  Tambahkan virtual host baru pada `/etc/apache2/sites-available/000-default.conf`
  ```
  <VirtualHost *:80>
    ServerName 10.183.2.3

    Redirect permanent / http://www.brokoli.d38.com/
  </VirtualHost>
  ```
  Dan tambahkan server alias `www.brokoli.d38.com` pada virtual host yang lama
  Restart apache
  ```
  service apache2 restart
  ```
  Akses web dari client
  ```
  lynx 10.183.2.3
  ```

<br>

## Soal 20

> Karena jumlah pengunjung website www.vitamin.brokoli.yyy.com semakin meningkat dan terdapat banyak gambar random, ubah permintaan gambar yang mengandung substring "vitamin" agar diarahkan ke file vitamin.png.

> _Since the number of visitors to www.vitamin.brokoli.yyy.com is increasing and there are many random images, redirect image requests that contain the substring "vitamin" to the file vitamin.png._

**Answer:**

- Screenshot

  ![image](https://github.com/user-attachments/assets/f56bfdae-5fc3-44eb-a6a5-95553bec1921)

- Explanation

  Tambahkan command di bawah ini pada `/var/www/vitamin.brokoli.d38/.htaccess`
  ```
  RewriteCond %{REQUEST_URI} ^/public/images/.*vitamin.*\.(png|jpg)$
  RewriteCond %{REQUEST_URI} !/public/images/vitamin.png$
  RewriteRule ^ /public/images/vitamin.png [L,R=301]
  ```
  Restart apache
  ```
  service apache2 restart
  ```
  Akses web dari client
  ```
  lynx vitamin.brokoli.d38.com/public/images/vitamintest.png
  ```  
  
<br>
  
## Problems

## Revisions (if any)

Number 17-20
