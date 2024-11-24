# Lab8Web
Membuat Database
create table data_barang (
    id_barang int(10) auto_increment primary key,
    kategori varchar(30),
    nama varchar(30),
    gambar varchar(100),
    harga_beli decimal(10.0),
    harga_jual decimal(10,0),
    stok int(4));
  ![Screenshot (421)](https://github.com/user-attachments/assets/ef55eedc-df35-403a-bc84-fbbc96d889a9)

Menambahkan data

INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
![Screenshot (422)](https://github.com/user-attachments/assets/d14c7a46-99e6-483c-84eb-6865ffd7bd77)

Membuat file koneksi database
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
}else echo "Koneksi berhasil";
?>
![Screenshot (423)](https://github.com/user-attachments/assets/6444ba5f-8d12-406b-b30a-b7ce7c18d684)

menampilkan data(read)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Barang</title>
    <style>
        table {
            width: 100%;
            border-collapse: collapse;
        }
        table, th, td {
            border: 1px solid black;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
        a {
            text-decoration: none;
            color: blue;
        }
        a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <h1>Data Barang</h1>
    <a href="tambah_barang.php">Tambah Barang</a>
    <br><br>
    <table>
        <thead>
            <tr>
                <th>Gambar</th>
                <th>Nama Barang</th>
                <th>Kategori</th>
                <th>Harga Jual</th>
                <th>Harga Beli</th>
                <th>Stok</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody>
            <?php
            $data_barang = [
                [
                    "id" => 1,
                    "gambar" => "HP Samsung Android",
                    "nama" => "HP Samsung Android",
                    "kategori" => "Elektronik",
                    "harga_jual" => 2000000,
                    "harga_beli" => 2400000,
                    "stok" => 5
                ],
                [
                    "id" => 2,
                    "gambar" => "HP Xiaomi Android",
                    "nama" => "HP Xiaomi Android",
                    "kategori" => "Elektronik",
                    "harga_jual" => 1000000,
                    "harga_beli" => 1400000,
                    "stok" => 5
                ],
                [
                    "id" => 3,
                    "gambar" => "HP OPPO Android",
                    "nama" => "HP OPPO Android",
                    "kategori" => "Elektronik",
                    "harga_jual" => 1800000,
                    "harga_beli" => 2300000,
                    "stok" => 5
                ]
            ];

            foreach ($data_barang as $barang) {
                echo "<tr>
                        <td>{$barang['gambar']}</td>
                        <td>{$barang['nama']}</td>
                        <td>{$barang['kategori']}</td>
                        <td>{$barang['harga_jual']}</td>
                        <td>{$barang['harga_beli']}</td>
                        <td>{$barang['stok']}</td>
                        <td>
                            <a href='ubah.php?id={$barang['id']}'>Ubah</a> | 
                            <a href='hapus.php?id={$barang['id']}'>Hapus</a>
                        </td>
                    </tr>";
            }
            ?>
        </tbody>
    </table>
</body>
</html>

![Screenshot (424)](https://github.com/user-attachments/assets/b627621b-623d-4545-b268-ebd2f8225827)

Menambah data(create)
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit'])) {
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;

    if ($file_gambar['error'] == 0) {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(_FILE_) . '/gambar/' . $filename;

        if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
            $gambar = 'gambar/' . $filename;
        }
    }

    $sql = 'INSERT INTO data_barang (nama, kategori, harga_jual, harga_beli, stok, gambar) ';
    $sql .= "VALUE ('{$nama}', '{$kategori}', '{$harga_jual}', '{$harga_beli}', '{$stok}', '{$gambar}')";
    $result = mysqli_query($conn, $sql);

    if ($result) {
        header('Location: index.php');
        exit;
    } else {
        echo "Error: " . mysqli_error($conn);
    }
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Tambah Barang</title>
</head>
<body>
    <div class="container">
        <h1>Tambah Barang</h1>
        <div class="main">
            <form method="post" action="tambah.php" enctype="multipart/form-data">
                <table>
                    <tr>
                        <td><label>Nama Barang</label></td>
                        <td><input type="text" name="nama" required /></td>
                    </tr>
                    <tr>
                        <td><label>Kategori</label></td>
                        <td>
                            <select name="kategori" required>
                                <option value="Komputer">Komputer</option>
                                <option value="Elektronik">Elektronik</option>
                                <option value="Hand Phone">Hand Phone</option>
                            </select>
                        </td>
                    </tr>
                    <tr>
                        <td><label>Harga Jual</label></td>
                        <td><input type="number" name="harga_jual" required /></td>
                    </tr>
                    <tr>
                        <td><label>Harga Beli</label></td>
                        <td><input type="number" name="harga_beli" required /></td>
                    </tr>
                    <tr>
                        <td><label>Stok</label></td>
                        <td><input type="number" name="stok" required /></td>
                    </tr>
                    <tr>
                        <td><label>File Gambar</label></td>
                        <td><input type="file" name="file_gambar" /></td>
                    </tr>
                    <tr>
                        <td colspan="2" style="text-align: center;">
                            <input type="submit" name="submit" value="Simpan" />
                        </td>
                    </tr>
                </table>
            </form>
        </div>
    </div>
</body>
</html>

![Screenshot (426)](https://github.com/user-attachments/assets/f7f43cf9-d419-4c54-bfc5-f98335dc04e1)

Mengubah data
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit'])) {
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;

    if ($file_gambar['error'] == 0) {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(_FILE_) . '/gambar/' . $filename;

        if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
            $gambar = 'gambar/' . $filename;
        }
    }

    $sql = 'INSERT INTO data_barang (nama, kategori, harga_jual, harga_beli, stok, gambar) ';
    $sql .= "VALUE ('{$nama}', '{$kategori}', '{$harga_jual}', '{$harga_beli}', '{$stok}', '{$gambar}')";
    $result = mysqli_query($conn, $sql);

    if ($result) {
        header('Location: index.php');
        exit;
    } else {
        echo "Error: " . mysqli_error($conn);
    }
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Ubah Barang</title>
</head>
<body>
    <div class="container">
        <h1>Ubah Barang</h1>
        <div class="main">
            <form method="post" action="tambah.php" enctype="multipart/form-data">
                <table>
                    <tr>
                        <td><label>Nama Barang</label></td>
                        <td><input type="text" name="nama" required /></td>
                    </tr>
                    <tr>
                        <td><label>Kategori</label></td>
                        <td>
                            <select name="kategori" required>
                                <option value="Komputer">Komputer</option>
                                <option value="Elektronik">Elektronik</option>
                                <option value="Hand Phone">Hand Phone</option>
                            </select>
                        </td>
                    </tr>
                    <tr>
                        <td><label>Harga Jual</label></td>
                        <td><input type="number" name="harga_jual" required /></td>
                    </tr>
                    <tr>
                        <td><label>Harga Beli</label></td>
                        <td><input type="number" name="harga_beli" required /></td>
                    </tr>
                    <tr>
                        <td><label>Stok</label></td>
                        <td><input type="number" name="stok" required /></td>
                    </tr>
                    <tr>
                        <td><label>File Gambar</label></td>
                        <td><input type="file" name="file_gambar" /></td>
                    </tr>
                    <tr>
                        <td colspan="2" style="text-align: center;">
                            <input type="submit" name="submit" value="Simpan" />
                        </td>
                    </tr>
                </table>
            </form>
        </div>
    </div>
</body>
</html>

![Screenshot (429)](https://github.com/user-attachments/assets/f516b21c-070d-4d3b-86cf-9985d35106f7)

Menghapus data(delete)
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
