# 1TiketKeretaApi - Sistem Pemesanan Tiket Kereta Api

## JAR yang Dapat Dieksekusi

```
dist/1TiketKeretaApi.jar
```

---

## Layar Awal (Login)

* Form login dengan kolom:

  * Username
  * Password
* Autentikasi dilakukan terhadap tabel `admin` di database.

---

## Navigasi Utama (`tiket.Home`)

Setelah login berhasil, pengguna diarahkan ke jendela utama dengan fitur berikut:

### Formulir Pemesanan Tiket

* Kolom input:

  * ID Penumpang
  * Nama Penumpang
  * Alamat
  * Stasiun Awal (default: Bandung - BD)
  * Stasiun Tujuan
  * Kereta (default: Argo Wilis, Argo Parahyangan, Lodaya)
  * Gerbong (default: Eksekutif 1/2, Ekonomi 1/2/3)
  * Tanggal Keberangkatan
  * Jumlah Penumpang Dewasa dan Anak-anak

### Perhitungan Harga

* Harga tiket dihitung secara dinamis berdasarkan:

  * Status (Dewasa/Anak)
  * Kelas Gerbong
* Data harga diambil dari tabel `harga` di database.

### Pembuatan Tiket

* Data pemesanan disimpan ke tabel `penumpang`.
* Tiket dicetak menggunakan JasperReports (`Tiket.jasper`).

---

## Bilah Menu

### Menu File

* **Admin**: Akses ke pengelolaan admin dan harga.
* **Keluar**: Menutup aplikasi.

### Menu Data Penumpang

* Akses ke tampilan dan pengelolaan data penumpang.

---

## Panel Admin (`Admin.java`)

Akses melalui: Menu File → Admin

### 1. Manajemen Pengguna Admin

* Fitur:

  * Tambah Admin
  * Edit Admin
  * Hapus Admin
* Kolom:

  * Nama
  * Username
  * Password
* Tampilan menggunakan `JTable`.
* Menggunakan `JTextField bayangan1` untuk menyimpan ID tersembunyi saat edit/hapus.

### 2. Manajemen Harga Tiket

* Lihat dan edit harga tiket.
* Kolom:

  * Status (Dewasa/Anak)
  * Kelas Gerbong
  * Harga
* Menggunakan `JTable` dan tombol Simpan untuk menyimpan perubahan.

---

## Panel Data Penumpang (`Penumpang.java`)

Akses melalui: Menu Data Penumpang

### Fitur:

* Melihat semua data dari tabel `penumpang`.
* Kolom tampilan:

  * ID Penumpang
  * Nama
  * Alamat
  * Kereta
  * Gerbong
  * Tujuan
  * Waktu Berangkat / Tiba
  * Tanggal
  * Jumlah Dewasa / Anak
  * Total Harga
* Cetak Laporan Penumpang: Menggunakan `penumpang.jasper`.
* Hapus Semua Data Penumpang: Mengosongkan tabel `penumpang` setelah konfirmasi.

---

## Catatan Teknis

* Bahasa: Java (GUI menggunakan Swing)
* Database: MySQL

  * Koneksi ditangani oleh `tiket.Koneksi`
* Logika utama aplikasi: `tiket.Home`
* Laporan menggunakan JasperReports
* Driver Database: `mysql-connector-java`
* Menjalankan aplikasi di lingkungan tanpa GUI (headless) akan menyebabkan `java.awt.HeadlessException`

---

## Ringkasan Fitur Utama

| Fitur           | Deskripsi                                                 |
| --------------- | --------------------------------------------------------- |
| Login           | Autentikasi ke sistem sebagai admin.                      |
| Pemesanan Tiket | Formulir lengkap dengan kalkulasi harga otomatis.         |
| Cetak Tiket     | Menyimpan dan mencetak tiket dalam format JasperReport.   |
| Admin Panel     | Kelola akun admin dan harga tiket.                        |
| Data Penumpang  | Lihat, cetak laporan, dan hapus data pemesanan penumpang. |
| Navigasi GUI    | Antar-frame: Home, Admin, dan Penumpang melalui menu.     |
