#  Install MySQL on Linux

### Pengenalan

MySQL adalah sistem manajemen database Open-source, yang umumnya dipasang sebagai bagian dari stack LAMP (Linux, Apache, MySQL, PHP/Python/Perl) yang populer. Menggunakan database relasional dan SQL (Structured Query Language) untuk mengelola datanya.

Versi pendek dari instalasi sederhana: update indeks paket Anda, menginstal paket MySQL-server, dan kemudian menjalankan skrip keamanan disertakan.

```
sudo apt update
sudo apt install mysql-server
sudo mysql_secure_installation
```

Di sini kita akan menjelaskan cara menginstal MySQL versi 5,7 pada Ubuntu 18,04 server. Namun, jika ingin memperbarui instalasi MySQL yang ada ke versi 5,7, Anda dapat membaca panduan pembaruan MySQL 5,7 ini sebagai gantinya.



### Prasyarat

Untuk mengikuti tutorial ini, Anda akan perlu:

- Satu Ubuntu 18,04 server mengatur dengan mengikuti panduan penyiapan server awal ini, termasuk pengguna non-**root** dengan hak `sudo` dan firewall.

##### Langkah 1-menginstal MySQL

Pada Ubuntu 18,04, hanya versi terbaru dari MySQL yang termasuk dalam paket APT repository secara default. Pada saat penulisan, itu MySQL 5,7

Untuk menginstalnya, perbarui indeks paket pada server Anda dengan apt:

```
`sudo apt update`
```

Kemudian instal paket default:

```
sudo apt install mysql-server
```

Ini akan menginstal MySQL, tetapi tidak akan meminta Anda untuk mengatur password atau membuat perubahan konfigurasi lainnya. Karena hal ini membuat instalasi MySQL tidak aman, kami akan membahas ini berikutnya.

##### Langkah 2-mengkonfigurasi MySQL

Untuk instalasi baru, Anda akan ingin menjalankan skrip keamanan yang disertakan. Ini mengubah beberapa opsi default yang kurang aman untuk hal seperti login root jarak jauh dan pengguna sampel. Pada versi lama dari MySQL, Anda perlu untuk menginisialisasi direktori data secara manual juga, tapi ini dilakukan secara otomatis sekarang.

Jalankan skrip keamanan:

```
sudo mysql_secure_installation
```

Ini akan membawa Anda melalui serangkaian prompt di mana Anda dapat membuat beberapa perubahan pada opsi keamanan instalasi MySQL Anda. Prompt pertama akan menanyakan apakah Anda ingin mengatur validasi password plugin, yang dapat digunakan untuk menguji kekuatan password MySQL Anda. Terlepas dari pilihan Anda, prompt berikutnya akan menetapkan password untuk pengguna **root** MySQL. Masukkan dan kemudian mengkonfirmasi password yang aman pilihan Anda.

Dari sana, Anda dapat menekan **Y** dan kemudian **ENTER** untuk menerima default untuk semua pertanyaan berikutnya. Ini akan menghapus beberapa pengguna anonim dan database uji, menonaktifkan login root jarak jauh, dan memuat aturan baru ini sehingga MySQL segera menghormati perubahan yang telah Anda buat.

Untuk menginisialisasi direktori Data MySQL, Anda akan menggunakan **mysql_install_db** untuk versi sebelum 5.7.6, dan **mysqld--Initialize** untuk 5.7.6 dan yang lebih baru. Namun, jika Anda menginstal MySQL dari distribusi Debian, seperti yang dijelaskan pada langkah 1, direktori data diinisialisasi secara otomatis; Anda tidak perlu melakukan apa-apa. Jika Anda mencoba menjalankan perintah Anyway, Anda akan melihat galat berikut ini:

```
mysqld: Can't create directory '/var/lib/mysql/' (Errcode: 17 - File exists)
. . .
2018-04-23T13:48:00.572066Z 0 [ERROR] Aborting
```

*Perhatikan bahwa meskipun Anda telah menetapkan kata sandi untuk pengguna root MySQL, pengguna ini tidak dikonfigurasi untuk mengotentikasi dengan password saat menghubungkan ke shell MySQL. Jika ingin, Anda dapat menyesuaikan pengaturan ini dengan mengikuti langkah 3.*



#### Langkah 3 — (opsional) menyesuaikan autentikasi dan hak istimewa pengguna

Dalam sistem Ubuntu menjalankan MySQL 5,7 (dan versi yang lebih baru), **root** pengguna MySQL diatur untuk mengotentikasi menggunakan plugin `auth_socket` secara default daripada dengan password. Hal ini memungkinkan untuk beberapa keamanan yang lebih besar dan kegunaan dalam banyak kasus, tetapi juga dapat mempersulit sesuatu ketika Anda perlu untuk memungkinkan program eksternal (misalnya, phpMyAdmin) untuk mengakses pengguna.

Dalam rangka untuk menggunakan password untuk terhubung ke MySQL sebagai **root**, Anda akan perlu untuk beralih metode otentikasi dari `auth_socket` ke `mysql_native_password`. Untuk melakukan ini, buka MySQL prompt dari terminal Anda:

```
sudo mysql
```

Selanjutnya, periksa metode autentikasi mana yang digunakan oleh masing-masing akun pengguna MySQL dengan perintah berikut:

```
SELECT user,authentication_string,plugin,host FROM mysql.user;
```

```
Output
+------------------+-------------------------------------------+-----------------------+-----------+
| user             | authentication_string                     | plugin                | host      |
+------------------+-------------------------------------------+-----------------------+-----------+
| root             |                                           | auth_socket           | localhost |
| mysql.session    | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
| mysql.sys        | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
| debian-sys-maint | *CC744277A401A7D25BE1CA89AFF17BF607F876FF | mysql_native_password | localhost |
+------------------+-------------------------------------------+-----------------------+-----------+
4 rows in set (0.00 sec)
```

Dalam contoh ini, Anda dapat melihat bahwa pengguna **root** sebenarnya mengotentikasi menggunakan plugin `auth_socket`. Untuk mengkonfigurasi akun **root** untuk mengotentikasi dengan sandi, jalankan perintah `ALTER USER` berikut ini. Pastikan untuk mengubah password untuk password yang kuat yang Anda pilih, dan perhatikan bahwa perintah ini akan mengubah password root yang Anda tetapkan pada langkah 2:

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

Kemudian, jalankan FLUSH PRIVILEGES yang memberitahu server untuk memuat ulang tabel hibah dan menempatkan perubahan baru Anda berlaku:

```
FLUSH PRIVILEGES;
```

Periksa metode otentikasi yang digunakan oleh setiap pengguna Anda lagi untuk mengkonfirmasi bahwa akar tidak lagi mengotentikasi menggunakan plugin `auth_socket`:

```
ELECT user,authentication_string,plugin,host FROM mysql.user;
```

output

```
+------------------+-------------------------------------------+-----------------------+-----------+
| user             | authentication_string                     | plugin                | host      |
+------------------+-------------------------------------------+-----------------------+-----------+
| root             | *3636DACC8616D997782ADD0839F92C1571D6D78F | mysql_native_password | localhost |
| mysql.session    | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
| mysql.sys        | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
| debian-sys-maint | *CC744277A401A7D25BE1CA89AFF17BF607F876FF | mysql_native_password | localhost |
+------------------+-------------------------------------------+-----------------------+-----------+
4 rows in set (0.00 sec)
```

Anda dapat melihat dalam contoh ini output bahwa akar MySQL pengguna sekarang mengotentikasi menggunakan password. Setelah Anda mengkonfirmasi ini pada server Anda sendiri, Anda dapat keluar dari shell MySQL:

```
exit
```

Atau, beberapa mungkin menemukan bahwa lebih baik sesuai dengan alur kerja mereka untuk terhubung ke MySQL dengan pengguna khusus. Untuk membuat pengguna seperti itu, membuka shell MySQL sekali lagi:

```
sudo mysql
```

*Catatan: jika Anda memiliki otentikasi sandi diaktifkan untuk akar, seperti yang dijelaskan dalam paragraf sebelumnya, Anda akan perlu menggunakan perintah yang berbeda untuk mengakses shell MySQL. Berikut ini akan menjalankan klien MySQL Anda dengan hak istimewa pengguna biasa, dan Anda hanya akan mendapatkan hak administrator dalam database dengan mengotentikasi:*  `mysql -u root -p`

Dari sana, buat pengguna baru dan berikan kata sandi yang kuat:

```
CREATE USER 'sammy'@'localhost' IDENTIFIED BY 'password
```

Kemudian, berikan hak istimewa yang sesuai kepada pengguna baru Anda. Sebagai contoh, Anda dapat memberikan hak pengguna untuk semua tabel dalam database, serta kekuatan untuk menambah, mengubah, dan menghapus hak pengguna, dengan perintah ini:

```
GRANT ALL PRIVILEGES ON *.* TO 'sammy'@'localhost' WITH GRANT OPTION;
```

Perhatikan bahwa, pada titik ini, Anda tidak perlu menjalankan perintah `FLUSH PRIVILEGES` lagi. Perintah ini hanya diperlukan bila Anda mengubah tabel hibah menggunakan pernyataan seperti `INSERT, UPDATE, atau DELETE`. Karena Anda membuat pengguna baru, bukannya memodifikasi yang sudah ada, `FLUSH PRIVILEGES` tidak diperlukan di sini.

Setelah ini, keluar dari shell MySQL:

```
exit
```

Akhirnya, mari kita uji instalasi MySQL.



#### Langkah 4-Pengujian MySQL

Terlepas dari bagaimana Anda memasangnya, MySQL seharusnya mulai berjalan secara otomatis. Untuk menguji ini, periksa statusnya.

```
systemctl status mysql.service
```

Anda akan melihat output yang mirip dengan berikut ini:

```
 mysql.service - MySQL Community Server
   Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: en
   Active: active (running) since Wed 2018-04-23 21:21:25 UTC; 30min ago
 Main PID: 3754 (mysqld)
    Tasks: 28
   Memory: 142.3M
      CPU: 1.994s
   CGroup: /system.slice/mysql.service
           └─3754 /usr/sbin/mysqld
```

Jika MySQL tidak berjalan, Anda dapat memulainya dengan `sudo systemctl mulai MySQL.`

Untuk pemeriksaan tambahan, Anda dapat mencoba menghubungkan ke database menggunakan alat mysqladmin, yang merupakan klien yang memungkinkan Anda menjalankan perintah administratif. Sebagai contoh, perintah ini mengatakan untuk menghubungkan ke MySQL sebagai root *(-u root*), meminta password (-p), dan mengembalikan versi.

```
sudo mysqladmin -p -u root version
```

Anda akan melihat output yang mirip dengan ini:

```
mysqladmin  Ver 8.42 Distrib 5.7.21, for Linux on x86_64
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Server version      5.7.21-1ubuntu1
Protocol version    10
Connection      Localhost via UNIX socket
UNIX socket     /var/run/mysqld/mysqld.sock
Uptime:         30 min 54 sec

Threads: 1  Questions: 12  Slow queries: 0  Opens: 115  Flush tables: 1  Open tables: 34  Queries per second avg: 0.006
```

Ini berarti MySQL sudah bangun dan berjalan.



#### Kesimpulan

Anda sekarang memiliki setup MySQL dasar yang diinstal pada server Anda.