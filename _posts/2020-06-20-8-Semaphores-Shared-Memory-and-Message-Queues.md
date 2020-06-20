



# Semaphores, Shared Memory, and Message Queues

### **1. Semaphore**

Sebuah variabel yang dilindungi atau tipe data abstrak yang merupakan metode klasik untuk membatasi akses ke sumber daya bersama seperti shared memory dalam suatu lingkungan pemrograman paralel. Semaphore adalah solusi klasik untuk mencegah race condition pada dining philosopher problem, meskipun mereka tidak mencegah deadlock sumber daya.

### **2. Shared Memory**: 

memori yang dapat diakses secara bersamaan oleh beberapa program dengan maksud untuk menyediakan komunikasi di antara mereka atau menghindari salinan yang berlebihan. Tergantung pada konteks, program dapat berjalan pada satu prosesor atau beberapa prosesor yang terpisah.

Merupakan salah satu cara komunikasi antar proses dengan cara mengalokasikan suatu alamat memori untuk dipakai berkomunikasi antar proses. Alamat dan besar alokasi memori yang digunakan biasanya ditentukan oleh pembuat program. Pada metode ini, sistem akan mengatur proses mana yang akan memakai memori pada waktu tertentu sehingga pekerjaan dapat dilakukan secara efektif.



###  **3. Message Queue**: 

merupakan metode dimana proses (atau program contoh) dapat bertukar data menggunakan atau melalui sebuah interface untuk sistem yang dikelola message queue. Sebuah pesan antrian dapat dibuat oleh satu proses dan digunakan oleh banyak proses yang membaca dan / atau menulis pesan ke antrian. Misalnya, server proses dapat membaca dan menulis pesan dari dan ke pesan antrian dibuat untuk klien proses. Jenis pesan dapat digunakan untuk menghubungkan pesan dengan klien tertentu proses meskipun semua pesan pada antrian yang sama.

