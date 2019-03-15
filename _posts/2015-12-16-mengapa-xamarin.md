---
title: Mengapa Xamarin?
updated: 2015-12-16 10:00
comments: true
---

Judul diatas merupakan pertanyaan yang terlontar dari dosen pembimbing saya sewaktu saya melakukan bimbingan skripsi. Jawaban saya sederhana, karena xamarin itu cross-platform dan kebetulan bahasa yang digunakan C#. Lalu beliau bertanya lagi.

> Apa bagusnya Xamarin ini ?

Dengan nada diplomatis bak tim marketing dari Xamarin HQ, saya menjawab bahwa dengan xamarin kita bisa mendapatkan performa native tanpa mengorbankan code-sharing terlebih di xamarin.forms yang nyaris bisa sampai **90%**!

Saya sedikit trauma dengan performa phone-gap di masa lalu yang lemotnya gak ketulungan di ponsel android jadul saya. Mungkin waktu itu belum banyak pilihan, namun sekarang sepertinya xamarin sudah cukup menjanjikan.

## Judul yang Panjang

Waktu itu saya menggunakan xamarin.forms untuk skripsi saya yang berjudul *rancang bangun aplikasi perangkat bergerak pemantau prakiraan cuaca maritim*. Panjang ? Memang, saya kadang juga lupa sama judul sendiri. wkwk.

Beruntung sekali Xamarin mempunyai program student, dimana kita bisa mendapatkan lisensi cuma-cuma yang harga normalnya lumayan mahal untuk kantong orang Indonesia.

Sedikit membahas teknikal, bayangin aja bero kalau kita kudu koding tiap platform, apa kagak gempor itu. Di Xamarin.Forms, cukup dengan kode dibawah kita bisa dapatkan tampilan fungsional yang sama di ketiga sistem operasi (iOS, Android, dan Windows Phone).

Cukup beberapa potong kode seperti dibawah:

{% highlight csharp %}
using Xamarin.Forms;

var profilePage = new ContentPage {
    Title = "Profile",
    Icon = "Profile.png",
    Content = new StackLayout {
        Spacing = 20, Padding = 50,
        VerticalOptions = LayoutOptions.Center,
        Children = {
            new Entry { Placeholder = "Username" },
            new Entry { Placeholder = "Password", IsPassword = true },
            new Button {
                Text = "Login",
                TextColor = Color.White,
                BackgroundColor = Color.FromHex("77D065") }}}
};

var settingsPage = new ContentPage {
    Title = "Settings",
    Icon = "Settings.png",
    (...)
};

var mainPage = new TabbedPage { Children = { profilePage, settingsPage } };
{% endhighlight %}

... dan selanjutnya anda bisa mempunyai tampilan fungsional yang sama di ketiga sistem operasi seperti dibawah.

![gambar xamarin app](https://raw.githubusercontent.com/putuyoga/putuyoga.github.io/c43acbca9998c277ed113bb9aadc4864e67486c7/assets/images/example_xamarin.jpg)


## Sederhana ?
Gak juga, karena terkadang untuk app yang kompleks dan butuh fitur-fitur kompleks, xamarin menurut saya sendiri kurang cocok. 

**Sekarang gimana menurutmu sendiri?**
