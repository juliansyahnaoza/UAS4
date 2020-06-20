# 5. Processes and Signals

Untuk mengatur kegiatan proses mereka perlu saling berkomunikasi. Linux mendukung berbagai mekanisme komunikasi antar proses (KAP). Sinyal dan pemipaan merupakan dua di antaranya, tapi linux juga mendukung system sistem ke lima mekanisme KAP.



### Sinyal

inyal merupakan salah satu metode KAP tertua sistem Unix. Sinyal digunakan untuk memberitahukan kejadian yang asinkronus pada satu atau lebih proses. misalnya sinyal yang dihasilkan oleh keyboard saat ditekan oleh pemakai. Sinyal juga dapat dihasilkan oleh kondisi yang menyatakan error, misalnya saat suatu proses mencoba mengakses lokasi yang tidak pernah ada di dalam memori utama. Sinyal pun digunakan oleh shells untuk memberitahu perintah kontrol kerja kepada proses child.

### Beberapa Sinyal di Linux

Ada satu perangkat sinyal terdefinisi yang bisa digenerate oleh kernel atau oleh proses lain di dalam sistem, tentunya setelah proses tersebut punya hak untuk melakukannya. Anda dapat melihat daftar dari seperangkat sinyal sistem dengan menggunakan perintah `kill (kill -l)`. Di dalam box Linux Intel, perintah kill tersebut menghasilkan keluaran sebagai berikut:

1) SIGHUP 2) SIGINT 3) SIGQUIT 4) SIGILL

5) SIGTRAP 6) SIGIOT 7) SIGBUS 8) SIGFPE

9) SIGKILL 10) SIGUSR1 11) SIGSEGV 12) SIGUSR2

13) SIGPIPE 14) SIGALRM 15) SIGTERM 17) SIGCHLD

18) SIGCONT 19) SIGSTOP 20) SIGTSTP 21) SIGTTIN

22) SIGTTOU 23) SIGURG 24) SIGXCPU 25) SIGXFSZ

26) SIGVTALRM 27) SIGPROF 28) SIGWINCH 29) SIGIO

30) SIGPWR

### Bagaimana Suatu Sinyal Disikapi

Proses dapat memilih untuk mengabaikan kebanyakan sinyal yang digenerate dengan dua pengecualian: baik sinyal SIGSTOP, yang menyebabkan suatu proses menghentikan pekerjaannya, mau pun sinyal SIGKILL, yang menyebabkan suatu proses berhenti, tidak dapat diabaikan. Selain itu, suatu proses dapat memilih bagaimana cara ia mengatasi bermacam-macam sinyal. Proses dapat menghalangi sinyal tersebut dan, bila tidak menghalanginya, proses itu dapat memilih antara mengatasinya sendiri atau membiarkan kernel mengatasinya. Bila kernel mengatasi sinyal tersebut maka sejumlah tindakan default akan dilakukan untuk mengatasi sinyal ini. Misalnya, tindakan default saat sebuah proses menerima sinyal SIGPE (exception floating point) adalah dengan core dump kemudian keluar. Sinyal tidak punya prioritas-prioritas yang terkait. Bila dua sinyal dihasilkan untuk suatu proses pada waktu yang sama, maka keduanya dapat diberikan ke proses tersebut atau ditangani dengan urutan tertentu. Selain itu, tidak ada mekanisme untuk mengatasi sinyal yang sama dan banyak sekaligus. Tidak ada cara bahwa suatu proses dapat memberitahukan apakah ia menerima 1 atau 42 sinyal SIGCONT

### Penerapan Sinyal

Linux menerapkan sinyal dengan menggunakan informasi yang disimpan dalam task_struct untuk proses tersebut. Jumlah sinyal yang didukung terbatas pada ukuran word prosesornya. Proses dengan ukuran word 32 bit dapat memiliki 32 sinyal sementara prosesor 64 bit seperti Alpha AXP dapat memiliki sampai 64 sinyal. Sinyal-sinyal yang tertunda saat ini disimpan dalam field sinyal dengan sebuah mask dari sinyal-sinyal terblokir yang disimpan di blocked. Dengan pengecualian SIGTOP dan SIGKILL, semua sinyal dapat diblokir. Bila sinyal yang diblokir digenerate, maka sinyal itu akan tetap tertahan sampai ia tidak diblokir lagi.

Linux juga menyimpan informasi tentang bagaimana setiap proses menangani sinyal-sinyal yang mungkin terjadi. Informasi ini disimpan dalam suatu array stuktur data sigaction yang ditunjuk oleh task_struct untuk setiap proses. Di antara hal-hal yang lain, informasi ini mengandung baik alamat routin yang nantinya menangani sinyal atau flag, yang memberitahu Linux bahwa proses tersebut ingin mengabaikan sinyal ini atau membiarkan kernel menanganinya. Proses tersebut memodifikasi penanganan default sinyal dengan membuat system call ,dan call ini mengubah sigaction untuk sinyal yang sesuai dan juga mask daripada blocked.

Tidak semua proses di dalam sistem dapat mengirimkan sinyal ke proses lainnya. Kernel dapat melakukannya demikian pula super users. Proses-proses biasa hanya dapat mengirim sinyal pada proses-proses yang memiliki uid dan gid yang sama atau pun pada kelompok proses yang sama. Sinyal digenerate dengan mengatur bit yang sesuai di dalam field signal task_struct. Jika proses tersebut belum memblokir sinyal dan sedang menunggu (namun dapat diinterrupt di status Interruptible), maka ia akan dibangunkan dengan mengubah statusnya ke Running dan memastikan bahwa proses ini berada pada antrian run. Dengan cara itu scheduler akan menganggapnya sebagai suatu yang akan running pada jadwal sistem berikutnya. Jika penanganan default diperlukan, maka Linux dapat mengoptimalkan penganganan sinyal tersebut. Sebagai contoh, jika sinyal SIGWINCH (fokus yang berubah dari jendela X) dan penangan default sedang digunakan, maka tidak ada yang perlu dilakukan.

Sinyal-sinyal tidak diberikan ke proses segera saat mereka digenerate. Sinyal-sinyal ini harus menunggu sampai proses tersebut berjalan kembali. Setiap kali sebuah proses keluar dari suatu system calls, field signals dan blocked dicek dan bila ada sinyal-sinyal apa pun yang tidak terblokir, sekarang sinyal-sinyal ini dapat disampaikan. Kelihatannya cara ini bukanlah cara yang dapat diandalkan, namun setiap proses di dalam sistem pasti membuat system calls, sebagai contoh, untuk menulis suatu karakter ke terminal sepanjang waktu. Proses dapat memilih untuk menunggu sinyal bila ia mau, kemudian dapat disuspend di status Interruptible sampai sinyal itu datang. Kode pemrosesan sinyal Linux melihat pada struktur sigaction untuk setiap sinyal yang saat ini belum diblokir.

Jika sebuah penangan sinyal diset ke tindakan default, maka kernel akan mengatasinya. Penangan default sinyal SIGSTOP akan mengubah status proses saat ini ke status Stopped dan selanjutnya menjalankan scheduler untuk memilih sebuah proses baru untuk berjalan. Tindakan default untuk sinyal SIGFPE akan core dump proses dan menyebabkannya keluar. Cara lainnya, proses tersebut dapat menentukan handler sinyalnya sendiri. Penangan ini merupakan suatu routine yang akan dipanggil kapan pun sinyal digenerate dan struktur sigactionnya menyimpan alamat routine ini. Kernel tersebut harus memanggil routine penangan sinyal proses tersebut dan bagaimana ini terjadi adalah kekhususan masing-masing prosesor tetapi intinya semua CPU harus berhasil mengatasi kenyataan bahwa proses saat ini sedang berjalan di mode kernel dan mengembalikan proses yang tadi memanggil kernel atau system routine di mode user. Masalah ini terpecahkan dengan memanipulasi stack dan register daripada proses tersebut. Program counter dari proses diset ke alamat sinyalnya, yang menangani routine, dan parameter-parameter ke routine dimasukkan ke frame callnya atau dilewatkan di register. Ketika proses tersebut menerima operasi, proses ini terlihat seolah-olah routine penangan sinyalnya dipanggil secara normal.

Linux bersifat POSIX compatible dan oleh karena itu prosesnya dapat menentukan sinyal-sinyal mana yang diblokir saat routine tertentu penangan sinyal dipanggil. Hal ini berarti mengubah mask blocked tersebut selama pemanggilan penangan sinyal proses-proses tersebut. mask blocked harus dikembalikan ke nilai aslinya ketika routine penangan routine tersebut telah selesai. Oleh karena itu Linux menambahkan suatu call pada sebuah routine perapih yang akan mengembalikan mask asli daripada blocked ke dalam stack call dari proses yang disinyal. Linux juga mengoptimalkan kasus di mana beberapa routine penangan sinyal perlu dipanggil dengan stacking routine-routine ini sehingga setiap saat sebuah routine penangan ada, routine penangan berikutnya dipanggil sampai routine perapih dipanggil.

