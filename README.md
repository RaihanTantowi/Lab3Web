# Lab3Web

# Tugas Praktikum 3 - PHP & Database MySQL
## Pemrograman Web 2

```sh
Nama   : Raihan Tantowi
Nim    : 312110229
Matkul : Pemrograman Web 2
```
#
### 1.  Membuat database dengan nama latihan1
* **Membuat tabel database :**

![Gambar 1](Screenshoot/ss1.png)

* **Masukan sql dari tabel database latihan1 :**

![Gambar 2](Screenshoot/ss2.png)

* **Berikut adalah sql tabel database latihan1 :**
```roomsql
CREATE TABLE data_barang (
 id_barang int(10) auto_increment Primary Key,
 kategori varchar(30),
 nama varchar(30),
 gambar varchar(100),
 harga_beli decimal(10,0),
 harga_jual decimal(10,0),
 stok int(4)
);
```

### 2.  Menambahkan data pada tabel *"data_barang"*
* **Masukan sql tersebut untuk menambahkan data pada tabel *data_barang* :**
```roomsql
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, 
stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 
2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```
![Gambar 3](Screenshoot/ss3.png)

### 3.  Membuat program *CRUD*
* **Selanjutnya buat folder lab3_php_database pada root directory web server (c:\xampp\htdocs) :**
![Gambar 4](Screenshoot/ss4.png)
Lalu untuk mengakses direktory tersebut pada web server dengan mengakses URL:
*http://localhost/lab3_php_database/*

### 4.  Membuat file koneksi database
**Buat file koneksi.php di dalam folder C:\xampp\htdocs\lab3_php_database untuk mengetahui koneksi server database akan terhubung atau tidaknya, jika terhubung maka akan ada keterangan "Koneksi Berhasil"**
* **Masukan kodingan tersebut pada file koneksi.php :**
```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";

$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
    echo "Koneksi ke server gagal.";
    die();
} else echo "Koneksi berhasil";
?>
``` 
![Gambar 5](Screenshoot/ss5.png)

Lalu Buka melalui browser untuk mengetahui koneksi database (untuk menampilkan pesan koneksi berhasil atau tidaknya).

![Gambar 6](Screenshoot/ss6.png)

### 5.  Membuat file index untuk menampilkan data (Read)
**Selanjutnya Buat file baru dengan nama index.php di dalam folder C:\xampp\htdocs\lab3_php_database**
* **Berikut adalah kodingan file index.php :**
```php
<?php
include("koneksi.php");
// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);
?>
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <link href="style.css" rel="stylesheet" type="text/css" />
 <title>Data Barang</title>
</head>
<body>
 <div class="container">
 <h1>Data Barang</h1>
 <div class="main">
    <a href="tambah.php">Tambah data</a>
 <table>
 <tr>
 <th>Gambar</th>
 <th>Nama Barang</th>
 <th>Katagori</th>
 <th>Harga Jual</th>
 <th>Harga Beli</th>
 <th>Stok</th>
 <th>Aksi</th>
 </tr>
 <?php if($result): ?>
 <?php while($row = mysqli_fetch_array($result)): ?>
 <tr>
 <td><img src="gambar/<?= $row['gambar'];?>" alt="<?=
$row['nama'];?>"></td>
 <td><?= $row['nama'];?></td>
 <td><?= $row['kategori'];?></td>
 <td><?= $row['harga_beli'];?></td>
 <td><?= $row['harga_jual'];?></td>
 <td><?= $row['stok'];?></td>
 <td><a href="ubah.php?id=<?=$row['id_barang'];?>">ubah data</a>
 <a href="hapus.php?id=<?=$row['id_barang'];?>">hapus data</a>
 </td>
 </tr>
 <?php endwhile; else: ?>
 <tr>
 <td colspan="7">Belum ada data</td>
 </tr>
 <?php endif; ?>
 </table>
 </div>
 </div>
</body>
</html>
```

* **Berikut adalah tampilan dari index.php**
![Gambar 7](Screenshoot/ss7.png)

### 6.  Menambahkan data (Create)
**Lalu buat file baru lagi dengan nama tambah.php di dalam folder C:\xampp\htdocs\lab3_php_database**
* **Berikut adalah kodingan file tambah.php :**
```php
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit']))
{
 $nama = $_POST['nama'];
 $kategori = $_POST['kategori'];
 $harga_jual = $_POST['harga_jual'];
 $harga_beli = $_POST['harga_beli'];
 $stok = $_POST['stok'];
 $file_gambar = $_FILES['file_gambar'];
 $gambar = null;
 if ($file_gambar['error'] == 0)
 {
 $filename = str_replace(' ', '_',$file_gambar['name']);
 $destination = dirname(__FILE__) .'/gambar/' . $filename;
 if(move_uploaded_file($file_gambar['tmp_name'], $destination))
 {
 $gambar = 'gambar/' . $filename;;
 }
}
$sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli, 
stok, gambar) ';
$sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}', 
'{$harga_beli}', '{$stok}', '{$gambar}')";
$result = mysqli_query($conn, $sql);
header('location: index.php');
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<link href="style.css" rel="stylesheet" type="text/css" />
<title>Tambah Barang</title>
</head>
<body>
<div class="container">
<h1>Tambah Barang</h1>
<div class="main">
<form method="post" action="tambah.php" enctype="multipart/formdata">
<div class="input">
<label>Nama Barang</label>
<input type="text" name="nama" />
</div>
<div class="input">
<label>Kategori</label>
<select name="kategori">
<option value="Komputer">Komputer</option>
<option value="Elektronik">Elektronik</option>
<option value="Hand Phone">Hand Phone</option>
</select>
</div>
<div class="input">
<label>Harga Jual</label>
<input type="text" name="harga_jual" />
</div>
<div class="input">
<label>Harga Beli</label>
<input type="text" name="harga_beli" />
</div>
<div class="input">
<label>Stok</label>
<input type="text" name="stok" />
</div>
<div class="input">
<label>File Gambar</label>
<input type="file" name="file_gambar" />
</div>
 <div class="submit">
 <input type="submit" name="submit" value="Simpan" />
 </div>
 </form>
 </div>
</div>
</body>
</html>
```
* **Berikut tampilan dari file tambah.php :**
![Gambar 8](Screenshoot/ss8.png)

### 7.  Mengubah Data (Update)
**Buat file baru lagi dengan nama ubah.php di dalam folder C:\xampp\htdocs\lab3_php_database**
* **Berikut adalah kodingan file ubah.php :**
```php
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit']))
{
 $id = $_POST['id'];
 $nama = $_POST['nama'];
 $kategori = $_POST['kategori'];
 $harga_jual = $_POST['harga_jual'];
 $harga_beli = $_POST['harga_beli'];
 $stok = $_POST['stok'];
 $file_gambar = $_FILES['file_gambar'];
 $gambar = null;
 
 if ($file_gambar['error'] == 0)
 {
    $filename = str_replace(' ', '_', $file_gambar['name']);
    $destination = dirname(__FILE__) . '/gambar/' . $filename;
    if (move_uploaded_file($file_gambar['tmp_name'], $destination))
    {
    $gambar = 'gambar/' . $filename;;
    }
    }
    $sql = 'UPDATE data_barang SET ';
    $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', 
   stok = '{$stok}' ";
    if (!empty($gambar))
    $sql .= ", gambar = '{$gambar}' ";
    $sql .= "WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);
    header('location: index.php');
   }
   $id = $_GET['id'];
   $sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
   $result = mysqli_query($conn, $sql);
   if (!$result) die('Error: Data tidak tersedia');
   $data = mysqli_fetch_array($result);
   function is_select($var, $val) {
    if ($var == $val) return 'selected="selected"';
    return false;
   }
   ?>
   <!DOCTYPE html>
   <html lang="en">
   <head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Ubah Barang</title>
   </head>
   <body>
   <div class="container">
    <h1>Ubah Barang</h1>
    <div class="main">
    <form method="post" action="ubah.php" enctype="multipart/formdata">
    <div class="input">
    <label>Nama Barang</label>
    <input type="text" name="nama" value="<?php echo 
   $data['nama'];?>" />
</div>
 <div class="input">
 <label>Kategori</label>
 <select name="kategori">
 <option <?php echo is_select
('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
 <option <?php echo is_select
('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
 <option <?php echo is_select
('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
 </select>
 </div>
 <div class="input">
 <label>Harga Jual</label>
 <input type="text" name="harga_jual" value="<?php echo 
$data['harga_jual'];?>" />
 </div>
 <div class="input">
 <label>Harga Beli</label>
 <input type="text" name="harga_beli" value="<?php echo 
$data['harga_beli'];?>" />
 </div>
 <div class="input">
 <label>Stok</label>
 <input type="text" name="stok" value="<?php echo 
$data['stok'];?>" />
 </div>
 <div class="input">
 <label>File Gambar</label>
 <input type="file" name="file_gambar" />
 </div>
 <div class="submit">
 <input type="hidden" name="id" value="<?php echo 
$data['id_barang'];?>" />
 <input type="submit" name="submit" value="Simpan" />
 </div>
 </form>
 </div>
</div>
</body>
</html>
```
* **Berikut tampilan dari file ubah.php :**
![Gambar 9](Screenshoot/ss9.png)

### 8.  Menghapus Data (Delete)
**Langkah terakhir buat file baru dengan nama hapus.php di dalam folder C:\xampp\htdocs\lab3_php_database**
* **Berikut adalah kodingan file hapus.php :**
```php
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```
* **Berikut adalah tampilan dari file hapus.php yang sebagian datanya sudah di hapus :**
  ![Gambar 10](Screenshoot/ss10.png)

