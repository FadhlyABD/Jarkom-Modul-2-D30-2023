# Jarkom-Modul-2-D30-2023

## Group Member    :
| Nama                              | NRP        |
|-----------------------------------|------------|
|Abdullah Yasykur Bifadhlil Midror  |5025211035  |
|Muhammad Ahyun Irsyada             |5025211251  |

Berikut adalah demo untuk praktikum modul 2

## Nomor 1

Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi sesuai dengan yang telah ditentukan (Topologi No.2 dalam drive soal). 

## Nomor 2

Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

## Nomor 3

Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

## Nomor 4

Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

## Nomor 5

Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

## Nomor 6

Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

**Penyelesaian**
- Untuk membuat DNS Slave di werkudara pertama-tama kita tambahkan script berikut di dalam `bashrc` di dalam root werkudara : ( ss 1)
- Script tersebut berguna untuk melakukan update dan menginstall Bind9
- Langkah selanjutnya adalah membuat `domain.sh` di root atau home,lalu tambahkan script berikut di dalamnya : (ss 2)
- Script tersebut berguna untuk membuat konfigurasi DNS yang ada di werkudara menjadi DNS slave,  menambahkan konfigurasi zona DNS `arjuna.d30.com` dan `abimanyu.d30.com` sebagai zona slave dengan server master yang memiliki alamat IP "192.206.2.2" ke berkas konfigurasi DNS /etc/bind/named.conf.local. Dengan demikian, server DNS akan mulai mengambil data dari server master tersebut untuk zona `arjuna.d30.com` dan `abimanyu.d30.com`.Lalu script tersebut di masukkan di dalam `/etc/bind/named.conf.local`
- Langkah selanjutnya adalah Menambahkan script berikut pada `domain.sh` di zona `arjuna.d30.com` dan `abimanyu.d30.com`. di dalam `Yudistira`  
  ``also-notify { 192.206.3.2; }; // Masukan IP Werkudara tanpa tanda petik
    allow-transfer { 192.206.3.2; }; // Masukan IP Werkudara tanpa tanda petik``
  (ss 3)
- script tersebut berfungsi untuk Menunjukkan bahwa server BIND juga akan memberitahu (notify) server DNS lain dengan alamat IP "192.206.3.2" ketika ada perubahan dalam zona `arjuna.d30.com` dan `abimanyu.d30.com`. Ini berguna untuk menjaga konsistensi data. script juga tersebut berfungsi untuk Menunjukkan bahwa server dengan alamat IP "192.206.3.2" diperbolehkan untuk mengambil data dari zona `arjuna.d30.com` dan `abimanyu.d30.com`. Ini mengatur izin untuk transfer zona.
  
## Nomor 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

**Penyelesaian**
langkah untuk membuat subdomain baratayuda.abimanyu.d30.com dengan alias www.baratayuda.abimanyu.d30.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda. adalah sebagai berikut : 
- Pertama tama kita tambahkan dulu script berikut di dalam `domain.sh` di dalam `werkudara` (ss 4)
- Script tersebut berfungsi untuk membuat konfigurasi domain server `baratayuda.abimanyu.d30.com`
- Lalu tambahkan script berikut di dalam `domain.sh` di dalam `werkudara` (ss 5)
- Script `allow-query { any; };:` Mengatur bahwa server DNS akan mengizinkan permintaan DNS dari semua sumber (any).Berarti siapa pun dapat mengirim permintaan DNS ke server.
`auth-nxdomain no;:`  Mengatur perilaku server DNS ketika menemui kasus domain yang tidak ditemukan (NXDOMAIN). Dalam hal ini, opsi diatur ke "no", yang berarti server tidak akan mengirimkan jawaban yang tepat mengikuti RFC1035. Ini bisa bermanfaat dalam beberapa situasi tertentu, tetapi perlu diterapkan dengan hati-hati.
`listen-on-v6 { any; };:` Mengatur server DNS agar mendengarkan permintaan DNS di alamat IPv6. Dalam hal ini, digunakan "any" untuk mendengarkan di semua alamat IPv6 yang tersedia.
Dan memasukkan script tersebut di `/etc/bind/named.conf.options`
- Lalu buat direktori baru dengan nama `delegasi` di dalam `/etc/bind` (ss5)
- Lalu copy `/etc/bind/db.local` ke `/etc/bind/delegasi/baratayuda.abimanyu.d30.com` (ss6)
- Lalu tambah kan script berikut di `domain.sh` di dalam `Yudistira` : (ss7)
- Script `Ns1 IN A 192.206.3.2:` adalah catatan A yang menetapkan alamat IP untuk domain "Ns1.abimanyu.d30.com" ke "192.206.3.2".
`baratayuda IN NS Ns1:` adalah catatan NS yang menunjukkan bahwa "baratayuda.abimanyu.d30.com" adalah zona yang diotorisasi oleh server "Ns1.abimanyu.d30.com".
- Lalu tambahkan script berikut di `domain.sh` di dalam `Werkudara` : (ss8)
- Script `@ IN SOA baratayuda.abimanyu.d30.com. root.baratayuda.abimanyu.d30.com. ( ... ): `Ini adalah catatan SOA (Start of Authority), yang menentukan informasi tentang zona seperti nama server otoritas (baratayuda.abimanyu.d30.com.), alamat email administrator (root.baratayuda.abimanyu.d30.com.), dan parameter lainnya seperti serial number, refresh interval, retry interval, dan expire interval.
`@ IN NS baratayuda.abimanyu.d30.com.: `Ini menunjukkan bahwa server otoritas untuk zona ini adalah "baratayuda.abimanyu.d30.com."
`@ IN A 192.206.3.2:` Ini adalah catatan A yang menetapkan alamat IP untuk domain "baratayuda.abimanyu.d30.com" ke "192.206.3.2".
`www IN CNAME baratayuda.abimanyu.d30.com.:` Ini adalah catatan CNAME yang menunjukkan bahwa alamat "www.baratayuda.abimanyu.d30.com" akan diarahkan (alias) ke "baratayuda.abimanyu.d30.com."
`rjp IN A 192.206.3.4:` Ini adalah catatan A yang menetapkan alamat IP untuk domain "rjp.baratayuda.abimanyu.d30.com" ke "192.206.3.4".
lalu memasukkan nya ke `/etc/bind/delegasi/baratayuda.abimanyu.d30.com`

## Nomor 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

**Penyelesaian** 
- Untuk membuat  subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu. Pertama tama tambahkan script berikut di dalam `/etc/bind/delegasi/baratayuda.abimanyu.d30.com` di atas script `www     IN      CNAME   baratayuda.abimanyu.d30.com.` (ss9)
- Script `rjp IN A 192.206.3.4:` Ini adalah catatan A yang menetapkan alamat IP untuk domain "rjp.baratayuda.abimanyu.d30.com" ke "192.206.3.4".

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
