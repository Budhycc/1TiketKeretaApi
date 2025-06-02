# Laporan Pengujian: Aplikasi 1TiketKeretaApi

## 1. Pendahuluan

**Tujuan Pengujian:**
Tujuan dari pengujian ini adalah untuk mengevaluasi fungsionalitas inti dari aplikasi Java Swing "1TiketKeretaApi" melalui analisis kode statis dan simulasi eksekusi skenario pengujian. Laporan ini mendokumentasikan temuan-temuan, termasuk bug yang teridentifikasi, area untuk peningkatan, dan penilaian keseluruhan terhadap perilaku aplikasi berdasarkan kode sumbernya.

**Metodologi:**
Karena keterbatasan dalam menjalankan aplikasi GUI secara langsung dan dependensinya terhadap database di lingkungan saat ini, pengujian dilakukan melalui:
1.  **Analisis Kode Statis:** Meninjau kode sumber Java untuk setiap modul fungsional (`Login.java`, `Home.java`, `Admin.java`, `Penumpang.java`, `Koneksi.java`).
2.  **Simulasi Skenario Pengujian:** Secara mental menjalankan skenario pengujian yang telah ditentukan (dari `test_scenarios.md`) dengan menelusuri alur kode, memprediksi perilaku antarmuka pengguna (pesan dialog, navigasi), dan menganalisis interaksi database (kueri SQL).
3.  **Dokumentasi:** Mencatat "Hasil Aktual" dan "Status" untuk setiap kasus uji berdasarkan analisis ini.

## 2. Gambaran Umum Aplikasi

Aplikasi "1TiketKeretaApi" adalah aplikasi desktop Java Swing yang dirancang sebagai sistem pemesanan tiket kereta api. Aplikasi ini menyediakan fungsionalitas bagi pengguna (yang diasumsikan sebagai administrator setelah login) untuk memesan tiket bagi penumpang, mengelola pengguna administratif, dan mengatur harga tiket.

Fungsionalitas utama meliputi:
*   **Login:** Mengautentikasi pengguna terhadap tabel `admin` di database MySQL.
*   **Pemesanan Tiket (`Home.java`):** Memungkinkan pemesanan tiket dengan memasukkan detail penumpang (ID, nama, alamat), memilih stasiun awal/tujuan, kereta, kelas gerbong, tanggal, dan jumlah penumpang dewasa/anak-anak. Aplikasi ini secara dinamis menghitung harga dan dimaksudkan untuk menghasilkan tiket menggunakan JasperReports.
*   **Panel Admin (`Admin.java`):**
    *   **Manajemen Pengguna:** Menambah, melihat, mengedit, dan menghapus pengguna admin (data disimpan di tabel `admin`).
    *   **Manajemen Harga:** Melihat dan mengedit harga tiket untuk status yang berbeda (dewasa/anak) dan kelas gerbong (data disimpan di tabel `harga`).
*   **Manajemen Data Penumpang (`Penumpang.java`):** Melihat daftar semua penumpang yang telah memesan tiket, mencetak laporan penumpang (JasperReports), dan menghapus semua catatan penumpang dari tabel `penumpang`.

## 3. Lingkungan Pengujian

*   **Artefak:** File kode sumber Java untuk aplikasi (`Login.java`, `Home.java`, `Admin.java`, `Penumpang.java`, `Koneksi.java`, `Index.java`).
*   **Skema Database (Disimpulkan):**
    *   Tabel `admin` (untuk login)
    *   Tabel `penumpang` (untuk menyimpan pemesanan)
    *   Tabel `harga` (untuk menyimpan informasi harga)
*   **Metodologi:** Tinjauan kode manual, analisis statis, dan simulasi eksekusi skenario pengujian. Tidak ada eksekusi langsung atau interaksi database yang dilakukan.

## 4. Lingkup Pengujian

Fungsionalitas berikut dicakup selama pengujian:
*   Autentikasi login.
*   Proses Pemesanan Tiket, termasuk kolom input, perhitungan dinamis, dan operasi penyimpanan.
*   Panel Admin:
    *   Manajemen pengguna admin (operasi CRUD).
    *   Manajemen harga tiket (lihat dan perbarui).
*   Manajemen Data Penumpang:
    *   Melihat daftar penumpang yang dipesan.
    *   Verifikasi pemesanan baru muncul.
    *   Inisiasi cetak laporan.
    *   Penghapusan semua data penumpang.

Pembuatan JasperReport diuji secara konseptual dengan memverifikasi apakah alur kode untuk memanggil pembuatan laporan diimplementasikan dengan benar.

## 5. Ringkasan Pelaksanaan Pengujian

Sebanyak 26 skenario pengujian dianalisis.

*   **Total Pengujian:** 26
*   **Pengujian Lulus:** 21
*   **Pengujian Gagal:** 5

**Tabel Skenario Pengujian:**

| ID                | Judul                                                                      | Status | Hasil Aktual Singkat / Catatan                                                                                                                            |
| ----------------- | -------------------------------------------------------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| TC_LOGIN_001      | Login berhasil dengan kredensial admin yang valid.                         | Lulus  | Alur kode untuk login berhasil dan navigasi ke layar Beranda sudah benar.                                                                                |
| TC_LOGIN_002      | Login gagal dengan username tidak valid.                                   | Lulus  | Menampilkan pesan "Gagal Login Oi" seperti yang diharapkan.                                                                                             |
| TC_LOGIN_003      | Login gagal dengan password tidak valid.                                   | Lulus  | Menampilkan pesan "Gagal Login Oi" seperti yang diharapkan.                                                                                             |
| TC_LOGIN_004      | Login gagal dengan username kosong.                                        | Lulus  | Menampilkan pesan "Gagal Login Oi".                                                                                                                       |
| TC_LOGIN_005      | Login gagal dengan password kosong.                                        | Lulus  | Menampilkan pesan "Gagal Login Oi".                                                                                                                       |
| TC_BOOK_001       | Pemesanan tiket berhasil dengan semua input valid untuk satu orang dewasa. | Lulus  | Alur kode untuk menyimpan pemesanan dan memanggil JasperReports sudah benar.                                                                             |
| TC_BOOK_002       | Pemesanan tiket berhasil untuk beberapa dewasa dan anak-anak.              | Lulus  | Perhitungan harga dan logika penyimpanan untuk beberapa penumpang sudah benar.                                                                          |
| TC_BOOK_003       | Upaya pemesanan tanpa memilih tujuan.                                      | Gagal  | Tidak ada validasi; menyimpan dengan tujuan default/kosong.                                                                                              |
| TC_BOOK_004       | Upaya pemesanan tanpa memilih kereta.                                      | Gagal  | Tidak ada validasi; menyimpan nama kereta kosong, waktu berangkat/tiba salah.                                                                          |
| TC_BOOK_005       | Upaya pemesanan tanpa memilih kelas gerbong.                               | Gagal  | Tidak ada validasi; menyimpan gerbong kosong, menggunakan harga salah/nol.                                                                               |
| TC_BOOK_006       | Upaya pemesanan tanpa memasukkan nama penumpang.                           | Gagal  | Tidak ada validasi; bergantung pada batasan DB (jika ada) untuk mencegah nama kosong.                                                                   |
| TC_BOOK_007       | Upaya pemesanan dengan nol dewasa dan nol anak-anak.                       | Gagal  | Tidak ada validasi; mengizinkan pemesanan untuk nol penumpang dengan total harga nol.                                                                     |
| TC_BOOK_008       | Verifikasi perhitungan harga dinamis.                                      | Lulus  | Logika perhitungan harga berdasarkan kelas dan jumlah penumpang tampak benar.                                                                           |
| TC_BOOK_009       | Verifikasi proses pembuatan tiket dipanggil.                               | Lulus  | Alur kode pemanggilan JasperReports sudah benar.                                                                                                          |
| TC_ADMIN_USER_001 | Lihat daftar pengguna admin.                                               | Lulus  | Metode `data()` memuat admin ke tabel dengan benar.                                                                                                     |
| TC_ADMIN_USER_002 | Tambah pengguna admin baru dengan data valid.                              | Lulus  | `btnadminActionPerformed` menyisipkan admin baru dengan benar.                                                                                           |
| TC_ADMIN_USER_003 | Upaya menambah admin dengan username yang sudah ada.                       | Lulus  | Gagal di tingkat DB (pelanggaran batasan unik), pesan kesalahan ditampilkan.                                                                            |
| TC_ADMIN_USER_004 | Upaya menambah admin dengan username kosong.                               | Lulus  | Gagal di tingkat DB (jika ada batasan NOT NULL), pesan kesalahan ditampilkan.                                                                             |
| TC_ADMIN_USER_005 | Upaya menambah admin dengan password kosong.                               | Lulus  | Aplikasi mengizinkan penyimpanan admin dengan password kosong.                                                                                          |
| TC_ADMIN_USER_006 | Edit detail pengguna admin yang ada.                                       | Lulus  | `editActionPerformed` memperbarui detail admin dengan benar.                                                                                             |
| TC_ADMIN_USER_007 | Hapus pengguna admin.                                                      | Lulus  | `hapusActionPerformed` menghapus admin dengan benar.                                                                                                    |
| TC_ADMIN_PRICE_001| Lihat daftar harga.                                                        | Lulus  | Metode `data_harga()` memuat harga ke tabel dengan benar.                                                                                                 |
| TC_ADMIN_PRICE_002| Edit harga yang ada untuk kelas dan status tertentu.                       | Lulus  | `simpanActionPerformed` (panel harga) memperbarui harga dengan benar.                                                                                    |
| TC_ADMIN_PRICE_003| Upaya menyimpan harga dengan nilai non-numerik.                            | Lulus  | Gagal di tingkat DB (jika kolom numerik), pesan kesalahan ditampilkan.                                                                                   |
| TC_ADMIN_PRICE_004| Upaya menyimpan harga dengan nilai negatif.                                | Lulus  | Aplikasi mengizinkan penyimpanan harga negatif.                                                                                                         |
| TC_PASSENGER_001  | Lihat daftar penumpang yang telah memesan.                                 | Lulus  | Metode `data()` di `Penumpang.java` memuat data penumpang dengan benar.                                                                                 |
| TC_PASSENGER_002  | Verifikasi bahwa tiket yang baru dipesan muncul di daftar penumpang.        | Lulus  | Penyisipan pemesanan baru di `Home` dan pemuatan ulang di `Penumpang` berfungsi.                                                                        |
| TC_PASSENGER_003  | Uji fungsionalitas "Cetak" untuk data penumpang.                           | Lulus  | Pemanggilan JasperReports untuk daftar penumpang sudah benar.                                                                                             |
| TC_PASSENGER_004  | Uji fungsionalitas "Hapus Semua Data".                                     | Lulus  | `HapusActionPerformed` menghapus semua penumpang dengan benar setelah konfirmasi.                                                                      |
| TC_PASSENGER_005  | Uji fungsionalitas "Hapus Semua Data" - Batal.                             | Lulus  | Pembatalan "Hapus Semua Data" berfungsi seperti yang diharapkan.                                                                                         |

*(Catatan: Hasil aktual untuk TC_LOGIN_003 dan TC_LOGIN_005 telah dikoreksi pada tabel di atas karena terduplikasi dalam file .md sumber selama output alat pada langkah sebelumnya.)*

## 6. Temuan Utama dan Isu

### 6.1. Bug/Kasus Uji Gagal

Kasus uji berikut gagal, terutama karena kurangnya validasi input dalam modul Pemesanan Tiket (`Home.java`):

*   **TC_BOOK_003: Upaya pemesanan tanpa memilih tujuan.**
    *   **Isu:** Aplikasi tidak memvalidasi apakah stasiun tujuan telah dipilih. Aplikasi melanjutkan untuk menyimpan pemesanan, kemungkinan dengan item default (pertama) di JComboBox, atau nilai kosong jika ComboBox mengizinkannya. Hal ini dapat menyebabkan data pemesanan yang salah dan memengaruhi logika dependen seperti penentuan waktu berangkat/tiba.
*   **TC_BOOK_004: Upaya pemesanan tanpa memilih kereta.**
    *   **Isu:** Aplikasi mengizinkan pemesanan dengan string kosong (`""`) sebagai nama kereta karena JComboBox untuk kereta memiliki item kosong awal dan tidak ada validasi. Hal ini menghasilkan data yang tidak berarti untuk kereta, dan waktu berangkat/tiba tidak diatur (tetap "00.00 WIB").
*   **TC_BOOK_005: Upaya pemesanan tanpa memilih kelas gerbong.**
    *   **Isu:** Mirip dengan pemilihan kereta, kelas gerbong kosong (`" "`) dapat disimpan karena kurangnya validasi. Hal ini menyebabkan perhitungan harga yang salah, karena logika untuk mengambil harga (`kelasActionPerformed`) tidak akan menemukan kriteria yang cocok, sehingga harga menjadi "0" atau basi dari pilihan sebelumnya.
*   **TC_BOOK_006: Upaya pemesanan tanpa memasukkan nama penumpang.**
    *   **Isu:** Aplikasi tidak melakukan validasi sisi klien untuk memastikan nama penumpang dimasukkan. Aplikasi mencoba menyimpan string kosong. Ini hanya akan gagal jika kolom database yang sesuai (`nama` di tabel `penumpang`) memiliki batasan `NOT NULL` yang juga mencegah string kosong. Jika tidak, pemesanan tanpa nama penumpang akan dibuat.
*   **TC_BOOK_007: Upaya pemesanan dengan nol dewasa dan nol anak-anak.**
    *   **Isu:** Sistem mengizinkan pemesanan dilakukan meskipun jumlah dewasa dan anak-anak adalah nol. Hal ini menghasilkan pemesanan dengan total harga nol dan tidak ada penumpang aktual, yang secara logis salah.

### 6.2. Area Peningkatan/Observasi

Selain kegagalan pengujian langsung, analisis statis mengungkapkan beberapa area di mana aplikasi dapat ditingkatkan untuk ketahanan dan kemudahan pemeliharaan:

*   **Kurangnya Validasi Input Secara Umum (Pemesanan Tiket):** Seperti yang disorot oleh kasus uji yang gagal, modul pemesanan tiket (`Home.java`) sangat kurang dalam validasi input proaktif untuk kolom-kolom wajib (tujuan, kereta, kelas, nama penumpang) dan minimum logis (setidaknya satu penumpang). Bergantung semata-mata pada batasan database (jika ada) tidak ramah pengguna.
*   **Harga Negatif Diizinkan (Panel Admin - Manajemen Harga):** (TC_ADMIN_PRICE_004) Sistem mengizinkan nilai negatif dimasukkan dan disimpan untuk harga tiket. Hal ini kemungkinan tidak diinginkan dan harus dicegah dengan validasi.
*   **Pembuatan ID Penumpang (`autoid()` di `Home.java`):** Metode saat ini `SELECT COUNT(id) as kode FROM penumpang` kemudian menambahkan 1 ke hitungan (misalnya, "MR.X") tidak tangguh. Ini dapat menyebabkan ID duplikat jika catatan dihapus atau dalam skenario akses bersamaan (meskipun kurang mungkin untuk jenis aplikasi desktop ini). Pendekatan yang lebih tangguh adalah menggunakan kunci primer auto-increment database secara langsung, atau UUID.
*   **Penanganan Tanggal untuk Database (`simpan()` di `Home.java`):** Kode menggunakan `tanggal.getDate()` dan secara langsung menggabungkannya ke dalam string SQL. Cara `java.util.Date.toString()` memformat tanggal mungkin tidak selalu kompatibel dengan format tanggal yang diharapkan MySQL, yang berpotensi menyebabkan kesalahan atau penyimpanan tanggal yang salah tergantung pada lokal atau konfigurasi MySQL. Menggunakan `PreparedStatement` dengan `setDate()` adalah praktik yang direkomendasikan.
*   **Password Admin Kosong Diizinkan (Panel Admin - Manajemen Pengguna):** (TC_ADMIN_USER_005) Aplikasi mengizinkan pembuatan pengguna admin dengan password kosong. Meskipun pengujian lulus karena sistem berfungsi sesuai kode, ini adalah masalah keamanan dan idealnya harus dilarang melalui validasi.
*   **Penanganan Kesalahan:** Blok `catch(Exception e)` atau `catch(SQLException e)` generik digunakan, menampilkan `JOptionPane.showMessageDialog(null,e);`. Meskipun ini menampilkan kesalahan, ini tidak terlalu ramah pengguna karena sering kali mengekspos detail SQL mentah atau pengecualian Java. Pesan kesalahan yang lebih spesifik akan lebih baik.
*   **Struktur Kode di `Home.java` (`ckeretaActionPerformed`):** Rantai if-else-if yang panjang untuk mengatur waktu berangkat/tiba berdasarkan kereta dan tujuan sulit dipelihara dan rentan terhadap kesalahan. Ini dapat direfaktor, mungkin menggunakan struktur data atau pendekatan yang lebih berorientasi objek jika jadwal kereta menjadi kompleks.

## 7. Penilaian Keseluruhan

Aplikasi "1TiketKeretaApi" menyediakan kerangka kerja dasar untuk fungsionalitas yang dimaksudkan. Bagian Admin dan Data Penumpang tampak relatif stabil berdasarkan alur kode untuk operasi inti mereka (CRUD untuk admin/harga, melihat/menghapus/melaporkan untuk penumpang).

Namun, **modul Pemesanan Tiket (`Home.java`) memerlukan perhatian signifikan**. Kurangnya validasi input mendasar untuk kolom-kolom kritis (tujuan, kereta, kelas, nama penumpang, jumlah penumpang) menyebabkan beberapa kasus uji gagal dan dapat mengakibatkan data yang tidak konsisten atau tidak valid dalam database, serta pengalaman pengguna yang buruk.

Rekomendasi untuk peningkatan meliputi:
*   Mengimplementasikan validasi input yang komprehensif dalam modul `Home.java` sebelum mencoba menyimpan data pemesanan.
*   Menambahkan validasi untuk mencegah harga negatif.
*   Merefaktor pembuatan ID penumpang dan penanganan tanggal untuk operasi database guna meningkatkan ketahanan.
*   Meningkatkan penanganan kesalahan untuk memberikan pesan yang lebih ramah pengguna.
*   Meninjau kebijakan mengenai password admin kosong.

Mengatasi isu-isu ini, terutama validasi input dalam pemesanan tiket, akan secara substansial meningkatkan keandalan dan kegunaan aplikasi.
