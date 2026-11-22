# Lab8Web – Praktikum 8: PHP dan Database MySQL

| | |
|---|---|
| **Nama** | Saqila Elby Neina |
| **NIM** | 312510173 |
| **Mata Kuliah** | Pemrograman Web OBE |
| **Program Studi** | Teknik Informatika |

## Tujuan
1. Memahami konsep dasar database.
2. Memahami konsep dasar CRUD menggunakan PHP.
3. Membuat program CRUD sederhana menggunakan PHP dan MySQL.

## Struktur File
| File | Fungsi |
|------|--------|
| `database.sql` | Membuat database `latihan1`, tabel `data_barang`, dan data awal |
| `koneksi.php` | Koneksi ke database MySQL |
| `index.php` | Menampilkan data barang (Read) |
| `tambah.php` | Menambah data barang (Create) |
| `ubah.php` | Mengubah data barang (Update) |
| `hapus.php` | Menghapus data barang (Delete) |
| `style.css` | Tampilan halaman |
| `gambar/` | Folder penyimpanan gambar barang |

---

## Langkah Praktikum

### 1. Persiapan: Menjalankan Apache dan MySQL
Buka **XAMPP Control Panel**, lalu klik **Start** pada modul **Apache** dan **MySQL**. Keduanya harus berstatus *running* karena Apache melayani file PHP dan MySQL menyimpan data.

> Pada praktikum ini Apache berjalan di port **8080**, sehingga alamat yang digunakan adalah `http://localhost:8080/`.

### 2. Membuat Database dan Tabel
Buka `http://localhost:8080/phpmyadmin/`, pilih tab **SQL**, lalu jalankan isi `database.sql`:

```sql
CREATE DATABASE latihan1;

CREATE TABLE data_barang (
  id_barang int(10) auto_increment Primary Key,
  kategori varchar(30),
  nama varchar(50),
  gambar varchar(100),
  harga_beli decimal(10,0),
  harga_jual decimal(10,0),
  stok int(4)
);
```

Penjelasan: database `latihan1` berisi tabel `data_barang`. Kolom `id_barang` adalah *primary key* yang bertambah otomatis.

### 3. Menambahkan Data Awal
```sql
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

Tiga data barang dimasukkan sekaligus. Data ini muncul pada halaman utama di langkah Read.

### 4. Membuat Folder Proyek
Buat folder `lab8_php_database` di dalam `htdocs` (misalnya `d:\xampp\htdocs`), lalu akses melalui `http://localhost:8080/lab8_php_database/`.

### 5. File Koneksi Database (`koneksi.php`)
```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db   = "latihan1";

$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
    echo "Koneksi ke server gagal.";
    die();
} #else echo "Koneksi berhasil";
?>
```

Penjelasan: `mysqli_connect()` menghubungkan PHP ke MySQL dengan host, user, password, dan nama database. Jika gagal, pesan error ditampilkan dan program berhenti. Hilangkan tanda `#` pada `echo "Koneksi berhasil"` untuk menguji koneksi. File ini di-*include* oleh semua file lain.

### 6. Menampilkan Data (Read) – `index.php`
`index.php` menjalankan query `SELECT * FROM data_barang`, lalu menampilkan setiap baris ke dalam tabel HTML dengan perulangan `while` dan `mysqli_fetch_array()`. Setiap baris memiliki link **Ubah** dan **Hapus**, dan di atas tabel ada link **Tambah Barang**.

**Screenshot: tampilan awal data barang (3 data)**

![Tampilan index](screenshots/01_read_index.jpeg)

### 7. Menambah Data (Create) – `tambah.php`
Halaman ini berisi form (nama, kategori, harga jual, harga beli, stok, file gambar) dengan `method="post"` dan `enctype="multipart/form-data"` agar bisa mengunggah file. Saat tombol **Simpan** ditekan:
1. Data dari `$_POST` diambil dan diamankan dengan `mysqli_real_escape_string()`.
2. Gambar dipindahkan ke folder `gambar/` dengan `move_uploaded_file()`.
3. Data disimpan dengan query `INSERT INTO data_barang ...`.
4. Pengguna diarahkan kembali ke `index.php` dengan `header()`.

*(Tambahkan screenshot form Tambah Barang di sini.)*

### 8. Mengubah Data (Update) – `ubah.php`
Klik **Ubah** pada salah satu baris. ID barang dikirim lewat URL (`ubah.php?id=1`), data lama diambil dengan `SELECT ... WHERE id_barang`, lalu ditampilkan pada form. Setelah **Simpan** ditekan, data diperbarui dengan query `UPDATE data_barang SET ... WHERE id_barang`. Gambar hanya diganti jika pengguna memilih file baru.

**Screenshot: form Ubah Barang (id=1, HP Samsung Android)**

![Tampilan ubah](screenshots/02_update_ubah.jpeg)

### 9. Menghapus Data (Delete) – `hapus.php`
Klik **Hapus** pada salah satu baris. Browser menampilkan konfirmasi *"Yakin hapus data ini?"* (dibuat dengan `onclick="return confirm(...)"`). Jika **OK**, `hapus.php?id=...` menjalankan query `DELETE FROM data_barang WHERE id_barang = ...`, lalu kembali ke `index.php`.

**Screenshot: konfirmasi hapus (data HP Xiaomi, id=2)**

![Konfirmasi hapus](screenshots/03_delete_konfirmasi.jpeg)

**Screenshot: hasil setelah dihapus (tersisa 2 data)**

![Hasil hapus](screenshots/04_delete_hasil.jpeg)

---

## Kesimpulan
Praktikum ini menghasilkan aplikasi CRUD sederhana untuk data barang dengan PHP dan MySQL. Aplikasi dapat menampilkan (Read), menambah (Create), mengubah (Update), dan menghapus (Delete) data melalui browser.
