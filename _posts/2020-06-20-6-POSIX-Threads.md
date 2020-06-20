# POSIX Threads

Thread merupakan sebuah alur kontrol proses yang dapat dijadwalkan pengeksekusiannya oleh sistem operasi, bayangkan sebuah program main mempunyai berbagai prosedur (fungsi) dan fungsi-fungsi tersebut dapat dijalankan secara serentak dan atau bebas dijalankan oleh sistem operasi. Dan itulah yang disebut sebagai multithread.

Sebuah proses diciptakan oleh sistem operasi dan membutuhkan cukup banyak “overhead”. Proses terdiri dari informasi tentang sumber daya program dan posisi eksekusi proses :

- Process ID, process group ID, user ID, and group ID
- Environment
- Working directory.
- Program instructions
- Registers
- Stack
- Heap
- File descriptors
- Signal actions
- Shared libraries
- Inter-process communication tools (such as message queues, pipes, semaphores, or shared memory)

Sebuah thread digunakan dan berada pada proses dan dapat secara bebas dieksekusi oleh sistem informasi.

Kesimpulannya thread dalam UNIX :

- Ada didalam proses dan menggunakan sumber daya.

- Memiliki aliran kontrol sendiri selama proses induknya ada dan didukung OS.

- Hanya menduplikat sumber daya penting untuk dapat berjalan sendiri.

- Dapat berbagi sumber daya dengan thread lain yang sama-sama berjalan secara independen.

- Akan mati jika proses induknya mati.

  

**Thread Management**

Membuat thread : **pthread_create (thread,attr,start_routine,arg)** 

Perintah diatas digunakan untuk membuat thread dan membuatnya dapat dieksekusi. Routine ini dapat dipanggil berkali-kali di dalam kode anda.

Argumen pada pthread_create :

**thread :** merupakan pengenal dari sebuah thread yang dibuat.

**attr :** merupakan atribut objek yang mungkin digunakan untuk mengeset atribut thread. Anda bisa mengesetnya atau NULL untuk nilai default.

**start_routine :** merupakan fungsi pada C yang digunakan sebagai perintah untuk thread.

**arg :** merupakan nilai tunggal yang akan di passingkan ke start_routine. passing yang ditentukan menggunakan passing by reference sebagai pinter pada tipe void. Gunakan NULL untuk nilai default.

Attribut thread :

Secara default sebuah thread mempunyai attribut. Beberapa atribut dapat dirubah oleh programmer menggunakan thread object atribut. **pthread_attr_init(attr)** dan **pthread_attr_destroy(attr)** digunakan untuk mengeset dan menghancurkan atribut pada thread.

Menghentikan thread : **pthread_exit()**

Suatu thread dapat berhenti melalui beberapa cara diantaranya :

– thread berhenti dengan normal saat pekerjaannya telah selesai.

– thread dibatalkan dengan routine **pthread_cancel.**

– semua proses diberhentikan dengan fungsi system exec atau exit ().

– berakhirnya perintah pada main().

– pemberhentian melalui pemanggilan **pthread_exit()**, baik pekerjaannya telah selesai atau belum.

#### **Codelabs**

1. Buka terminal anda, kemudiaan buatlah file baru dengan berekstensi .c dengan perintah `vim pthread2.c` , karena menggunkan bahasa c.

   ![](C:\Users\Naoza\Desktop\UAS\Linux prog\Posix\vim pthread2.png)

2. ketikan sourcode berikut ini:

   ```c
   #include <pthread.h>
    #include <stdio.h>
    #define NUM_THREADS     5
   
    void *PrintHello(void *threadid)
    {
       long tid;
       tid = (long)threadid;
       printf("Hello World! It's me, thread #%ld!\n", tid);
       pthread_exit(NULL);
    }
   
    int main (int argc, char *argv[])
    {
       pthread_t threads[NUM_THREADS];
       int rc;
       long t;
       for(t=0; t<NUM_THREADS; t++){
          printf("In main: creating thread %ld\n", t);
          rc = pthread_create(&threads[t], NULL, PrintHello, (void *)t);
          if (rc){
             printf("ERROR; return code from pthread_create() is %d\n", rc);
             exit(-1);
          }
       }
   
       /* Last thing that main() should do */
       pthread_exit(NULL);
    }
   ```

   seperti screenshot dibawah ini:

   <img src="C:\Users\Naoza\Desktop\UAS\Linux prog\Posix\code_pt.png" alt="image-20200618075559920"  />

3. Kemudian setelah menulis code dan menyimpannya, maka untuk melihat file telah disimpan gunakan perintah `ls` 

   ![image-20200618080126325](C:\Users\Naoza\Desktop\UAS\Linux prog\Posix\ls_file.png)

4. kemudian Compile:
   - C compiler: `cc -lpthread pthread1.c`

![image-20200618080533136](C:\Users\Naoza\Desktop\UAS\Linux prog\Posix\hasil.png)



Itulah pembahasan tentang POSIX Threads bagian perkenalan dan membuat *Thread Management*, masih ada lagi banyak pembahasan mengenai POSIX Threads, mulai *Thread Synchronization, Thread Scheduling, Thread Pitfalls, Thread Debugging, Thread Man Pages,* dan selanjutnya. 