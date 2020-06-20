## Data Management

Kita akan melihat pada awalnya cara mengelola alokasi sumber daya Anda, kemudian berurusan dengan file yang diakses oleh banyak pengguna lebih atau kurang secara simultan dan terakhir pada satu alat yang disediakan di sebagian besar UNIX sistem untuk mengatasi keterbatasan file data.

Topik ini sebagai tiga cara untuk mengelola data:

· Manajemen memori dinamis: apa yang harus dilakukan dan apa UNIX tidak akan membiarkan Anda lakukan.
· Penguncian file: penguncian kooperatif, wilayah penguncian file bersama dan menghindari  kemogokan.
· Database dBm: pustaka database ditampilkan dalam UNIX.

##### **Managing Memory**

1. **Alokasi memori sederhana**
   Kami mengalokasikan memori menggunakan malloc panggilan di Perpustakaan C standar.

   `#include <stdlib.h>`
   `void *malloc(size_t size)`;

   buatlah file baru dengan nama `memory1.c` dengan mengetikan `vim memory1.c`diterminal linux anda, 

   ![1592149772034]({{site.baseurl}}/assets/images/vim-memory1c.png)

   kemudian ketikan source code dibawah ini: 

   **codelabs**

   `#include <unistd.h>
   #include <stdlib.h>
   #include <stdio.h>
   #define A_MEGABYTE (1024 * 1024)
   int main()
   {
   char *some_memory;
   int megabyte = A_MEGABYTE;
   int exit_code = EXIT_FAILURE;
   some_memory = (char *)malloc(megabyte);
   if (some_memory != NULL) {
   sprintf(some_memory, "Hello World\n");
   printf("%s", some_memory);
   exit_code = EXIT_SUCCESS;
   }
   exit(exit_code);
   }
   `

   ![1592149997563](C:\Users\Naoza\Desktop\UAS\memory1c.png)

   Ketika kita akan menjalankan file memory1.c harus terlebih dalu di compile, dengan perintah `gcc memory1.c -o memory1.c`:

   `$ ./memory1` cara menjalankannya
   `Hello World`

   # ![1592150237091](C:\Users\Naoza\Desktop\UAS\hasil-memory1.png)

##### **Mengunci File (File Lock)**

Penguncian file adalah bagian yang sangat penting dari Multiuser, sistem operasi multitasking. Program sering membutuhkan untuk berbagi data, biasanya melalui file, dan sangat penting bahwa program tersebut memiliki beberapa cara untuk membangun kontrol file. File kemudian dapat diperbarui dalam mode aman, atau program kedua dapat berhenti sendiri dari mencoba untuk membaca file yang dalam keadaan sementara sementara program lain menulis untuk itu.

1. Membuat File Lock

   **codelabs**

   `#include <unistd.h>
   #include <stdlib.h>
   #include <stdio.h>
   #include <fcntl.h>
   #include <errno.h>
   int main()
   {
   int file_desc;
   int save_errno;
   file_desc = open("/tmp/LCK.test", O_RDWR | O_CREAT | O_EXCL, 0444);
   if (file_desc == −1) {
   save_errno = errno;
   printf("Open failed with error %d\n", save_errno);
   }
   else {
   printf("Open succeeded\n");
   }
   exit(EXIT_SUCCESS);
   }`

   







