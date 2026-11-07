# Lapak Online Komunitas Lokal (LOKA)

## Deskripsi Singkat

LOKA merupakan sebuah platform marketplace digital yang didedikasikan secara eksklusif untuk pemberdayaan Usaha Mikro, Kecil, dan Menengah (UMKM) lokal di wilayah Kota Samarinda. Platform ini dirancang untuk mengatasi keterbatasan jangkauan pemasaran dan mengelola operasional bisnis secara terstruktur melalui sistem digital. Dengan fitur dasbor penjual, pencarian katalog produk, integrasi pembayaran digital, hingga kalkulasi ongkos kirim berdasarkan jarak, LOKA memfasilitasi pertemuan antara penjual dan pembeli secara efisien. Pengembangan sistem ini sejalan dengan komitmen terhadap Sustainable Development Goals (SDGs) poin 8 (Pekerjaan Layak & Pertumbuhan Ekonomi), 9 (Industri, Inovasi, & Infrastruktur), dan 12 (Konsumsi & Produksi yang Bertanggung Jawab).

Flow Chart & Use Case:

## Fitur Program

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

## Penerapan OOP

### Encapsulation

Encapsulation adalah proses menyembunyikan data (atribut) dan hanya memperbolehkan akses melalui method khusus (getter dan setter). LOKA juga menambahkan fungsi validasi ketat di dalam setter (misalnya saldo tidak boleh minus, email harus mengandung '@') agar data tetap valid.


      package model;
      public class Pengguna {
          private String email;
          
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
                  System.out.println("Peringatan: Format email tidak valid!");
                  return false;
              }
          }
      }

### Inheritance
Inheritance adalah kemampuan Class untuk mewarisi atribut dan metode dari Class induk, sehingga menghindari duplikasi penulisan kode. Pada sistem LOKA, hierarki pengguna dipecah menjadi tiga entitas spesifik, yaitu Pembeli, Penjual, dan Pengelola yang semuanya mewarisi atribut dasar (seperti idPengguna, namaLengkap, email, dan password) dari Class utama Pengguna menggunakan kata kunci extends.

#### Class Penjual
Mewarisi data dasar pengguna dan menambahkan atribut khusus bisnis seperti namaToko, noIzinUsaha, dan saldo toko.

    package model;
    import enums.RolePengguna;
    
    public class Penjual extends Pengguna {
        private String namaToko;
        private double saldo;
        private String noIzinUsaha;
    
        public Penjual(int idPengguna, String namaLengkap, String email, String password, String noTelpon, RolePengguna role, String namaToko, double saldo, String noIzinUsaha /*...*/) {
            // Memanggil konstruktor atribut dasar dari class Pengguna
            super(idPengguna, namaLengkap, email, password, noTelpon, role);
            setNamaToko(namaToko);
            setSaldo(saldo);
            setNoIzinUsaha(noIzinUsaha);
        }
    }

#### Class Pembeli
Mewarisi data dasar pengguna dan menambahkan atribut khusus belanja seperti alamatPengiriman, saldo dompet pembeli, dan pin keamanan.

    package model;
    import enums.RolePengguna;
    
    public class Pembeli extends Pengguna {
        private String alamatPengiriman;
        private double saldo;
        private String pin;
    
        public Pembeli(int idPengguna, String namaLengkap, String email, String password, String noTelpon, RolePengguna role, String alamatPengiriman, double saldo, String pin /*...*/) {
            super(idPengguna, namaLengkap, email, password, noTelpon, role);
            this.alamatPengiriman = alamatPengiriman;
            setSaldo(saldo);
            setPin(pin);
        }
    }

#### Class Pengelola (Admin/Verifikator)
Mewarisi data dasar pengguna dan menambahkan atribut otorisasi yaitu posisi pengelola.

    package model;
    import enums.PosisiPengelola;
    import enums.RolePengguna;
    
    public class Pengelola extends Pengguna {
        private PosisiPengelola posisi;
    
        public Pengelola(int idPengguna, String namaLengkap, String email, String password, String noTelpon, RolePengguna role, PosisiPengelola posisi) {
            super(idPengguna, namaLengkap, email, password, noTelpon, role);
            setPosisi(posisi);
        }
    }


