  <?php
// koneksi ke database
$host = "localhost";
$user = "root";
$pass = "";
$db = "akademik";
$koneksi = mysqli_connect($host, $user, $pass, $db);

// penanganan masalah koneksi
if (!$koneksi) {
    die("Koneksi gagal: " . mysqli_connect_error());
} else {
    echo "koneksi berhasil";
}
$nim = "";
$nama = "";
$jurusan = "";
$error = "";
$sukses = "";

// untuk menambah data mahasiswa
if (isset($_GET["op"])) {
    $op = $_GET["op"];
} else {
    $op = "";
}
// menghapus data mahasiswa
if ($op == 'delete') {
    $id = $_GET['id'];
    $sql1 = "DELETE FROM mahasiswa1 WHERE id = '$id'";
    $q1 = mysqli_query($koneksi, $sql1);
    if ($q1) {
        $sukses = "Berhasil di hapus";
    } else {
        $error = "gagal untuk di hapus";
    }
}
// untuk mengedit data mahasiswa
if ($op == 'edit') {
    if (isset($_GET['id'])) {
        $id = $_GET['id'];
        // Perbaikan: Query SQL dengan SELECT *
        $sql1 = "SELECT * FROM mahasiswa1 WHERE id = '$id'";
        $q1 = mysqli_query($koneksi, $sql1);
        if ($q1 && mysqli_num_rows($q1) > 0) {
            $r1 = mysqli_fetch_array($q1);
            $nim = $r1["nim"];
            $nama = $r1["nama"];
            $jurusan = $r1["jurusan"];
        } else {
            $error = "Data tidak ditemukan";
        }
    } else {
        $error = "ID tidak valid";
    }
}

// memasukkan data mahasiswa
if (isset($_POST["simpan"])) {
    $nim = $_POST['nim'];
    $nama = $_POST['nama'];
    $jurusan = $_POST['jurusan'];

    if ($nim && $nama && $jurusan) {
        if ($op == 'edit') {
            if (isset($_GET['id'])) {
                $id = $_GET['id'];
                // Perbaikan: Gunakan prepared statement untuk UPDATE
                $sql_update = "UPDATE mahasiswa1 SET nim = ?, nama = ?, jurusan = ? WHERE id = ?";
                $stmt = mysqli_prepare($koneksi, $sql_update);
                mysqli_stmt_bind_param($stmt, "sssi", $nim, $nama, $jurusan, $id);
                $q_update = mysqli_stmt_execute($stmt);
                if ($q_update) {
                    $sukses = "Data berhasil diupdate";
                } else {
                    $error = "Data gagal diupdate";
                }
            } else {
                $error = "ID tidak valid";
            }
        } else {
            // Perbaikan: Gunakan prepared statement untuk INSERT
            $sql_insert = "INSERT INTO mahasiswa1 (nim, nama, jurusan) VALUES (?, ?, ?)";
            $stmt = mysqli_prepare($koneksi, $sql_insert);
            mysqli_stmt_bind_param($stmt, "sss", $nim, $nama, $jurusan);
            $q_insert = mysqli_stmt_execute($stmt);
            if ($q_insert) {
                $sukses = "Data berhasil ditambahkan";
            } else {
                $error = "Data gagal ditambahkan";
            }
        }
    } else {
        $error = "Silakan lengkapi semua data";
    }
}
?>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Mahasiswa</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
    <style>
        .mx-auto {
            width: 800px;
        }

        .card {
            margin-top: 10px;
        }
    </style>
</head>

<body>
    <div class="mx-auto">
        <!-- untuk menambah data -->
        <div class="card">
            <div class="card-header">
                Creat / Buat
            </div>
            <div class="card-body">
                <?php
                if ($error) {
                ?>
                    <div class="alert alert-danger" role="alert">
                        <?php echo $error ?>
                    </div>
                <?php
                    header("refresh:4;url=index.php");
                }
                ?>
                <?php
                if ($sukses) {
                ?>
                    <div class="alert alert-success" role="alert">
                        <?php echo $sukses ?>
                    </div>
                <?php
                    header("refresh:4;url=index.php");
                }
                ?>


                <form action="" method="post">

                    <div class="mb-3">
                        <label for="nim" class="form-label">NIM</label>
                        <input type="text" class="form-control" id="nim" name="nim" value="<?php echo $nim ?>" placeholder="22XXXXXXXXX">
                    </div>
                    <div class="mb-3">
                        <label for="nama" class="form-label">Nama</label>
                        <input type="text" class="form-control" id="nama" name="nama" value="<?php echo $nama ?>" placeholder="Jhon Doe">
                    </div>
                    <div class="mb-3">
                        <label for="jurusan" class="form-label">Jurusan</label>

                        <select class="form-control" name="jurusan" id="jurusan">
                            <option value="">- Pilih Fakultas -</option>
                            <option value="manajemen" <?php if ($jurusan == "manajemen") echo "selected" ?>>- Manajemen -</option>
                            <option value="akuntansi" <?php if ($jurusan == "akuntansi") echo "selected" ?>>- Akuntansi -</option>
                            <option value="informatika" <?php if ($jurusan == "informatika") echo "selected" ?>>- Informatika -</option>
                        </select>
                    </div>
                    <div class="col-12">
                        <input type="submit" name="simpan" value="Simpan Data" class="btn btn-primary">
                    </div>
                </form>
            </div>
        </div>
        <!-- untuk mengeluarkan data -->
        <div class="card">
            <div class="card-header text-white bg-secondary">
                Data Mahasiswa
            </div>
            <div class="card-body">
                <table class="table">
                    <thead>
                        <tr>
                            <th scope="col">#</th>
                            <th scope="col">NIM</th>
                            <th scope="col">Nama</th>
                            <th scope="col">Jurusan</th>
                            <th scope="col">Aksi</th>
                        </tr>
                    <tbody>
                        <?php
                        $sql2 = "SELECT * FROM mahasiswa1 ORDER by id ASC";
                        $q2 = mysqli_query($koneksi, $sql2);
                        $urut = 1;
                        // loop untuk menampilkan data mahasiswa
                        while ($r2 = mysqli_fetch_array($q2)) {
                            $id = $r2["id"];
                            $nim = $r2["nim"];
                            $nama = $r2["nama"];
                            $jurusan = $r2["jurusan"];

                        ?>
                            <tr>
                                <th scope="row"> <?php echo $urut++ ?>
                                <td scope="row"><?php echo $nim ?></td>
                                <td scope="row"><?php echo $nama ?></td>
                                <td scope="row"><?php echo $jurusan ?></td>
                                <td scope="row">
                                    <a href="index.php?op=edit&id=<?php echo $id ?>"> <button type="button" class="btn btn-warning">Edit</button>
                                    </a>
                                    <!-- Tambahkan konfirmasi sebelum menghapus -->
                                    <a href="index.php?op=delete&id=<?php echo $id ?>" onclick="return confirm('Apakah Anda yakin ingin menghapus data?')"><button type="button" class="btn btn-danger">Delete</button></a>
                                </td>
                            </tr>
                        <?php
                        }
                        ?>
                    </tbody>
                    </thead>
                </table>


                </form>
            </div>
        </div>
    </div>
</body>

</html>
