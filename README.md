# Lapak Online Komunitas Lokal (LOKA)

## Deskripsi Singkat

LOKA merupakan sebuah platform marketplace digital yang didedikasikan secara eksklusif untuk pemberdayaan Usaha Mikro, Kecil, dan Menengah (UMKM) lokal di wilayah Kota Samarinda. Platform ini dirancang untuk mengatasi keterbatasan jangkauan pemasaran dan mengelola operasional bisnis secara terstruktur melalui sistem digital. Dengan fitur dasbor penjual, pencarian katalog produk, integrasi pembayaran digital, hingga kalkulasi ongkos kirim berdasarkan jarak, LOKA memfasilitasi pertemuan antara penjual dan pembeli secara efisien. Pengembangan sistem ini sejalan dengan komitmen terhadap Sustainable Development Goals (SDGs) poin 8 (Pekerjaan Layak & Pertumbuhan Ekonomi), 9 (Industri, Inovasi, & Infrastruktur), dan 12 (Konsumsi & Produksi yang Bertanggung Jawab).

## Fitur Utama Program

Berikut adalah fitur–fitur utama dalam sistem LOKA:

1. Autentikasi & Manajemen Hak Akses
Sistem ini mengimplementasikan keamanan berlapis dengan membagi pengguna ke dalam 3 entitas (aktor) utama yang memiliki hak akses (privileges) berbeda:

* Pengelola (Admin & Verifikator): Bertindak sebagai pengawas ekosistem. Verifikator bertugas memverifikasi pendaftaran toko UMKM baru dan menyetujui/menolak izin jual dari sebuah produk. Admin layanan dapat melihat analitik laporan pendapatan (total bayar) seluruh transaksi berdasarkan periode waktu tertentu.

* Penjual (UMKM): Memiliki dasbor khusus untuk mendaftarkan nama toko & nomor izin usaha, menambahkan produk ke katalog, memperbarui ketersediaan stok, dan menangani pesanan masuk (mengubah status menjadi "Dikirim", "Diproses", "Selesai", "Pengembalian", dan "Dikembalikan").

* Pembeli: Memiliki akses penuh ke etalase produk, dapat menambahkan barang ke keranjang, melakukan pengisian saldo dompet digital (Top-Up), dan melacak status barang yang dibeli.

2. Sistem Transaksi Cerdas & Kalkulasi Logistik
Proses pemesanan barang (Checkout) tidak dilakukan secara manual, melainkan diotomatisasi penuh oleh logika sistem:

* Kalkulasi Ongkos Kirim Dinamis: Sistem otomatis mengalkulasi biaya kirim berdasarkan input jarak (km). Tarif yang ditetapkan adalah Rp ... per kilometer, dengan batas tarif ongkos kirim minimal sebesar Rp ....

* Validasi End-to-End: Sebelum transaksi dicatat, sistem akan mengecek: 

  * Apakah stok produk mencukupi jumlah yang ingin dibeli? 

  * Apakah saldo LOKA-Pay pembeli cukup untuk membayar total belanja + ongkos kirim?

* Sinkronisasi Otomatis: Apabila validasi berhasil, sistem akan langsung membuat data Pesanan dan DetailPesanan, lalu secara real-time memotong saldo pembeli dan mengurangi ketersediaan stok produk di database penjual.

3. Manajemen Data Interaktif Terpusat (CRUD System)
Sistem memberikan keleluasaan bagi pengguna untuk memanipulasi data operasional (Create, Read, Update, Delete) sesuai porsinya:

* Manajemen Katalog (Penjual): UMKM dapat mendaftarkan produk baru (Create), melihat daftar produk mereka (Read), mengubah harga/deskripsi/gambar/stok produk (Update), serta menghapus produk yang sudah tidak dijual (Delete).

* Manajemen Keranjang (Pembeli): Pembeli dapat menimbun barang di dalam keranjang, menelusuri isi keranjang, dan menghapus barang dari keranjang jika tidak jadi membeli. Setelah proses checkout dari keranjang berhasil, sistem otomatis membersihkan (Delete) produk tersebut dari keranjang pembeli.

4. Layanan Pasca-Pembelian (Pengembalian & Ulasan)
Untuk menjaga kepercayaan dan transparansi ekosistem jual-beli lokal:

* Sistem Pengembalian: Jika barang rusak atau tidak sesuai, pembeli dapat mengajukan form retur yang berisi alasan komplain dan nominal dana yang diminta. Status pengembalian awalnya adalah "Diproses", kemudian penjual memiliki otoritas untuk memperbarui statusnya menjadi "Pengembalian Disetuju" atau "Pengembalian Ditolak".

* Rating & Ulasan: Setelah pesanan selesai, pembeli dapat memberikan umpan balik publik berupa teks komentar dan Rating (dengan skala divalidasi antara 1.0 hingga 5.0 bintang) untuk membangun reputasi toko UMKM terkait.

5. Komunikasi & Pembayaran Internal

* Pesan Pribadi: Tersedia modul pertukaran pesan yang memungkinkan pembeli melakukan konsultasi produk secara langsung ke penjual. Sistem mencatat waktu pengiriman dan melacak status pesan (Sudah Dibaca / Belum Dibaca).

* Top-Up LOKA-Pay: Sistem dompet digital internal di mana pembeli dapat mengisi saldo (diinput dan divalidasi tidak boleh minus) yang nantinya menjadi satu-satunya metode pembayaran sah dan terintegrasi di dalam aplikasi LOKA.

## Tech Stack
| KATEGORI | TEKNOLOGI |
|----------|-----------|
| Bahasa Pemrograman | Java |
| Build Automation Tool | Apache Maven |
| Framework GUI | Java Swing |
| Database | MySQL |
| Library/Driver |  |
| Arsitektur Sistem |  |
|Tools Pengembangan |  |

## Perancangan Sistem

* ERD: <img width="2057" height="1919" alt="LOKA_Logical" src="https://github.com/user-attachments/assets/aeee1712-01e1-466a-9726-e27587923f7e" />

* Relational: <img width="5175" height="2707" alt="LOKA_Relational" src="https://github.com/user-attachments/assets/7b246e96-e853-427a-a95a-3ad59ab907ed" />

* Flowchart & Use case: https://drive.google.com/file/d/17GgE9G_xQwHs0BnOM0JndjFk6LYqK0E1/view

## Struktur Folder/Package

## Penerapan OOP

### Encapsulation
Encapsulation adalah proses menyembunyikan data (atribut) melalui modifikator private dan hanya memperbolehkan akses melalui method khusus (getter dan setter). Pada LOKA, konsep ini diterapkan secara ketat dengan menambahkan fungsionalitas validasi di dalam metode setter (misalnya memastikan format email memiliki '@' atau saldo tidak boleh minus), sehingga integritas dan keamanan data objek selalu terjaga sebelum diproses oleh sistem.

**Class `Pengguna`**: atribut `private`, setter yang memanggil validasi sebelum menyimpan nilai.

```java
public class Pengguna {
    private String email;
    private String password;
    // ...

    public String getEmail() { return email; }

    public void setEmail(String email) {
        if (validasiEmail(email)) {
            this.email = email;
        }
    }

    public boolean validasiEmail(String email) {
        if (email != null && email.contains("@") && email.contains(".")) {
            return true;
        } else {
            System.out.println("Peringatan: Format email tidak valid (harus mengandung '@' dan '.')!");
            return false;
        }
    }
}
```

**Class `Pembeli`**: validasi saldo agar tidak bernilai minus.

```java
public class Pembeli extends Pengguna {
    private double saldo;
    // ...

    public void setSaldo(double saldo) {
        if (validasiSaldoPembeli(saldo)) {
            this.saldo = saldo;
        }
    }

    public boolean validasiSaldoPembeli(double saldo) {
        if (saldo >= 0) {
            return true;
        } else {
            System.out.println("Peringatan: Saldo pembeli tidak boleh bernilai minus!");
            return false;
        }
    }
}
```

### Inheritance
Inheritance (Pewarisan) digunakan untuk mencegah duplikasi kode dengan cara mewariskan atribut dasar dari Super-class Pengguna (seperti ID, nama, email, dan sandi) kepada tiga Sub-class spesifik: Penjual, Pembeli, dan Pengelola. Melalui pemanggilan metode super() di dalam konstruktor masing-masing, Sub-class dapat menginisialisasi identitas dasarnya ke class induk sekaligus menambahkan atribut eksklusifnya sendiri, seperti namaToko untuk entitas bisnis Penjual, alamatPengiriman untuk kebutuhan belanja Pembeli, dan posisi otoritas (Admin/Verifikator) untuk manajemen Pengelola.

#### Class Penjual
Class ini mewarisi atribut identitas dasar dari Pengguna, namun ia mendefinisikan atribut yang secara eksklusif hanya dimiliki oleh entitas bisnis. Penjual memiliki namaToko, noIzinUsaha, dan saldo operasional toko yang ditambahkan ke dalam konstruktornya.

```java
public class Penjual extends Pengguna {
    private String namaToko;
    private String noIzinUsaha;
    private double saldo;
    // ...

    public Penjual(int idPengguna, String namaLengkap, String email, String password,
                   String noTelpon, RolePengguna role, String namaToko, /* ... */
                   double saldo, String noIzinUsaha, /* ... */) {

        super(idPengguna, namaLengkap, email, password, noTelpon, role); // atribut warisan dari Pengguna

        setNamaToko(namaToko);          // atribut khusus Penjual
        setSaldo(saldo);
        setNoIzinUsaha(noIzinUsaha);
        // ...
    }
}
```

#### Class Pembeli
Sama halnya dengan Penjual, Class Pembeli menggunakan extends Pengguna. Namun, bukan berfokus pada atribut bisnis, ia menambahkan atribut yang spesifik mendukung kegiatan belanja, seperti alamatPengiriman yang detail, saldo LOKA-Pay pribadi, dan pin keamanan transaksi.

```java
public class Pembeli extends Pengguna {
    private String alamatPengiriman;
    private double saldo;
    private String pin;
    // ...

    public Pembeli(int idPengguna, String namaLengkap, String email, String password,
                   String noTelpon, RolePengguna role, Date tanggalLahir, /* ... */
                   String alamatPengiriman, double saldo, String pin, /* ... */) {

        super(idPengguna, namaLengkap, email, password, noTelpon, role); // atribut warisan dari Pengguna

        this.alamatPengiriman = alamatPengiriman;   // atribut khusus Pembeli
        setSaldo(saldo);
        setPin(pin);
        // ...
    }
}
```

#### Class Pengelola (Admin/Verifikator)
Berbeda dengan Pembeli atau Penjual yang terlibat langsung dalam transaksi komersial, Class Pengelola didesain khusus untuk manajemen sistem di balik layar. Setelah mewarisi kredensial dasar dari Pengguna, Class ini menambahkan satu atribut otorisasi khusus yaitu posisi (menggunakan tipe data Enum PosisiPengelola). Atribut ini mendefinisikan pembagian wewenang yang tegas antara seorang Admin (pemantau analitik) dan seorang Verifikator (pemberi validasi).

 ```java
public class Pengelola extends Pengguna {
    private PosisiPengelola posisi;   // atribut khusus Pengelola

    public Pengelola(int idPengguna, String namaLengkap, String email, String password,
                     String noTelpon, RolePengguna role, PosisiPengelola posisi) {

        super(idPengguna, namaLengkap, email, password, noTelpon, role); // atribut warisan dari Pengguna
        setPosisi(posisi);
    }
}
```

### Abstraction
Abstraction (Abstraksi) diterapkan untuk menyembunyikan detail kompleksitas data mentah dan membatasi input pengguna hanya pada fungsionalitas esensial yang distandardisasi menggunakan tipe data Enum. Melalui penerapan Enum seperti StatusPesanan, aplikasi menyembunyikan manipulasi tipe string mentah dan menciptakan sebuah kontrak konstan yang mencegah kesalahan pengetikan (human-error), sehingga integritas status operasional di dalam database selalu terjamin konsistensinya.

```java
package enums;

public enum StatusPesanan {
    Diproses,
    Dikirim,
    Selesai,
    Pengembalian,
    Dikembalikan
}
```

### Polymorphism
Polymorphism (Polimorfisme) memungkinkan satu antarmuka atau metode yang sama digunakan untuk menangani berbagai bentuk objek yang berbeda secara dinamis. Dalam sistem LOKA, konsep ini diimplementasikan pada metode login di Class LokaControl, di mana metode tersebut memiliki wujud kembalian abstrak berupa Super-class Pengguna, namun secara fleksibel dapat menelusuri, memeriksa, dan mengembalikan wujud asli objek yang berhasil login, baik itu sebagai objek turunan Pembeli, Penjual, maupun Pengelola.

```java
public Pengguna login(String email, String password) {   // tipe kembalian: Pengguna (super-class)

    for (Pembeli p : pembeliDAO.getAllPembeli()) {
        if (p.getEmail().equalsIgnoreCase(email) && p.getPassword().equals(password)) {
            return p;   // mengembalikan objek Pembeli
        }
    }

    for (Penjual p : penjualDAO.getAllPenjual()) {
        if (p.getEmail().equalsIgnoreCase(email) && p.getPassword().equals(password)) {
            return p;   // mengembalikan objek Penjual
        }
    }

    for (Pengelola p : pengelolaDAO.getAllPengelola()) {
        if (p.getEmail().equalsIgnoreCase(email) && p.getPassword().equals(password)) {
            return p;   // mengembalikan objek Pengelola
        }
    }

    return null;
}
```

## Cara Penggunaan Program

## Kontributor
| NO | NAMA | NIM | KELAS |
|----|------|-----|--------------|
| 1 | Nazmi Ramadani | 2509116031 | A |
| 2 | Atha Rina Sarwahita | 2509116047 | B |
| 3 | Naisyla Auliya Rahmi Putri | 2509116078 | B |
| 4 | Qonitah Khansa' Ayu Madani A | 2509116113 | C |
