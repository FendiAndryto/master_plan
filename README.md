# Dokumentasi Praktikum Master Plan

## Hasil Akhir Praktikum

Berikut adalah hasil akhir dari praktikum dalam bentuk GIF:
![GIF Hasil Praktikum](./assets/master_plan_demo.gif)

## Penjelasan Praktikum

Praktikum ini bertujuan untuk membuat aplikasi perencanaan tugas menggunakan Flutter.
Aplikasi ini memungkinkan pengguna untuk menambahkan tugas, menandai tugas sebagai selesai, dan menggulir daftar tugas.

### **Langkah 4: Penjelasan**

Langkah 4 bertujuan untuk membuat struktur proyek agar lebih terorganisir dengan memisahkan tampilan (UI) dari logika data. Dengan menggunakan folder **models**, kita dapat mengelola struktur data secara terpisah dari tampilan UI.

### **Langkah 6: Mengapa Perlu Variabel `plan`?**

Variabel `plan` digunakan untuk menyimpan data perencanaan yang mencakup daftar tugas. Dibuat sebagai konstanta (`const Plan()`) untuk memastikan bahwa nilai awalnya tetap dan tidak dapat diubah langsung, sehingga setiap perubahan harus dilakukan dengan membuat objek baru.

### **Langkah 9: Hasil dan Penjelasan**

Langkah ini menambahkan tampilan **ListTile** untuk setiap tugas dalam daftar perencanaan. Berikut adalah GIF hasil implementasi:
![GIF Langkah 9](./assets/praktikum1.gif)

Pada langkah ini, setiap tugas ditampilkan dalam bentuk **TextFormField** dan dapat ditandai dengan checkbox.

### **Kegunaan Method pada Langkah 11 dan 13 dalam Lifecycle State**

- **Langkah 11 (`initState`)**
  - Method ini digunakan untuk menginisialisasi `ScrollController`.
  - Menambahkan listener untuk menghapus fokus dari semua input teks ketika pengguna menggulir daftar.
- **Langkah 13 (`dispose`)**
  - Method ini digunakan untuk membersihkan `ScrollController` saat widget dihapus dari tree.
  - Mencegah kebocoran memori dengan memastikan controller tidak terus berjalan setelah widget tidak lagi digunakan.

## Penjelasan Praktikum 2

### 1. **Penjelasan tentang `InheritedWidget` dan Penggunaan `InheritedNotifier` pada Langkah 1**

Pada Langkah 1, kita diminta untuk membuat file `plan_provider.dart` yang berisi class `PlanProvider`. Dalam class ini, kita menggunakan konsep **InheritedWidget**.

- **`InheritedWidget`** adalah widget di Flutter yang memungkinkan data untuk diteruskan ke subtree widget yang lebih dalam dalam widget tree. Widget ini digunakan agar child widgets dapat mengakses data yang ada di widget yang lebih atas dalam tree tanpa perlu meneruskan data tersebut secara eksplisit melalui constructor. Biasanya digunakan untuk keadaan global yang perlu dibagikan di banyak tempat dalam aplikasi (seperti tema, pengaturan, dll).

Namun, pada contoh ini, kita menggunakan **`InheritedNotifier<ValueNotifier<T>>`** sebagai pengganti **`InheritedWidget`**.

- **`InheritedNotifier`** adalah subclass dari `InheritedWidget`, tetapi dengan tambahan kemampuan untuk bekerja dengan objek `ValueNotifier`. **`ValueNotifier`** adalah objek yang dapat memberikan pemberitahuan (notify) kepada listener-nya jika nilainya berubah. Oleh karena itu, dengan menggunakan **`InheritedNotifier<ValueNotifier<Plan>>`**, kita bisa memberikan pemberitahuan perubahan pada data `Plan` dan memastikan bahwa widget-widget yang bergantung pada data ini dapat merespons perubahan tersebut secara otomatis.

**Mengapa menggunakan `InheritedNotifier`?**  
Karena **`InheritedNotifier`** memungkinkan kita untuk menghubungkan data yang bersifat mutable (dalam hal ini adalah data `Plan` yang dapat berubah) dengan widget yang ingin mendengarkan perubahan data tersebut. Jadi, menggunakan `InheritedNotifier<ValueNotifier<Plan>>` memungkinkan kita untuk memanfaatkan fungsi `ValueNotifier` dalam pengelolaan status aplikasi yang dinamis.

---

### 2. **Penjelasan tentang Method pada Langkah 3 (Metode `completedCount` dan `completenessMessage`)**

Pada Langkah 3, kita diminta untuk menambahkan dua method dalam class `Plan` di file `plan.dart`.

**Penjelasan**:

- **`completedCount`** adalah method yang menghitung jumlah task yang telah selesai.

  - **`tasks`** adalah daftar task yang ada dalam `Plan`.
  - **`where((task) => task.complete)`** adalah fungsi filter yang mencari task-task yang memiliki properti `complete` bernilai `true`, yang menandakan bahwa task tersebut telah selesai.
  - **`length`** kemudian mengembalikan jumlah task yang telah selesai.

- **`completenessMessage`** adalah method yang menghasilkan pesan yang menunjukkan seberapa banyak task yang telah diselesaikan dari total task yang ada.
  - Method ini memanggil `completedCount` untuk mendapatkan jumlah task yang selesai dan kemudian menghasilkan string seperti: `"3 out of 5 tasks"`, yang memberikan gambaran tentang progres penyelesaian task.

**Mengapa dilakukan demikian?**
Penting untuk memberikan informasi visual yang menggambarkan seberapa banyak progress yang telah tercapai, terutama dalam aplikasi pengelolaan tugas seperti ini. Dengan menambahkan `completedCount` dan `completenessMessage`, kita dapat memberikan feedback langsung kepada pengguna tentang status tugas yang telah mereka kerjakan.

---

### 3. **Capture GIF Hasil dari Langkah 9 dan Penjelasan**

**Capture GIF**:
![GIF Langkah 9](./assets/praktikum2.gif)

Penjelasan mengenai hasil dari Langkah 9:

- Pada Langkah 9, kita melakukan wrapping pada widget `_buildList` dengan widget **`Expanded`** dan menambahkan widget **`SafeArea`** di bagian bawah untuk menampilkan **`completenessMessage`**. Hal ini bertujuan agar tampilan progres tugas tetap terlihat dengan jelas di bagian bawah layar tanpa terganggu oleh elemen UI lainnya.
- **`Expanded`**: Widget ini memastikan bahwa daftar tugas akan memanfaatkan ruang yang tersedia di layar sesuai dengan ukuran yang ada, dan memastikan bahwa footer yang berisi pesan progres tetap berada di bagian bawah layar.
- **`SafeArea`**: Widget ini digunakan untuk memastikan bahwa widget yang berada di dalamnya tidak tertutup oleh area layar yang tidak dapat digunakan seperti status bar atau navigasi sistem. Jadi, teks yang menampilkan progres (`completenessMessage`) akan selalu terlihat dengan baik meskipun pada perangkat dengan notch atau area lain yang terhalang.

**Apa yang telah dibuat?**

- Kami telah membuat sebuah aplikasi pengelolaan tugas dengan state management menggunakan `InheritedNotifier` untuk menyediakan data `Plan` kepada widget-widget turunannya.
- Dengan menggunakan `ValueListenableBuilder`, aplikasi akan merespons perubahan pada data dan memperbarui tampilan secara otomatis.
- Kami juga telah menambahkan fitur untuk menghitung progress dari task yang diselesaikan dan menampilkannya di bagian bawah layar.
