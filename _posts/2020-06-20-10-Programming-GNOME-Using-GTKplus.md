# Programming GNOME Using GTK+

GTK+ (GIMP Tool Kit ) adalah library untuk membuat antarmuka(GUI) berlisensi GPL ( General Public Lisence ) yang memudahkan untuk membuat free software ataupun commercial software. Dinamakan GIMP Tool Kit karena padaawalnya merupakan pengembangan General Image Manipulation Program (GIMP).Saat ini GTK juga digunakan pada banyak project software  seperti GNU Network Object Model Environment  (GNOME) project

GTK telah menerapkan teknik Object Oriented Programming  (OOP)dan Application Programmers Interface  (API) menggunakan bahasa C danmenggunakan teknik pemrograman classes system  dan callback function. Secaraumum GTK+ adalah sebuah event control  yang artinya GTK akan menunggu padabagian gtk_main() sampai pada peristiwa ( event ) misal klik tombol mouse yangkemudian akan dikendalikan agar melewati fungsi yang telah ditentukan atau disebut signal 

2. Struktur GTK Dasar

GTK merupakan Application Programmers Interface (API) berorientasi obyek yang mudah dan sederhana untuk ditulis serta sederhana untuk dimengerti. Sebagai contoh program berikut:

#### Program GUI dengan GTK+

Apabila kita ingin membuat program GUI atau Desktop, maka kita bisa menggunakan beberapa library untuk membuat GUI.

Salah satu yang sering digunakan di Linux adalah GTK+.

Pastikan library ini sudah terinstal di komputer kita. Karena kalau tidak, kamu akan mendapatkan error seperti ini:

![image-20200619091126684](C:\Users\Naoza\Desktop\UAS\Linux prog\10 Prog dengan GTK+\erorGTK.png)

Ini artinya, library GTK+ belum ada di komputer kita. Maka dari itu, kita harus menginstalnya.

Cara install-nya:

```
sudo apt install libgtk2.0-dev
```

Atau (untuk versi 3):

```
sudo apt install libgtk-3-dev
```

mari kita install dulu:

![image-20200619093121637](C:\Users\Naoza\Desktop\UAS\Linux prog\10 Prog dengan GTK+\instalGTK.png)

Setelah itu, mari kita buat program desktop dengan GTK+.

Buatlah file baru kemudian isi dengan kode berikut:

```c
#include <gtk/gtk.h>

int main( int argc, char *argv[] )
{GtkWidget *window;gtk_init (&argc, &argv);
window = gtk_window_new (GTK_WINDOW_TOPLEVEL);
gtk_widget_show (window);
gtk_main ();

return 0;
}
```

Simpan kode program tersebut dengan nama program1.c 

Lalu coba jalankan dengan perintah:

```
gcc program1.c -o program1 `pkg-config --cflags --libs gtk+-2.0
```

Maka hasilnya:

![img](C:\Users\Naoza\Desktop\UAS\Linux prog\Prog dengan GTK+\gtk_pemula.jpg)

Kerenkan !!!!!

Argumen `--pkg gtk+-3.0` artinya kita akan menggunakan library GTK versi 3 pada program ini.