# 7. Inter-Process Communication: Pipes



### Komunikasi Antar Proses

Untuk mengatur kegiatan proses mereka perlu saling berkomunikasi. Linux mendukung berbagai mekanisme komunikasi antar proses (KAP). Sinyal dan pemipaan merupakan dua di antaranya, tapi linux juga mendukung system sistem ke lima mekanisme KAP.



### Pipa

Semua shells Linux yang biasa, membolehkan redirection. Sebagai contoh:

```
$ ls | pr | lpr
```

memipakan output dari perintah ls, yang melist file yang ada di dalam direktori, sampai standar input dari perintah pr yang mempaginasi file tersebut. Pada akhirnya standard output dari perintah pr dipipakan ke standar input dari perintah lpr yang mencetak hasil-hasilnya ke printer default. Pipa-pipa berikutnya adalah unidirectional byte streams yang menghubungkan standard output dari suatu proses ke standar input dari proses lainnya. Proses tidak peduli terhadap redirection ini dan berperilaku seolah-olah ia berjalan normal saja. Adalah shell yang membangun pipa-pipa yang bersifat sementara ini di antara dua proses tersebut.

![pipa07](C:\Users\Naoza\Desktop\UAS\Linux prog\Pipa\pipa07.png)

*Gambar 7-7. Pipa.*



### Penerapan

Di Linux, suatu pipa diterapkan dengan menggunakan dua struktur data file yang keduanya menunjuk ke inode VFS sementara yang sama yang ia sendiri menunjuk pada halaman fisik di dalam memori. Gambar di atas menunjukkan bahwa setiap struktur data file mengandung pointer ke vektor-vektor routine operasi file yang berbeda; satu untuk menulis ke pipa, satu lagi untuk membaca dari pipa.

Hal tersebut menyembunyikan perbedaan-perbedaan yang mendasar dari system calls umum yang membaca dan menulis file biasa. Saat proses menulis tersebut menulis ke pipa, byte-byte dikopi ke halaman data bersama dan ketika proses membaca membaca dari pipa, byte-byte dikopi dari halaman data bersama. Linux harus mensinkronisasikan akses ke pipa tersebut. Linux harus memastikan bahwa pembaca dan penulis pipa berada pada jalur dan untuk melakukannya Linux menggukan kunci, antrian wait dan sinyal.

### Cara Menulis Data

Saat penulis ingin menulis ke pipa, ia menggunakan fungsi-fungsi pustaka penulisan yang standar. Semuanya ini melewatkan pendeskripsi file yang diindeks ke perangkat proses dari sturktur data file, masing-masing merepresentasikan file yang sedang terbuka atau pun, dalam kasus ini, pipa yang terbuka. routine penulis itu menggunakan informasi yang ada di dalam inode VFS yang merepresentasikan pipa untui mengatur permintaan menulis.

Bila ada cukup ruangan untuk menulis semua bytes kedalam pipa dan, sepanjang pipa tidak dikunci oleh pembacanya, Linux menguncinya untuk si penulis dan mengkopikan bytes tersebut dari ruang alamat proses itu ke halaman data bersama. Bila pipa itu dikunci oleh pembaca atau bila tidak ada cukup ruang bagi data maka proses sekarang disuruh tidur di antrian tunggu inode pipa itu dan scheduller dipanggil sehingga proses lainnya dapat berjalan. Proses yang tidur ini interruptible, sehingga ia masih dapat menerima sinyal dan dapat dibangunkan oleh pembaca ketika ruangan telah cukup untuk ditulisi data atau pun ketika pipa sudah tidak dikunci. Setelah data ditulis, inode VFS dari pipa dibuka kuncinya dan semua pembaca yang menunggu di antrian tunggu inode akan dibangunkan oleh mereka sendiri.

### Cara Membaca Data

Membaca data dari pipa sangat mirip dengan menulis.

Proses boleh membaca dengan tidak melakukan pemblokiran (tergantung pada mode di mana proses tersebut membuka file atau pipa) dan, dalam kasus ini, bila tidak ada data untuk dibaca atau bila pipa dikunci, pesan kesalahan akan dikembalikan. Artinya, proses tersebut dapat terus berjalan. Cara lainnya adalah dengan menunggu di antrian tunggu inode pipa sampai proses menulis sudah selesai. Saat kedua proses sudah selesai berurusan dengan pipa, inode pipa tersebut dibuang bersama halaman data bersama.







