# Jarkom-Modul-2-D30-2023

## Group Member    :
| Nama                              | NRP        |
|-----------------------------------|------------|
|Abdullah Yasykur Bifadhlil Midror  |5025211035  |
|Muhammad Ahyun Irsyada             |5025211251  |

Berikut adalah demo untuk praktikum modul 2

## Nomor 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi sesuai dengan yang telah ditentukan (Topologi No.2 dalam drive soal). 

**Penyelesaian**
- Tambahkan 1 node NAT, 1 node ubuntu untuk Router bernama Pandudewanta, 2 node Switch, dan 6 node ubuntu yang masing-masing dinamai sesuai dengan soal diatas.
- Tambahkan 2 node ubuntu lagi yang merupakan client dan masing-masing diberi nama Nakula dan Sadewa, buat juga hubungan antar node nya.
- Pada masing-masing node ubuntu, setting network dengan konfigurasi sebagai berikut `(Prefix IP: 192.206)`:
  1. RouterPandudewanta
  ```
    auto eth0
    iface eth0 inet dhcp
    
    auto eth1
    iface eth1 inet static
    	address 192.206.1.1
    	netmask 255.255.255.0
    
    auto eth2
    iface eth2 inet static
    	address 192.206.2.1
    	netmask 255.255.255.0
    
    auto eth3
    iface eth3 inet static
    	address 192.206.3.1
    	netmask 255.255.255.0
  ```
  2. SadewaClient
  ```
  auto eth0
  iface eth0 inet static
  	address 192.206.1.2
  	netmask 255.255.255.0
  	gateway 192.206.1.1
  ```
  3. NakulaClient
  ```
  auto eth0
  iface eth0 inet static
  	address 192.206.1.3
  	netmask 255.255.255.0
  	gateway 192.206.1.1
  ```
  4. YudhistiraDNSMaster
  ```
  auto eth0
  iface eth0 inet static
  	address 192.206.2.2
  	netmask 255.255.255.0
  	gateway 192.206.2.1
  ```
  5. WerkudaraDNSSlave
  ```
  auto eth0
  iface eth0 inet static
  	address 192.206.3.2
  	netmask 255.255.255.0
  	gateway 192.206.3.1
  ```
  6. ArjunaLoadBalancer
  ```
  auto eth0
  iface eth0 inet static
  	address 192.206.3.3
  	netmask 255.255.255.0
  	gateway 192.206.3.1
  ```
  7. AbimanyuWebServer
  ```
  auto eth0
  iface eth0 inet static
  	address 192.206.3.4
  	netmask 255.255.255.0
  	gateway 192.206.3.1
  ```
  9. PrabukusumaWebServer
  ```
  auto eth0
  iface eth0 inet static
  	address 192.206.3.5
  	netmask 255.255.255.0
  	gateway 192.206.3.1
  ```
  10. WisanggeniWebServer
  ```
  auto eth0
  iface eth0 inet static
  	address 192.206.3.6
  	netmask 255.255.255.0
  	gateway 192.206.3.1
  ```
- Sehingga topologi dapat terlihat seperti pada gambar berikut.
  
| <p align="center"> Topologi </p> |
| -------------------------------------------- |
| <img src="https://github.com/FadhlyABD/Jarkom-Modul-2-D30-2023/blob/main/Images/soal-1.png" width = "400"/> |

## Nomor 2
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

**Penyelesaian**
- Untuk menerapkan tuntunan di modul dalam membuat domain, namun juga agar tidak perlu mengulang-ulang set up, maka kita perlu membuat script.
- Dalam menyimpan script, disini dibagi dalam dua tempat, yaitu pada `/root/.bashrc` dan script buatan `domain.sh`.
- `/root/.bashrc` digunakan untuk menyimpan script yang memang harus dijalankan saat booting (saat node di start) seperti script instalasi.
- Sedangkan `domain.sh` digunakan untuk script yang sifatnya opsional seperti membuat akses domain dan seterusnya.
- Untuk membuat domain `arjuna.d30.com`, buka web console pada node `YudhistiraDNSMaster` dan buat file `domain.sh` pada home (`~`).
- Isikan script senagaimana berikut.

| <p align="center"> domain.sh pada Yudhistira </p> |
| -------------------------------------------- |
| <img src="https://github.com/FadhlyABD/Jarkom-Modul-2-D30-2023/blob/main/Images/soal-2a.png" width = "400"/> |

| <p align="center"> domain.sh pada Yudhistira </p> |
| -------------------------------------------- |
| <img src="https://github.com/FadhlyABD/Jarkom-Modul-2-D30-2023/blob/main/Images/soal-2b.png" width = "400"/> |

## Nomor 3

Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

## Nomor 4

Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

## Nomor 5

Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

## Nomor 6

Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

## Nomor 7

Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

## Nomor 8

Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

## Nomor 9

Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

## Nomor 10

Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

## Nomor 11

Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

## Nomor 12

Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

## Nomor 13

Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

## Nomor 14

Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

## Nomor 15

Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

## Nomor 16

Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

## Nomor 17

Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

## Nomor 18

Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

## Nomor 19

Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

## Nomor 20

Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

| <p align="center"> FTP Request untuk mengunggah </p> |
| -------------------------------------------- |
| <img src="https://github.com/FadhlyABD/Jarkom-Modul-1-D30-2023/blob/main/Images/soal1a.jpg" width = "400"/> |


| <p align="center">  </p> |
| -------------------------------------------- |
| <img src="https://github.com/FadhlyABD/Jarkom-Modul-1-D30-2023/blob/main/Images/no 10 (4).png" width = "400"/> |\

**Kendala** 
masih harus nyari TCP stream satu persatu 
