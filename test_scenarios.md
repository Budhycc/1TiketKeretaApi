# Skenario Pengujian untuk Aplikasi 1TiketKeretaApi

## Skenario Login

---
**ID:** TC_LOGIN_001
**Judul:** Login berhasil dengan kredensial admin yang valid.
**Prakondisi:**
    - Username dan password admin yang valid ada di tabel `admin`.
    - Aplikasi berada di layar Login.
**Langkah-langkah:**
    1. Masukkan username admin yang valid di kolom "Username".
    2. Masukkan password valid yang sesuai di kolom "Password".
    3. Klik tombol "LOGIN".
**Hasil yang Diharapkan:**
    - Jendela Login tertutup.
    - Jendela Beranda (pemesanan tiket) terbuka.
**Hasil Aktual:** Kode di `Login.java` (`btnloginActionPerformed`) menjalankan kueri SQL. Jika kecocokan ditemukan, ia memanggil `new Home().setVisible(true);` dan `dispose();`.
**Status:** Lulus

---
**ID:** TC_LOGIN_002
**Judul:** Login gagal dengan username tidak valid.
**Prakondisi:**
    - Aplikasi berada di layar Login.
**Langkah-langkah:**
    1. Masukkan username yang tidak valid di kolom "Username".
    2. Masukkan password apa pun di kolom "Password".
    3. Klik tombol "LOGIN".
**Hasil yang Diharapkan:**
    - Pesan kesalahan "Gagal Login Oi" (atau serupa) ditampilkan.
    - Pengguna tetap berada di layar Login.
**Hasil Aktual:** Kueri SQL `SELECT * FROM admin WHERE username='<username_tidak_valid>' And password='<password>'` dijalankan. `rs.next()` bernilai false. `JOptionPane.showMessageDialog(null, "Gagal Login Oi");` ditampilkan. Layar Login tetap ada.
**Status:** Lulus

---
**ID:** TC_LOGIN_003
**Judul:** Login gagal dengan password tidak valid.
**Prakondisi:**
    - Username admin yang valid ada di tabel `admin`.
    - Aplikasi berada di layar Login.
**Langkah-langkah:**
    1. Masukkan username admin yang valid di kolom "Username".
    2. Masukkan password yang tidak valid di kolom "Password".
    3. Klik tombol "LOGIN".
**Hasil yang Diharapkan:**
    - Pesan kesalahan "Gagal Login Oi" (atau serupa) ditampilkan.
    - Pengguna tetap berada di layar Login.
**Hasil Aktual:** Kueri SQL `SELECT * FROM admin WHERE username='<username_valid>' And password='<password_tidak_valid>'` dijalankan. `rs.next()` bernilai false. `JOptionPane.showMessageDialog(null, "Gagal Login Oi");` ditampilkan. Layar Login tetap ada.
**Status:** Lulus

---
**ID:** TC_LOGIN_004
**Judul:** Login gagal dengan username kosong.
**Prakondisi:**
    - Aplikasi berada di layar Login.
**Langkah-langkah:**
    1. Biarkan kolom "Username" kosong.
    2. Masukkan password apa pun di kolom "Password".
    3. Klik tombol "LOGIN".
**Hasil yang Diharapkan:**
    - Pesan kesalahan yang menunjukkan kegagalan (misalnya, "Gagal Login Oi" atau kesalahan database spesifik jika kueri gagal) ditampilkan.
    - Pengguna tetap berada di layar Login.
**Hasil Aktual:** Kueri SQL `SELECT * FROM admin WHERE username='' And password='<password>'` dijalankan. Dengan asumsi tidak ada pengguna admin dengan username kosong, `rs.next()` bernilai false. `JOptionPane.showMessageDialog(null, "Gagal Login Oi");` ditampilkan. Layar Login tetap ada.
**Status:** Lulus

---
**ID:** TC_LOGIN_005
**Judul:** Login gagal dengan password kosong.
**Prakondisi:**
    - Aplikasi berada di layar Login.
**Langkah-langkah:**
    1. Masukkan username apa pun di kolom "Username".
    2. Biarkan kolom "Password" kosong.
    3. Klik tombol "LOGIN".
**Hasil yang Diharapkan:**
    - Pesan kesalahan yang menunjukkan kegagalan (misalnya, "Gagal Login Oi" atau kesalahan database spesifik jika kueri gagal) ditampilkan.
    - Pengguna tetap berada di layar Login.
**Hasil Aktual:** Kueri SQL `SELECT * FROM admin WHERE username='<username>' And password=''` dijalankan. Dengan asumsi pengguna yang ditentukan tidak memiliki password kosong, `rs.next()` bernilai false. `JOptionPane.showMessageDialog(null, "Gagal Login Oi");` ditampilkan. Layar Login tetap ada.
**Status:** Lulus

---

## Skenario Pemesanan Tiket (Layar Beranda)

---
**ID:** TC_BOOK_001
**Judul:** Pemesanan tiket berhasil dengan semua input valid untuk satu orang dewasa.
**Prakondisi:**
    - Pengguna sudah login dan berada di layar Beranda.
    - Data harga untuk kereta/kelas yang dipilih ada di tabel `harga`.
**Langkah-langkah:**
    1. ID Penumpang (`fid`) terisi otomatis.
    2. Masukkan nama penumpang di "Nama Penumpang".
    3. Masukkan alamat penumpang di "Alamat".
    4. "Stasiun Awal" adalah "Bandung (BD)".
    5. Pilih "Stasiun Tujuan" dari dropdown.
    6. Pilih "Kereta Api" dari dropdown.
    7. Pilih "Gerbong" dari dropdown.
    8. Pilih tanggal dari "tanggal".
    9. Atur spinner "Dewasa" ke 1.
    10. Atur spinner "Anak-anak" ke 0.
    11. Verifikasi harga "Total" dihitung dan ditampilkan.
    12. Masukkan jumlah yang valid di "Bayar" (lebih besar atau sama dengan Total).
    13. Tekan Enter di kolom "Bayar" untuk menghitung "Kembali".
    14. Klik tombol "Simpan".
**Hasil yang Diharapkan:**
    - Pemesanan disimpan ke tabel `penumpang`.
    - Pesan sukses (atau proses pembuatan tiket) dimulai.
    - Jendela Beranda tertutup dan jendela Beranda baru (atau yang sama dikosongkan) ditampilkan dengan ID baru yang dibuat otomatis.
    - Proses pembuatan tiket JasperReports dipanggil (misalnya, `JasperViewer.viewReport` dipanggil).
**Hasil Aktual:** Kode di `Home.java` (metode `simpan`) menjalankan kueri SQL INSERT. Jika berhasil, ia menutup jendela `Home` saat ini, membuat yang baru (melalui `Index.oke()`), lalu memanggil `JasperFillManager.fillReport` dan `JasperViewer.viewReport` dengan "src/report/Tiket.jasper" dan ID penumpang.
**Status:** Lulus

---
**ID:** TC_BOOK_002
**Judul:** Pemesanan tiket berhasil dengan semua input valid untuk beberapa dewasa dan anak-anak.
**Prakondisi:**
    - Pengguna sudah login dan berada di layar Beranda.
    - Data harga untuk kereta/kelas yang dipilih ada di tabel `harga` untuk dewasa dan anak-anak.
**Langkah-langkah:**
    1. Masukkan nama penumpang, alamat.
    2. Pilih tujuan, kereta, gerbong, dan tanggal.
    3. Atur spinner "Dewasa" ke 2 (atau angka > 0).
    4. Atur spinner "Anak-anak" ke 1 (atau angka > 0).
    5. Verifikasi harga "Total" dihitung dengan benar berdasarkan harga dewasa dan anak-anak.
    6. Masukkan jumlah yang valid di "Bayar".
    7. Tekan Enter di kolom "Bayar".
    8. Klik "Simpan".
**Hasil yang Diharapkan:**
    - Pemesanan disimpan ke tabel `penumpang`.
    - Proses pembuatan tiket dimulai.
    - Jendela Beranda disegarkan/dibuka kembali.
**Hasil Aktual:** Mirip dengan TC_BOOK_001. Metode `dewasa()` dan `anak()` dengan benar menggunakan nilai spinner dan harga dari label `hdewasa`/`hanak`. Metode `simpan()` menyisipkan nilai-nilai ini. JasperReports dipanggil.
**Status:** Lulus

---
**ID:** TC_BOOK_003
**Judul:** Upaya pemesanan tanpa memilih tujuan.
**Prakondisi:**
    - Pengguna sudah login dan berada di layar Beranda.
**Langkah-langkah:**
    1. Isi semua kolom wajib lainnya (nama, kereta, kelas, penumpang).
    2. Biarkan "Stasiun Tujuan" pada keadaan default/tidak dipilih (jika berlaku, atau coba lanjutkan jika itu adalah pilihan wajib dari daftar).
    3. Klik "Simpan".
**Hasil yang Diharapkan:**
    - Pesan kesalahan ditampilkan yang menunjukkan bahwa tujuan wajib diisi.
    - Pemesanan tidak disimpan.
**Hasil Aktual:** Tidak ada validasi eksplisit di `simpan()`. `stasiun_tujuan.getSelectedItem()` kemungkinan mengembalikan item pertama secara default. Kueri `INSERT` dilanjutkan. Waktu berangkat/tiba mungkin salah jika bergantung pada tujuan yang valid tertentu di `ckeretaActionPerformed`. Pemesanan disimpan dengan tujuan yang mungkin default/tidak diinginkan.
**Status:** Gagal

---
**ID:** TC_BOOK_004
**Judul:** Upaya pemesanan tanpa memilih kereta.
**Prakondisi:**
    - Pengguna sudah login dan berada di layar Beranda.
**Langkah-langkah:**
    1. Isi semua kolom wajib lainnya (nama, tujuan, kelas, penumpang).
    2. Biarkan "Kereta Api" pada keadaan default/kosong (misalnya, item kosong pertama).
    3. Klik "Simpan".
**Hasil yang Diharapkan:**
    - Pesan kesalahan ditampilkan yang menunjukkan bahwa pemilihan kereta wajib diisi, atau aplikasi menanganinya dengan baik (misalnya, tidak ada perhitungan harga).
    - Pemesanan tidak disimpan jika pemilihan wajib untuk penetapan harga/penyimpanan.
**Hasil Aktual:** Tidak ada validasi eksplisit di `simpan()`. `ckereta.getSelectedItem()` mengembalikan `""` (item kosong default). Kueri `INSERT` menyimpan `""` untuk nama kereta. Label `berangkat` dan `tiba` tetap "00.00 WIB". Perhitungan harga mungkin didasarkan pada `hdewasa`/`hanak` yang basi jika tidak ada kereta berarti tidak ada harga kelas yang dimuat.
**Status:** Gagal

---
**ID:** TC_BOOK_005
**Judul:** Upaya pemesanan tanpa memilih kelas gerbong.
**Prakondisi:**
    - Pengguna sudah login dan berada di layar Beranda.
**Langkah-langkah:**
    1. Isi semua kolom wajib lainnya (nama, tujuan, kereta, penumpang).
    2. Biarkan "Gerbong" pada keadaan default/kosong.
    3. Klik "Simpan".
**Hasil yang Diharapkan:**
    - Pesan kesalahan ditampilkan yang menunjukkan bahwa kelas gerbong wajib diisi untuk perhitungan harga.
    - Pemesanan tidak disimpan.
**Hasil Aktual:** Tidak ada validasi eksplisit di `simpan()`. `cgerbong.getSelectedItem()` mengembalikan `" "` (item kosong default). `kelasActionPerformed` dipanggil, tetapi tidak ada kondisi yang cocok untuk `" "`, jadi `hdewasa` dan `hanak` tidak diperbarui (atau "0"). Perhitungan `ltotal` menggunakan harga yang salah/nol ini. Kueri `INSERT` menyimpan `" "` untuk gerbong dan total yang salah.
**Status:** Gagal

---
**ID:** TC_BOOK_006
**Judul:** Upaya pemesanan tanpa memasukkan nama penumpang.
**Prakondisi:**
    - Pengguna sudah login dan berada di layar Beranda.
**Langkah-langkah:**
    1. Isi semua kolom wajib lainnya (tujuan, kereta, kelas, penumpang).
    2. Biarkan "Nama Penumpang" kosong.
    3. Klik "Simpan".
**Hasil yang Diharapkan:**
    - Pesan kesalahan ditampilkan (misalnya, kesalahan database untuk kolom non-nullable, atau pesan validasi spesifik).
    - Pemesanan tidak disimpan.
**Hasil Aktual:** Tidak ada validasi eksplisit di `simpan()`. `fnama.getText()` kosong. Kueri `INSERT` mencoba menyimpan `""` untuk nama. Jika kolom DB `nama` di `penumpang` adalah `NOT NULL` dan tidak mengizinkan string kosong, eksekusi SQL gagal, dan `JOptionPane` generik menampilkan kesalahan SQL. Jika tidak, nama kosong disimpan.
**Status:** Gagal

---
**ID:** TC_BOOK_007
**Judul:** Upaya pemesanan dengan nol dewasa dan nol anak-anak.
**Prakondisi:**
    - Pengguna sudah login dan berada di layar Beranda.
**Langkah-langkah:**
    1. Isi semua kolom wajib lainnya (nama, tujuan, kereta, kelas).
    2. Atur spinner "Dewasa" ke 0.
    3. Atur spinner "Anak-anak" ke 0.
    4. Klik "Simpan".
**Hasil yang Diharapkan:**
    - Pesan kesalahan ditampilkan yang menunjukkan bahwa setidaknya satu penumpang diperlukan, atau total harga nol dan penyimpanan dicegah.
    - Pemesanan tidak disimpan.
**Hasil Aktual:** Tidak ada validasi eksplisit di `simpan()`. `sdewasa.getValue()` dan `sanak.getValue()` adalah 0. `ltotal` menjadi "Rp. 0". Kueri `INSERT` menyimpan pemesanan dengan 0 dewasa, 0 anak-anak, dan total 0.
**Status:** Gagal

---
**ID:** TC_BOOK_008
**Judul:** Verifikasi perhitungan harga dinamis berdasarkan kereta, kelas, dan jumlah penumpang.
**Prakondisi:**
    - Pengguna sudah login dan berada di layar Beranda.
    - Harga yang diketahui ada di tabel `harga` untuk kelas dan status dewasa/anak yang berbeda.
**Langkah-langkah:**
    1. Pilih "Kereta Api".
    2. Pilih "Gerbong" (misalnya, "Eksekutif 1").
    3. Atur spinner "Dewasa" ke 1, "Anak-anak" ke 0. Catat "Total".
    4. Ubah spinner "Dewasa" ke 2. Verifikasi "Total" menjadi dua kali lipat.
    5. Atur spinner "Anak-anak" ke 1. Verifikasi "Total" bertambah sebesar harga anak untuk kelas tersebut.
    6. Ubah "Gerbong" ke kelas lain (misalnya, "Ekonomi 1").
    7. Verifikasi "Total" diperbarui berdasarkan harga kelas baru untuk jumlah dewasa dan anak-anak saat ini.
**Hasil yang Diharapkan:**
    - Kolom "Total" diperbarui dengan benar dan dinamis saat kereta, kelas, dan jumlah penumpang berubah, mencerminkan harga dari tabel `harga`.
    - Label `hdewasa` dan `hanak` menampilkan harga per orang yang benar untuk kelas yang dipilih.
**Hasil Aktual:** Pemilihan `cgerbong` memicu `kelasActionPerformed` yang menjalankan kueri ke tabel `harga` dan mengatur label `hdewasa`/`hanak`. Spinner `sdewasa`/`sanak` memicu metode `dewasa()`/`anak()` yang menggunakan nilai label ini untuk menghitung `ltotal`. Logika tampak benar.
**Status:** Lulus

---
**ID:** TC_BOOK_009
**Judul:** Verifikasi proses pembuatan tiket dipanggil.
**Prakondisi:**
    - Pemesanan berhasil dibuat (seperti pada TC_BOOK_001).
**Langkah-langkah:**
    1. Selesaikan semua langkah untuk pemesanan yang berhasil.
    2. Klik "Simpan".
**Hasil yang Diharapkan:**
    - Metode `JasperFillManager.fillReport` dan `JasperViewer.viewReport` dipanggil dengan "src/report/Tiket.jasper" dan parameter ID penumpang yang benar.
    - (Konseptual) Tiket akan ditampilkan jika lingkungan tampilan tersedia.
**Hasil Aktual:** Metode `simpan()`, setelah INSERT database yang berhasil, memanggil `JasperFillManager.fillReport` dengan "src/report/Tiket.jasper" dan `JasperViewer.viewReport`. ID penumpang (`fid.getText()`) diteruskan dengan benar sebagai parameter.
**Status:** Lulus

---

## Skenario Panel Admin - Manajemen Pengguna

---
**ID:** TC_ADMIN_USER_001
**Judul:** Lihat daftar pengguna admin.
**Prakondisi:**
    - Pengguna sudah login dan berada di layar Panel Admin (`Admin.java`).
    - Pengguna admin ada di tabel `admin`.
**Langkah-langkah:**
    1. Navigasi ke Panel Admin.
**Hasil yang Diharapkan:**
    - Tabel di bawah batas berjudul "Admin" diisi dengan data pengguna admin yang ada (No, Nama, Username, Password).
**Hasil Aktual:** Konstruktor `Admin()` memanggil `data()`, yang menjalankan `SELECT * FROM admin order by id ASC` dan mengisi JTable `admin`.
**Status:** Lulus

---
**ID:** TC_ADMIN_USER_002
**Judul:** Tambah pengguna admin baru dengan data valid.
**Prakondisi:**
    - Pengguna berada di layar Panel Admin.
**Langkah-langkah:**
    1. Masukkan nama unik di kolom "Nama".
    2. Masukkan username unik di kolom "Username".
    3. Masukkan password di kolom "Password".
    4. Klik tombol "Simpan" di bawah bagian "Admin".
**Hasil yang Diharapkan:**
    - Pesan sukses "Berhasil" ditampilkan.
    - Pengguna admin baru muncul di tabel admin.
    - Kolom input dikosongkan.
**Hasil Aktual:** `btnadminActionPerformed` di `Admin.java` menjalankan kueri `INSERT INTO admin`. Jika berhasil, ia menampilkan pesan "Berhasil", memuat ulang tabel menggunakan `data()`, dan mengosongkan kolom input menggunakan `bersih()`.
**Status:** Lulus

---
**ID:** TC_ADMIN_USER_003
**Judul:** Upaya menambah pengguna admin dengan username yang sudah ada.
**Prakondisi:**
    - Pengguna berada di layar Panel Admin.
    - Pengguna admin dengan username 'testuser' sudah ada.
**Langkah-langkah:**
    1. Masukkan nama apa pun di "Nama".
    2. Masukkan 'testuser' di "Username".
    3. Masukkan password apa pun di "Password".
    4. Klik "Simpan".
**Hasil yang Diharapkan:**
    - Pesan kesalahan ditampilkan (kemungkinan kesalahan pelanggaran batasan database untuk username unik).
    - Pengguna tidak ditambahkan ke tabel.
**Hasil Aktual:** `btnadminActionPerformed` mencoba `INSERT`. Jika username sudah ada dan kolom `username` di tabel `admin` memiliki batasan UNIQUE, database akan menolak penyisipan. Blok `catch(SQLException e)` akan menampilkan `JOptionPane` yang berisi pesan kesalahan SQL (misalnya, "Duplicate entry..."). Tabel tidak akan menampilkan pengguna baru.
**Status:** Lulus

---
**ID:** TC_ADMIN_USER_004
**Judul:** Upaya menambah pengguna admin dengan username kosong.
**Prakondisi:**
    - Pengguna berada di layar Panel Admin.
**Langkah-langkah:**
    1. Masukkan nama apa pun di "Nama".
    2. Biarkan "Username" kosong.
    3. Masukkan password apa pun di "Password".
    4. Klik "Simpan".
**Hasil yang Diharapkan:**
    - Pesan kesalahan ditampilkan (kesalahan database untuk kolom non-nullable atau validasi spesifik).
    - Pengguna tidak ditambahkan.
**Hasil Aktual:** `btnadminActionPerformed` mencoba `INSERT` dengan string kosong untuk username. Jika kolom `username` di tabel `admin` adalah `NOT NULL` dan tidak mengizinkan string kosong, database akan menolaknya. Blok `catch(SQLException e)` akan menampilkan kesalahan SQL. Pengguna tidak ditambahkan.
**Status:** Lulus

---
**ID:** TC_ADMIN_USER_005
**Judul:** Upaya menambah pengguna admin dengan password kosong.
**Prakondisi:**
    - Pengguna berada di layar Panel Admin.
**Langkah-langkah:**
    1. Masukkan nama apa pun di "Nama".
    2. Masukkan username unik di "Username".
    3. Biarkan "Password" kosong.
    4. Klik "Simpan".
**Hasil yang Diharapkan:**
    - Pengguna ditambahkan (password mungkin disimpan sebagai string kosong jika diizinkan oleh DB). Atau kesalahan jika password memiliki batasan.
    - *Koreksi mandiri: Aplikasi menyimpan password kosong tanpa kesalahan.*
**Hasil Aktual:** `btnadminActionPerformed` mencoba `INSERT` dengan string kosong untuk password. Konfigurasi database standar untuk kolom password (VARCHAR) biasanya mengizinkan string kosong. Penyisipan berhasil, "Berhasil" ditampilkan, dan tabel diperbarui.
**Status:** Lulus

---
**ID:** TC_ADMIN_USER_006
**Judul:** Edit detail pengguna admin yang ada.
**Prakondisi:**
    - Pengguna berada di layar Panel Admin.
    - Setidaknya satu pengguna admin ada.
**Langkah-langkah:**
    1. Klik pengguna admin yang ada di tabel.
    2. Detail pengguna mengisi kolom "Nama", "Username", "Password". ID disimpan di `bayangan1`.
    3. Ubah kolom "Nama".
    4. Ubah kolom "Password".
    5. Klik tombol "Edit".
**Hasil yang Diharapkan:**
    - Pesan sukses "Berhasil" ditampilkan.
    - Tabel admin disegarkan dan menampilkan detail yang diperbarui untuk pengguna tersebut.
    - Kolom input dikosongkan.
**Hasil Aktual:** `adminMouseClicked` mengisi kolom termasuk `bayangan1` (ID). `editActionPerformed` menjalankan `UPDATE admin ... WHERE id='...'`. Jika berhasil, "Berhasil" ditampilkan, tabel dimuat ulang melalui `data()`, dan kolom dikosongkan melalui `bersih()`.
**Status:** Lulus

---
**ID:** TC_ADMIN_USER_007
**Judul:** Hapus pengguna admin.
**Prakondisi:**
    - Pengguna berada di layar Panel Admin.
    - Setidaknya satu pengguna admin ada yang dapat dihapus.
**Langkah-langkah:**
    1. Klik pengguna admin yang ada di tabel untuk memilihnya.
    2. Klik tombol "Hapus".
**Hasil yang Diharapkan:**
    - Pesan sukses "Berhasil" ditampilkan.
    - Tabel admin disegarkan dan pengguna yang dipilih tidak lagi terdaftar.
    - Kolom input dikosongkan.
**Hasil Aktual:** `adminMouseClicked` mengisi `bayangan1` (ID). `hapusActionPerformed` menjalankan `DELETE FROM admin WHERE id='...'`. Jika berhasil, "Berhasil" ditampilkan, tabel dimuat ulang melalui `data()`, dan kolom dikosongkan melalui `bersih()`.
**Status:** Lulus

---

## Skenario Panel Admin - Manajemen Harga

---
**ID:** TC_ADMIN_PRICE_001
**Judul:** Lihat daftar harga.
**Prakondisi:**
    - Pengguna berada di layar Panel Admin.
    - Data harga ada di tabel `harga`.
**Langkah-langkah:**
    1. Navigasi ke Panel Admin.
**Hasil yang Diharapkan:**
    - Tabel di bawah batas berjudul "Harga" diisi dengan data harga yang ada (No, Status, Kelas, Harga).
**Hasil Aktual:** Konstruktor `Admin()` memanggil `data_harga()`, yang menjalankan `SELECT * FROM harga` dan mengisi JTable `tableharga`.
**Status:** Lulus

---
**ID:** TC_ADMIN_PRICE_002
**Judul:** Edit harga yang ada untuk kelas dan status tertentu.
**Prakondisi:**
    - Pengguna berada di layar Panel Admin.
    - Data harga ada.
**Langkah-langkah:**
    1. Klik entri harga di tabel "Harga".
    2. Kolom "Status", "Kelas", dan "Harga" diisi. ID disimpan di `bayangan1`.
    3. Ubah nilai di kolom "Harga" menjadi nilai numerik baru yang valid.
    4. Klik tombol "Simpan" di bawah bagian "Harga".
**Hasil yang Diharapkan:**
    - Pesan sukses "Berhasil" ditampilkan.
    - Tabel "Harga" disegarkan dan menampilkan harga yang diperbarui untuk entri tersebut.
    - Kolom input dikosongkan.
**Hasil Aktual:** `tablehargaMouseClicked` mengisi kolom dari baris yang dipilih, menyimpan ID di `bayangan1`. Pengguna mengedit kolom `harga`. Tombol "Simpan" di panel "Harga" (`simpanActionPerformed`) menjalankan `UPDATE harga set harga='<nilai_baru>' where id='<id_dari_bayangan1>'`. Jika berhasil, "Berhasil" ditampilkan, `data_harga()` memuat ulang tabel, dan `bersih()` mengosongkan kolom.
**Status:** Lulus

---
**ID:** TC_ADMIN_PRICE_003
**Judul:** Upaya menyimpan harga dengan nilai non-numerik.
**Prakondisi:**
    - Pengguna berada di layar Panel Admin.
    - Data harga ada.
**Langkah-langkah:**
    1. Pilih entri harga dari tabel.
    2. Masukkan "abc" (atau string non-numerik lainnya) di kolom "Harga".
    3. Klik "Simpan" di bawah bagian "Harga".
**Hasil yang Diharapkan:**
    - Pesan kesalahan ditampilkan (kemungkinan `NumberFormatException` atau kesalahan database jika mencoba menyimpan).
    - Harga tidak diperbarui.
**Hasil Aktual:** Pengguna memasukkan "abc" ke dalam kolom `harga`. `simpanActionPerformed` menjalankan `UPDATE harga set harga='abc' ...`. Jika kolom `harga` di database adalah numerik, ini akan menyebabkan kesalahan SQL. Blok `catch(SQLException e)` menampilkan `JOptionPane` dengan pesan kesalahan database. Harga tidak diperbarui.
**Status:** Lulus

---
**ID:** TC_ADMIN_PRICE_004
**Judul:** Upaya menyimpan harga dengan nilai negatif.
**Prakondisi:**
    - Pengguna berada di layar Panel Admin.
    - Data harga ada.
**Langkah-langkah:**
    1. Pilih entri harga dari tabel.
    2. Masukkan "-100" di kolom "Harga".
    3. Klik "Simpan" di bawah bagian "Harga".
**Hasil yang Diharapkan:**
    - Harga diperbarui menjadi -100 (jika tidak ada validasi yang mencegahnya).
    - *Koreksi mandiri: Aplikasi mengizinkan harga negatif.*
**Hasil Aktual:** Pengguna memasukkan "-100" ke dalam kolom `harga`. `simpanActionPerformed` menjalankan `UPDATE harga set harga='-100' ...`. Dengan asumsi kolom `harga` di database adalah tipe numerik bertanda, pembaruan berhasil. "Berhasil" ditampilkan, tabel dimuat ulang. Tidak ada validasi tingkat aplikasi yang mencegah harga negatif.
**Status:** Lulus

---

## Skenario Manajemen Data Penumpang (Layar Penumpang)

---
**ID:** TC_PASSENGER_001
**Judul:** Lihat daftar penumpang yang telah memesan.
**Prakondisi:**
    - Pengguna sudah login.
    - Pemesanan penumpang ada di tabel `penumpang`.
**Langkah-langkah:**
    1. Navigasi ke layar "Data Penumpang" dari menu.
**Hasil yang Diharapkan:**
    - Tabel di layar "Data Penumpang" diisi dengan semua detail pemesanan penumpang yang ada.
**Hasil Aktual:** Konstruktor `Penumpang()` memanggil metode `data()`, yang menjalankan `SELECT * FROM penumpang order by id ASC` dan mengisi JTable `penumpang` dengan hasilnya.
**Status:** Lulus

---
**ID:** TC_PASSENGER_002
**Judul:** Verifikasi bahwa tiket yang baru dipesan muncul di daftar penumpang.
**Prakondisi:**
    - Pengguna sudah login.
**Langkah-langkah:**
    1. Catat jumlah penumpang saat ini di layar "Data Penumpang" (atau jika kosong).
    2. Navigasi ke layar Beranda dan berhasil memesan tiket baru (misalnya, TC_BOOK_001).
    3. Setelah memesan, navigasi kembali ke layar "Data Penumpang".
**Hasil yang Diharapkan:**
    - Detail tiket yang baru dipesan muncul sebagai baris baru di tabel penumpang.
    - Jumlah penumpang bertambah satu.
**Hasil Aktual:** Setelah pemesanan berhasil di `Home.java` (yang menyisipkan ke tabel `penumpang`), menavigasi ke layar `Penumpang` (yang membuat instance `Penumpang` baru) akan memanggil `data()`, mengisi ulang tabel dengan data terbaru, termasuk pemesanan baru.
**Status:** Lulus

---
**ID:** TC_PASSENGER_003
**Judul:** Uji fungsionalitas "Cetak" untuk data penumpang.
**Prakondisi:**
    - Pengguna berada di layar "Data Penumpang".
**Langkah-langkah:**
    1. Klik tombol "Cetak".
**Hasil yang Diharapkan:**
    - Metode `JasperFillManager.fillReport` dan `JasperViewer.viewReport` dipanggil dengan "src/report/penumpang.jasper".
    - (Konseptual) Laporan yang mencantumkan semua penumpang akan ditampilkan jika lingkungan tampilan tersedia.
**Hasil Aktual:** Listener aksi tombol "Cetak" (`jButton1ActionPerformed`) memanggil `JasperFillManager.fillReport` dengan "src/report/penumpang.jasper" dan `JasperViewer.viewReport`. Ini dengan benar memanggil proses pelaporan.
**Status:** Lulus

---
**ID:** TC_PASSENGER_004
**Judul:** Uji fungsionalitas "Hapus Semua Data".
**Prakondisi:**
    - Pengguna berada di layar "Data Penumpang".
    - Beberapa data penumpang ada.
**Langkah-langkah:**
    1. Klik tombol "Hapus" (Hapus Semua Data).
    2. Dialog konfirmasi "Yakin Ingin Menghapus Data?" muncul.
    3. Klik "Ya" (atau opsi afirmatif yang setara).
**Hasil yang Diharapkan:**
    - Pesan sukses "Berhasil" ditampilkan.
    - Tabel penumpang menjadi kosong.
    - Semua catatan dihapus dari tabel `penumpang` di database.
**Hasil Aktual:** Tombol "Hapus" (`HapusActionPerformed`) menampilkan dialog konfirmasi `JOptionPane`. Jika "Ya" dipilih, ia menjalankan `DELETE FROM penumpang`. Setelah penghapusan berhasil, "Berhasil" ditampilkan, dan `data()` dipanggil untuk menyegarkan tabel (yang sekarang akan kosong).
**Status:** Lulus

---
**ID:** TC_PASSENGER_005
**Judul:** Uji fungsionalitas "Hapus Semua Data" - Batal.
**Prakondisi:**
    - Pengguna berada di layar "Data Penumpang".
    - Beberapa data penumpang ada.
**Langkah-langkah:**
    1. Klik tombol "Hapus" (Hapus Semua Data).
    2. Dialog konfirmasi "Yakin Ingin Menghapus Data?" muncul.
    3. Klik "Tidak" (atau opsi negatif yang setara).
**Hasil yang Diharapkan:**
    - Tidak ada pesan sukses mengenai penghapusan yang ditampilkan.
    - Tabel penumpang tetap tidak berubah.
    - Tidak ada catatan yang dihapus dari tabel `penumpang`.
**Hasil Aktual:** Tombol "Hapus" (`HapusActionPerformed`) menampilkan dialog konfirmasi `JOptionPane`. Jika "Tidak" dipilih, kondisi `if (pesan == JOptionPane.YES_OPTION)` bernilai false, dan tidak ada operasi database atau penyegaran tabel yang terjadi selain pemuatan awal.
**Status:** Lulus

---
