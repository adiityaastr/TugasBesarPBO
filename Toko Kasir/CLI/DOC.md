# Dokumentasi Program Kasir Toko (OOP)

## Deskripsi Program
Program ini adalah aplikasi kasir sederhana berbasis CLI (Command Line Interface) untuk toko. Pengguna dapat menambah barang (sembako/elektronik), melihat daftar barang, melakukan transaksi penjualan, dan melihat laporan penjualan harian. Program ini dibuat menggunakan prinsip-prinsip OOP (Object Oriented Programming) di Java.

## Penjelasan Kelas
- **Barang (abstract)**: Kelas dasar untuk semua barang di toko. Memiliki atribut id, nama, harga, stok, dan method abstrak `tampilkan()`.
- **BarangSembako**: Turunan dari `Barang` untuk barang jenis sembako. Override method `tampilkan()`.
- **BarangElektronik**: Turunan dari `Barang` untuk barang elektronik. Override method `tampilkan()`.
- **TransaksiItem**: Mewakili satu item transaksi (barang + jumlah). Digunakan dalam transaksi penjualan.
- **Transaksi**: Menyimpan daftar item yang dibeli dalam satu transaksi, serta total harga. Ada method untuk menambah item dan menampilkan transaksi.
- **TokoKasir**: Kelas utama (main) yang menjalankan menu, mengelola daftar barang, transaksi, dan laporan.

## Contoh Output Program
```
========================
SISTEM KASIR TOKO
========================
1. Tambah Barang
2. Lihat Barang
3. Transaksi Penjualan
4. Laporan Penjualan
5. Keluar
Pilih menu: 1
Jenis barang:
1. Sembako
2. Elektronik
Pilih jenis: 1
Nama barang: Beras
Harga: 12000
Stok: 50
Barang berhasil ditambahkan!

Pilih menu: 2
+----+--------------+--------+-------+--------------------------+
| ID | Nama         | Harga  | Stok  | Keterangan               |
+----+--------------+--------+-------+--------------------------+
| 1  | Beras        | 12000  | 50    | Jenis: Sembako
+----+--------------+--------+-------+--------------------------+

Pilih menu: 3
Masukkan ID barang: 1
Masukkan jumlah: 2
Nama Barang : Beras
Harga Satuan : Rp 12000
Subtotal : Rp 24000
Konfirmasi transaksi? (y/n): y
Stok berhasil dikurangi.
Transaksi berhasil disimpan!

Pilih menu: 4
Laporan Transaksi Penjualan
Transaksi 1:
- Beras: 2 pcs
Total: Rp 24000
---------------------------------
Total pendapatan hari ini: Rp 24000
```

## Penerapan 4 Pilar OOP
- **Enkapsulasi**: Atribut pada setiap kelas dibuat private, akses melalui getter/setter/method publik.
- **Pewarisan**: `BarangSembako` dan `BarangElektronik` mewarisi dari kelas abstrak `Barang`.
- **Polimorfisme**: Method `tampilkan()` di-override pada setiap turunan `Barang`, dan dipanggil secara polimorfik di daftar barang.
- **Abstraksi**: Kelas `Barang` bersifat abstrak dan memiliki method abstrak `tampilkan()` yang harus diimplementasikan oleh kelas turunannya. 