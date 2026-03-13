<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

include_once __DIR__ . "/../config/koneksi.php";
require_once __DIR__ . "/../interfaceandmodel/SistemAntrian.php";

use AntrianRS\PasienUmum;
use AntrianRS\PasienBPJS;

/* Menangkap data dari form edit */
$id_antrian   = $_POST['id_antrian'];
$nama_pasien  = $_POST['nama_pasien'];
$poli_tujuan  = $_POST['poli_tujuan'];
$jenis_pasien = $_POST['jenis_pasien'];

/* Hitung ulang biaya karena bisa jadi status pasien diubah (Umum ke BPJS atau sebaliknya) */
if ($jenis_pasien == "BPJS") {
    $pasien = new PasienBPJS();
} else {
    $pasien = new PasienUmum();
}
$biaya = $pasien->hitungBiaya();

/* Query Update */
$query = mysqli_query($conn, "UPDATE antrian SET nama_pasien='$nama_pasien', poli_tujuan='$poli_tujuan', jenis_pasien='$jenis_pasien', biaya='$biaya' WHERE id_antrian='$id_antrian'");

if($query) {
    echo "<script>
            alert('Sukses! Data pasien berhasil diperbarui.'); 
            window.location.href='../../html/public/dashboard.php';
          </script>";
} else {
    echo "<h1>GAGAL MENGEDIT DATA!</h1>";
    echo "Pesan Error: " . mysqli_error($conn);
}
?>