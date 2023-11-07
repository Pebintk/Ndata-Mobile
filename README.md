# ndata_mobile

A PBP Flutter project.

# Tugas 7: Elemen Dasar Flutter
### Membuat sebuah program Flutter baru 
1. Buka _command prompt_ dan pindah ke folder tempat menyimpan aplikasi, kemudian buat proyek flutter baru dengan menjalankan perintah `flutter create ndata_mobile` dan masuk ke folder itu dengan `cd ndata_mobile`

2. Buka folder `ndata_mobile/lib` dan tambahkan file `menu.dart`.

3. Pada line awal `menu.dart`, tambahkan `import 'package:flutter/material.dart';` dan pindahkan kedua _class_ berikut yang dari file `main.dart` ke dalam file `menu.dart`
``` dart
class MyHomePage ... {
    ...
}

class _MyHomePageState ... {
    ...
}
```

4. Pada file `main.dart`, tambahkan 1 kode baris yang akan meng-import function di `menu.dart`, yaitu `import 'package:ndata_mobile/menu.dart';` agar tidak terjadi error

### Membuat tiga tombol sederhana dengan ikon dan teks dan memunculkan Snackbar ketika ditekan tombolnya
1. Pada file `main.dart`, hapus MyHomePage(title: 'Flutter Demo Home Page') menjadi `home: MyHomePage()`

2. Lakukan perubahan pada `menu.dart` di `class MyHomePage()` dari stateful menjadi stateless, dan ubah kodenya menjadi seperti ini.
```dart
class MyHomePage extends StatelessWidget {
    MyHomePage({Key? key}) : super(key: key);

    @override
    Widget build(BuildContext context) {
        return Scaffold(
            ...
        );
    }
}
```
> [!NOTE]
> Jangan lupa untuk hapus fungsi State yang ada dibawah bagian stateless widget ini.


3. Tambahkan _class_ berikut untuk atribut di aplikasi kita
```dart
class InventoryItem {
  final String name;    //Untuk define nama fitur
  final IconData icon;  //Untuk define icon 
  final Color color;    //Untuk define warna tombol masing-masing
  InventoryItem(this.name, this.icon, this.color);
}
```

4. Di file `menu.dart` pada `class MyHomePage extends StatelessWidget`, di bawah kode `MyHomePage({Key? key}) : super(key: key);`, tambahkan kode berikut untuk mendefinisikan setiap tombol yang ditambahkan
```dart
...
final List<InventoryItem> items = [
    ShopItem("Lihat Item", Icons.checklist,  Color(0xFF02A8A8)),
    ShopItem("Tambah Item", Icons.add_shopping_cart, Color(0xFFF5DEA3)),
    ShopItem("Logout", Icons.logout, Color(0xFFa50808)),
  ];
...
```

5. Dalam _class_ yang sama, ganti _function_ `Widget build` menjadi berikut
```dart
...
@override
  Widget build(BuildContext context) {
      return Scaffold(
      appBar: AppBar(
        backgroundColor : Color(0xFF02A8A8),
        title: const Text(
          'Ndata',
          style: TextStyle(color: Colors.white),
        ),
      ),
      body: SingleChildScrollView(
        // Widget wrapper yang dapat discroll
        child: Padding(
          padding: const EdgeInsets.all(10.0), // Set padding dari halaman
          child: Column(
            // Widget untuk menampilkan children secara vertikal
            children: <Widget>[
              const Padding(
                padding: EdgeInsets.only(top: 10.0, bottom: 10.0),
                // Widget Text untuk menampilkan tulisan dengan alignment center dan style yang sesuai
                child: Text(
                  'Welcome to Ndata', // Text yang menandakan toko
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    fontSize: 30,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
              // Grid layout
              GridView.count(
                // Container pada card kita.
                primary: true,
                padding: const EdgeInsets.all(20),
                crossAxisSpacing: 10,
                mainAxisSpacing: 10,
                crossAxisCount: 3,
                shrinkWrap: true,
                children: items.map((ShopItem item) {
                  // Iterasi untuk setiap item
                  return ShopCard(item);
                }).toList(),
              ),
            ],
          ),
        ),
      ),
    );
  }
...
```

6. Tambahkan _function_ baru untuk mendefinisikan tombol/card yang kita tambahkan
```dart
class InventoryCard extends StatelessWidget {
  final InventoryItem item;

  const InventoryCard(this.item, {super.key}); // Constructor

  @override
 Widget build(BuildContext context) {
    return Material(
      color: item.color,
      child: InkWell(
        // Area responsive terhadap sentuhan
        onTap: () {
          // Memunculkan SnackBar ketika diklik
          ScaffoldMessenger.of(context)
            ..hideCurrentSnackBar()
            ..showSnackBar(SnackBar(
                content: Text("Kamu telah menekan tombol ${item.name}!")));
        },
        child: Container(
          // Container untuk menyimpan Icon dan Text
          padding: const EdgeInsets.all(8),
          child: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(
                  item.icon,
                  color: Colors.white,
                  size: 30.0,
                ),
                const Padding(padding: EdgeInsets.all(3)),
                Text(
                  item.name,
                  textAlign: TextAlign.center,
                  style: const TextStyle(color: Colors.white),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```
## Perbedaan utama antara stateless dan stateful widget dalam konteks pengembangan aplikasi Flutter
Dalam pengembangan aplikasi Flutter, perbedaan utama antara stateless dan stateful widget terletak pada bagaimana mereka mengelola keadaan (state) dari widget tersebut. Berikut adalah perbedaan utama antara keduanya:
1. Stateless Widget:
    - Stateless widget adalah widget yang tidak memiliki state internal. Ini berarti bahwa setelah widget dibangun, properti atau tampilan widget tersebut tidak akan berubah.
    - Stateless widget berguna untuk bagian tampilan yang tidak perlu berubah atau tidak memerlukan pembaruan berdasarkan perubahan data. Contoh penggunaan stateless widget termasuk teks statis, ikon, gambar, dan elemen UI lainnya yang tidak memerlukan perubahan dinamis.
    - Stateless widget lebih efisien karena mereka tidak memerlukan pembaruan tampilan setelah dibangun.
2. Stateful Widget:
    - Stateful widget adalah widget yang memiliki state internal yang dapat berubah selama siklus hidup widget. Mereka dapat diperbarui, direbuild, dan merender tampilan sesuai dengan perubahan keadaan internal.
    - Stateful Widget digunakan ketika Anda perlu mengikuti perubahan data atau interaksi pengguna yang memengaruhi tampilan widget. Contohnya adalah formulir input, daftar item yang dapat diubah, atau tampilan yang perlu diperbarui secara dinamis.
    - Stateful widget memiliki kelas yang terkait dengan mereka yang disebut "State". Kelas "State" ini memungkinkan Anda untuk menyimpan dan memperbarui keadaan widget.

Intinya, perbedaan utama dari keduanya adalah stateless widget bersifat statis dan tidak dapat berubah, sementara stateful widget memiliki state internal yang dapat berubah dan memungkinkan respons terhadap perubahan. 

## Seluruh widget yang digunakan dan fungsinya masing-masing.
1. `MyHomePage`:
   - `Scaffold`: Ini menyediakan kerangka dasar aplikasi Material, yang meliputi struktur dan layout untuk banyak komponen UI seperti AppBar, Body, dan FloatingActionButton
   - `AppBar`: Ini memberikan area di bagian atas aplikasi yang biasanya menampilkan judul, tombol aksi, atau tombol navigasi
   - `SingleChildScrollView`: Digunakan untuk membuat konten yang bisa discroll ketika konten tersebut lebih besar dari ruang layar yang tersedia
   - `Padding`: Ini menambahkan ruang kosong atau jarak di sekitar child widgetnya, yaitu memberikan padding pada konten
   - `Column`: Digunakan untuk mengatur elemen-elemen anak secara vertikal.
2. `ShopCard`:
   - `Material`: Ini adalah widget yang mengaplikasikan efek Material Design dan memberikan latar belakang dengan warna (sesuai dengan warna yang diberikan pada item ShopItem).
   - `InkWell`: Ini adalah widget yang membuat area responsif terhadap sentuhan (misalnya, ketika di-klik). Ketika di-tap, ia menampilkan Snackbar.
   - `Container`: Menyediakan kotak untuk menempatkan child widget dengan kemampuan untuk memberikan styling, seperti padding dan margin
   - `Icon`: Menampilkan ikon dari set yang diberikan oleh Flutter, seperti yang ditemukan di Icons class
   - `Text`: Widget ini menampilkan teks yang sesuai dengan nama item dari `ShopItem`.
3. `MyApp`:
   - `MaterialApp`: Widget yang membungkus sejumlah widget yang memungkinkan pengembangan aplikasi material design (widget root). Ini termasuk pengaturan judul aplikasi, tema (theme), dan halaman awal (home).
   - `title`: Digunakan untuk memberi judul aplikasi.
   - `theme`: Ini adalah tema (theme) aplikasi dan mengatur tampilan keseluruhan aplikasi. Anda mengatur colorScheme untuk mengatur warna tema dan mengaktifkan useMaterial3 untuk menggunakan Material Design 3.0.
   - `home`: Ini adalah halaman utama aplikasi, dalam hal ini, Anda mengatur `MyHomePage()` sebagai halaman awal.
