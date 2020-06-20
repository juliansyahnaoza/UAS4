# Development Tools

Ada beberapa alat yang tersedia untuk mengembangkan program pada sistem UNIX. Selain kebutuhan yang jelas dari kompiler dan debugger, UNIX menyediakan seperangkat alat, masing-masing yang melakukan pekerjaan tunggal, dan kemudian memungkinkan pengembang untuk menggabungkan alat ini dalam cara-cara baru dan inovatif.

### Makefiles

Makefiles adalah cara sederhana untuk mengatur kompilasi kode. Tutorial ini bahkan tidak menggores permukaan apa yang mungkin menggunakan make, namun dimaksudkan sebagai panduan permulaan sehingga Anda dapat dengan cepat dan mudah membuat Makefile Anda sendiri untuk proyek kecil hingga menengah.

Mari kita mulai dengan tiga file berikut, hellomake. c, hellofunc. c, dan hellomake. h, yang akan mewakili sebuah program utama yang khas, beberapa kode fungsional dalam file terpisah, dan file include, masing-masing.

1. **hellomake.c**

   ```c
   #include <hellomake.h>
   
   int main() {
     // call a function in another file
     myPrintHelloMake();
   
     return(0);
   }
   ```

   2. **hellofunc.c**

      ```c
      #include <stdio.h>
      #include <hellomake.h>
      
      void myPrintHelloMake(void) {
      
        printf("Hello makefiles!\n");
      
        return;
      }
      ```

      3. **hellomake.h**

         ```h
         /*
         example include file
         */
         
         void myPrintHelloMake(void);
         ```

Biasanya, Anda akan mengkompilasi koleksi kode ini dengan mengeksekusi perintah berikut:

`gcc -o hellomake hellomake.c hellofunc.c -I`

Ini mengkompilasi dua. c file dan nama hellomake executable. The-I. disertakan agar GCC akan terlihat di direktori saat ini (.) untuk file include hellomake. h. Tanpa Makefile, pendekatan khas untuk tes/memodifikasi/debug siklus adalah dengan menggunakan panah atas di terminal untuk kembali ke perintah compile terakhir Anda sehingga Anda tidak perlu mengetik setiap kali, terutama setelah Anda telah menambahkan beberapa file. c ke dalam campuran.

Sayangnya, pendekatan ini untuk kompilasi memiliki dua downfalls. Pertama, jika Anda kehilangan perintah kompilasi atau beralih komputer Anda harus mengetik ulang dari awal, yang tidak efisien di terbaik. Kedua, jika Anda hanya membuat perubahan pada satu file. c, mengkompilasi ulang semuanya setiap waktu juga memakan waktu dan tidak efisien. Jadi, saatnya untuk melihat apa yang bisa kita lakukan dengan Makefile.

Makefile paling sederhana yang bisa Anda buat akan terlihat seperti:

```
hellomake: hellomake.c hellofunc.c
     gcc -o hellomake hellomake.c hellofunc.c -I.
```

![image-20200618193401412](C:\Users\Naoza\Desktop\UAS\Linux prog\Dev tools\makefile1.png)









Buat yang penasaran lebih dalam soal command `make` dan `Makefile` dapat bereferensi ke sini: https://www.gnu.org/software/make/manual/make.html (**mamam tuh dokumentasi 😈**)





### Source Code Control

![VCS](C:\Users\Naoza\Desktop\UAS\Linux prog\Dev tools\VCS.jpg)

*

*Version Control System* (VCS) adalah sebuah infrastruktur yang dapat mendukung pengembangan *software* secara kolaboratif. Setiap anggota yang berada di dalam sebuah tim pengembangan *software* dapat menulis kode programnya masing - masing kemudian digabungkan ke *server* yang sudah memiliki VCS yang digunakan.

Selain mengandalkan konkurensi yang dapat mempercepat pengembangan *software*, VCS juga mempunyai kemampuan untuk kembali ke versi *software* sebelumnya jika terjadi suatu bencana terhadap versi *software* yang sedang dikembangkan saat ini. Kemampuan ini disebut *reversibility*. Selain itu, dengan menggunakan VCS setiap perubahan pada *software* seperti penambahan *file* atau pengubahan isi *file* dapat dipantau bagian mana yang diubah dan siapa yang mengubah. Sehingga pengerjaan *software* akan lebih transparan dan terukur.

berikut daftar VCS yang perlu dikenal oleh *programmer* seperti Anda:

#### 1. RCS

Revision Control System atau RCS mampu mengelola revisi dari banyak *file*. RCS mengotomasi penyimpanan, pengambilan, pencatatan, pemeriksaan, dan penggabungan dari sebuah revisi. RCS berguna untuk teks yang sering direvisi seperti *source code*, *program*, dokumentasi, grafik, jurnal, dan surat.

RCS pertama kali dikembangkan oleh Walter F. Tichy di Purdue University pada awal 1980. RCS didesain dengan lebih unggul dibandingkan pendahulunya yaitu Source Code Control System (SCCS). Peningkatan RCS dibandingkan SCCS diantaranya adalah antar muka yang lebih mudah dan pengambilan yang lebih cepat dari penyimpanan. RCS menggunakan GNU Diffutils untuk melihat perbedaan diantara dua versi yang berbeda. Saat ini RCS berada di versi 5.9.2

Buat yang penasaran lebih dalam soal RCS bisa berreferensi ke sini  [Dokumentasi RCS](https://www.gnu.org/software/rcs/manual/rcs.html)

#### 2. CVS

CVS adalah *free VCS* yang digunakan oleh mayoritas proyek *free software*. Saat ini CVS perlahan mulai tergantikan oleh sistem VCS lain yang lebih baru. CVS mendukung pengembangan konkuren oleh banyak pengembang baik secara lokal atau diatas jaringan. CVS sangat kurang mendukung untuk *atomic commits* dan pemindahan / penamaan ulang *file*.

Dengan menggunakan VS Anda dapat merekam riwayat dari *file* dan *dokumen*. CVS mendukung *unreserved checkouts* yaitu lebih dari satu pengembang dapat mengerjakan *file* yang sama secara bersamaan. CVS Server dapat berjalan di berbagai varian Unix dan CVS Client dapat berjalan di Windows dan varian Unix. Terkadang CVS dapat melakukan *server mode* di sisi *client*. Saat ini CVS berada di versi 1.11.23

JIka anda tertarik untuk mempelajarinya lebih lanjut bisa ke [Dokumentasi CVS](http://ximbiot.com/cvs/manual/cvs-1.11.23/cvs_7.html)

#### 3. SCCS

CSSC merupakan proyek dari GNU Project yang menggantikan SCCS. SCCS adalah VCS yang bersifat *proprietary* yang tersedia untuk versi komersial dari Unix. GNU CSSC ini dikembangkan sebagai versi *free* untuk Unix. Dengan menggunakan CSSC, sebuah proyek dapat dikontrol menggunakan sistem lain seperti Git atau SVN. Saat ini GNU CSSC berada di versi 1.3.0

Tertarik mempelajari lebih lanjut silahkan ke [Dokumentasi GNU CSSC](https://www.gnu.org/software/cssc/manual/prs-usage.html#prs-usage)

#### 4. Git

Git merupakan VCS yang dikembangkan oleh Linux Torvald ketika mengembangkan Linux (kernel). Git merupakan *decentralized version control system*. Git mempunyai keunggulan seperti *repository syncing*, bekerja secara *offline*, *cheap local branching*, *staging area* yang nyaman, mampu menangani proyek besar seperti Kernel Linux secara efektif dalam hal kecepatan dan ukuran data, mendukung *non-linear development*, dan *multiple workflow*. Selain itu Git digunakan di berbagai layanan VCS seperti Github, Bitbucket, Assembla, dan Gitorious

















*Siapa yang tidak mengenal Git? Version control paling populer, dan tempat berkumpulnya para developer*

