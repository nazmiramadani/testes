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
    
    import enums.RolePengguna;
    
    public class Pengguna {
        private int idPengguna;
        private String namaLengkap;
        private String email;
        private String password;
        private String noTelpon;
        private RolePengguna role;
    
        public Pengguna() {}
    
        public Pengguna(int idPengguna, String namaLengkap, String email, String password, String noTelpon, RolePengguna role) {
            setIdPengguna(idPengguna);
            setNamaLengkap(namaLengkap);
            setEmail(email);
            setPassword(password);
            setNoTelpon(noTelpon);
            this.role = role;
        }
    
        public int getIdPengguna() { 
            return idPengguna; 
        }
        
        public void setIdPengguna(int idPengguna) {
            if (validasiIdPengguna(idPengguna)) {
                this.idPengguna = idPengguna;
            }
        }
    
        public String getNamaLengkap() { 
            return namaLengkap; 
        }
        
        public void setNamaLengkap(String namaLengkap) {
            if (validasiNamaLengkap(namaLengkap)) {
                this.namaLengkap = namaLengkap.trim();
            }
        }
    
        public String getEmail() { return email; }
        public void setEmail(String email) {
            if (validasiEmail(email)) {
                this.email = email;
            }
        }
    
        public String getPassword() { 
            return password; 
        }
        
        public void setPassword(String password) {
            if (validasiPassword(password)) {
                this.password = password;
            }
        }
    
        public String getNoTelpon() { 
            return noTelpon; 
        }
        
        public void setNoTelpon(String noTelpon) {
            if (validasiNoTelpon(noTelpon)) {
                this.noTelpon = noTelpon;
            }
        }
    
        public RolePengguna getRole() { 
            return role; 
        }
        
        public void setRole(RolePengguna role) { 
            this.role = role; 
        }
    
        public boolean validasiIdPengguna(int id) {
            if (id > 0) {
                return true;
            } else {
                System.out.println("Peringatan: ID pengguna harus lebih dari 0!");
                return false;
            }
        }
    
        public boolean validasiNamaLengkap(String nama) {
            if (nama != null && !nama.trim().isEmpty()) {
                return true;
            } else {
                System.out.println("Peringatan: Nama lengkap tidak boleh kosong!");
                return false;
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
    
        public boolean validasiPassword(String password) {
            if (password != null && password.length() >= 8) {
                return true;
            } else {
                System.out.println("Peringatan: Password minimal harus 8 karakter!");
                return false;
            }
        }
    
        public boolean validasiNoTelpon(String noTelpon) {
            if (noTelpon != null && noTelpon.matches("[0-9]+") && noTelpon.length() >= 11) {
                return true;
            } else {
                System.out.println("Peringatan: Nomor telepon hanya boleh berisi angka dan minimal 11 digit!");
                return false;
            }
        }
    }

### Inheritance
Inheritance adalah kemampuan Class untuk mewarisi atribut dan metode dari Class induk, sehingga menghindari duplikasi penulisan kode. Pada sistem LOKA, hierarki pengguna dipecah menjadi tiga entitas spesifik, yaitu Pembeli, Penjual, dan Pengelola yang semuanya mewarisi atribut dasar (seperti idPengguna, namaLengkap, email, dan password) dari Class utama Pengguna menggunakan kata kunci extends.

#### Class Penjual
Class ini mewarisi atribut identitas dasar dari Pengguna, namun ia mendefinisikan atribut yang secara eksklusif hanya dimiliki oleh entitas bisnis. Penjual memiliki namaToko, noIzinUsaha, dan saldo operasional toko yang ditambahkan ke dalam konstruktornya.

    package model;
    
    import enums.RolePengguna;
    import enums.StatusAkun;
    import enums.StatusVerifikasi;
    import java.util.Date;
    
    public class Penjual extends Pengguna {
        private String namaToko;
        private String deskripsiToko;
        private String alamatToko;
        private String kota;
        private String kecamatan;
        private String kodePos;
        private double saldo;
        private String noIzinUsaha;
        private StatusVerifikasi statusVerifikasi;
        private Date tanggalVerifikasi;
        private StatusAkun statusAkun;
        private Date tanggalDaftar;
    
        public Penjual() {
        }
    
        public Penjual(int idPengguna, String namaLengkap, String email, String password, String noTelpon, RolePengguna role, String namaToko, String deskripsiToko, String alamatToko, String kota, String kecamatan, String kodePos, double saldo, String noIzinUsaha, StatusVerifikasi statusVerifikasi, Date tanggalVerifikasi, StatusAkun statusAkun, Date tanggalDaftar) {
            
            super(idPengguna, namaLengkap, email, password, noTelpon, role);
            
            setNamaToko(namaToko);
            this.deskripsiToko = deskripsiToko;
            this.alamatToko = alamatToko;
            this.kota = kota;
            this.kecamatan = kecamatan;
            this.kodePos = kodePos;
            setSaldo(saldo);
            setNoIzinUsaha(noIzinUsaha);
            this.statusVerifikasi = statusVerifikasi;
            this.tanggalVerifikasi = tanggalVerifikasi;
            this.statusAkun = statusAkun;
            this.tanggalDaftar = tanggalDaftar;
        }
    
        public String getNamaToko() { 
            return namaToko; 
        }
        
        public void setNamaToko(String namaToko) {
            if (validasiNamaToko(namaToko)) {
                this.namaToko = namaToko;
            }
        }
    
        public String getDeskripsiToko() { 
            return deskripsiToko; 
        }
        
        public void setDeskripsiToko(String deskripsiToko) { 
            this.deskripsiToko = deskripsiToko; 
        }
    
        public String getAlamatToko() { 
            return alamatToko; 
        }
        
        public void setAlamatToko(String alamatToko) { 
            this.alamatToko = alamatToko; 
        }
    
        public String getKota() { 
            return kota; 
        }
        
        public void setKota(String kota) { 
            this.kota = kota; 
        }
    
        public String getKecamatan() { 
            return kecamatan; 
        }
        
        public void setKecamatan(String kecamatan) { 
            this.kecamatan = kecamatan; 
        }
    
        public String getKodePos() { 
            return kodePos; 
        }
        
        public void setKodePos(String kodePos) { 
            this.kodePos = kodePos; 
        }
    
        public double getSaldo() { 
            return saldo; 
        }
        
        public void setSaldo(double saldo) {
            if (validasiSaldoPenjual(saldo)) {
                this.saldo = saldo;
            }
        }
    
        public String getNoIzinUsaha() { 
            return noIzinUsaha; 
        }
        
        public void setNoIzinUsaha(String noIzinUsaha) {
            if (validasiNoIzinUsaha(noIzinUsaha)) {
                this.noIzinUsaha = noIzinUsaha;
            }
        }
    
        public StatusVerifikasi getStatusVerifikasi() { 
            return statusVerifikasi; 
        }
        
        public void setStatusVerifikasi(StatusVerifikasi statusVerifikasi) { 
            this.statusVerifikasi = statusVerifikasi; 
        }
    
        public Date getTanggalVerifikasi() { 
            return tanggalVerifikasi; 
        }
        
        public void setTanggalVerifikasi(Date tanggalVerifikasi) { 
            this.tanggalVerifikasi = tanggalVerifikasi; 
        }
    
        public StatusAkun getStatusAkun() { 
            return statusAkun; 
        }
        
        public void setStatusAkun(StatusAkun statusAkun) { 
            this.statusAkun = statusAkun; 
        }
    
        public Date getTanggalDaftar() { 
            return tanggalDaftar; 
        }
        
        public void setTanggalDaftar(Date tanggalDaftar) { 
            this.tanggalDaftar = tanggalDaftar; 
        }
    
        public boolean validasiNamaToko(String namaToko) {
            if (namaToko != null && !namaToko.trim().isEmpty()) {
                return true;
            } else {
                System.out.println("Peringatan: Nama toko tidak boleh kosong!");
                return false;
            }
        }
    
        public boolean validasiSaldoPenjual(double saldo) {
            if (saldo >= 0) {
                return true;
            } else {
                System.out.println("Peringatan: Saldo penjual tidak boleh bernilai minus!");
                return false;
            }
        }
    
        public boolean validasiNoIzinUsaha(String noIzinUsaha) {
            if (noIzinUsaha != null && !noIzinUsaha.trim().isEmpty()) {
                return true;
            } else {
                System.out.println("Peringatan: Nomor izin usaha wajib diisi!");
                return false;
            }
        }
    }

#### Class Pembeli
Sama halnya dengan Penjual, Class Pembeli menggunakan extends Pengguna. Namun, bukan berfokus pada atribut bisnis, ia menambahkan atribut yang spesifik mendukung kegiatan belanja, seperti alamatPengiriman yang detail, saldo LOKA-Pay pribadi, dan pin keamanan transaksi.

    package model;
    
    import enums.JenisKelamin;
    import enums.RolePengguna;
    import enums.StatusAkun;
    import java.util.Date;
    
    public class Pembeli extends Pengguna {
        private Date tanggalLahir;
        private JenisKelamin jenisKelamin;
        private String kota;
        private String kecamatan;
        private String kodePos;
        private String alamatPengiriman;
        private double saldo;
        private String pin;
        private StatusAkun statusAkun;
        private Date tanggalDaftar;
    
        public Pembeli() {}
    
        public Pembeli(int idPengguna, String namaLengkap, String email, String password, String noTelpon, RolePengguna role, Date tanggalLahir, JenisKelamin jenisKelamin, String kota, String kecamatan, String kodePos, String alamatPengiriman, double saldo, String pin, StatusAkun statusAkun, Date tanggalDaftar) {
            super(idPengguna, namaLengkap, email, password, noTelpon, role);
            this.tanggalLahir = tanggalLahir;
            this.jenisKelamin = jenisKelamin;
            this.kota = kota;
            this.kecamatan = kecamatan;
            this.kodePos = kodePos;
            this.alamatPengiriman = alamatPengiriman;
            setSaldo(saldo);
            setPin(pin);
            this.statusAkun = statusAkun;
            this.tanggalDaftar = tanggalDaftar;
        }
    
        public Date getTanggalLahir() { 
            return tanggalLahir; 
        }
        
        public void setTanggalLahir(Date tanggalLahir) { 
            this.tanggalLahir = tanggalLahir; 
        }
    
        public JenisKelamin getJenisKelamin() { 
            return jenisKelamin; 
        }
        
        public void setJenisKelamin(JenisKelamin jenisKelamin) { 
            this.jenisKelamin = jenisKelamin; 
        }
    
        public String getKota() { 
            return kota; 
        }
        
        public void setKota(String kota) { 
            this.kota = kota; 
        }
    
        public String getKecamatan() { 
            return kecamatan; 
        }
        
        public void setKecamatan(String kecamatan) { 
            this.kecamatan = kecamatan; 
        }
    
        public String getKodePos() { 
            return kodePos; 
        }
        
        public void setKodePos(String kodePos) { 
            this.kodePos = kodePos; 
        }
    
        public String getAlamatPengiriman() { 
            return alamatPengiriman; 
        }
        
        public void setAlamatPengiriman(String alamatPengiriman) { 
            this.alamatPengiriman = alamatPengiriman; 
        }
    
        public double getSaldo() { 
            return saldo; 
        }
        
        public void setSaldo(double saldo) {
            if (validasiSaldoPembeli(saldo)) {
                this.saldo = saldo;
            }
        }
    
        public String getPin() { 
            return pin; 
        }
        
        public void setPin(String pin) {
            if (validasiPin(pin)) {
                this.pin = pin;
            }
        }
    
        public StatusAkun getStatusAkun() { 
            return statusAkun; 
        }
        
        public void setStatusAkun(StatusAkun statusAkun) { 
            this.statusAkun = statusAkun; 
        }
    
        public Date getTanggalDaftar() { 
            return tanggalDaftar; 
        }
        
        public void setTanggalDaftar(Date tanggalDaftar) { 
            this.tanggalDaftar = tanggalDaftar; 
        }
    
       
        public boolean validasiSaldoPembeli(double saldo) {
            if (saldo >= 0) {
                return true;
            } else {
                System.out.println("Peringatan: Saldo pembeli tidak boleh bernilai minus!");
                return false;
            }
        }
    
        public boolean validasiPin(String pin) {
            if (pin != null && pin.matches("[0-9]{6}")) {
                return true;
            } else {
                System.out.println("Peringatan: PIN harus berupa 6 digit angka!");
                return false;
            }
        }
    }

#### Class Pengelola (Admin/Verifikator)
Berbeda dengan Pembeli atau Penjual yang terlibat langsung dalam transaksi komersial, Class Pengelola didesain khusus untuk manajemen sistem di balik layar. Setelah mewarisi kredensial dasar dari Pengguna, Class ini menambahkan satu atribut otorisasi khusus yaitu posisi (menggunakan tipe data Enum PosisiPengelola). Atribut ini mendefinisikan pembagian wewenang yang tegas antara seorang Admin (pemantau analitik) dan seorang Verifikator (pemberi validasi).

    package model;
    
    import enums.PosisiPengelola;
    import enums.RolePengguna;
    
    public class Pengelola extends Pengguna {
        private PosisiPengelola posisi;
    
        public Pengelola() {}
    
        public Pengelola(int idPengguna, String namaLengkap, String email, String password, String noTelpon, RolePengguna role, PosisiPengelola posisi) {
            super(idPengguna, namaLengkap, email, password, noTelpon, role);
            setPosisi(posisi);
        }
    
        public PosisiPengelola getPosisi() { 
            return posisi; 
        }
        
        public void setPosisi(PosisiPengelola posisi) {
            if (validasiPosisi(posisi)) {
                this.posisi = posisi;
            }
        }
    
        public boolean validasiPosisi(PosisiPengelola posisi) {
            if (posisi != null) {
                return true;
            } else {
                System.out.println(" Posisi pengelola tidak boleh kosong!");
                return false;
            }
        }
    }
    }

### Abstraction
