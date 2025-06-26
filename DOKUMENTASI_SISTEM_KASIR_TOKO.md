# DOKUMENTASI LENGKAP SISTEM KASIR TOKO
## Tugas Besar Pemrograman Berorientasi Objek (PBO)

---

## 📋 DAFTAR ISI
1. [Informasi Umum](#informasi-umum)
2. [Struktur Proyek](#struktur-proyek)
3. [Analisis Komponen CLI](#analisis-komponen-cli)
4. [Analisis Komponen GUI](#analisis-komponen-gui)
5. [Penerapan 4 Pilar OOP](#penerapan-4-pilar-oop)
6. [Arsitektur Sistem](#arsitektur-sistem)
7. [Database Design](#database-design)
8. [User Interface Design](#user-interface-design)
9. [Alur Program](#alur-program)
10. [Fitur-fitur](#fitur-fitur)
11. [Teknologi yang Digunakan](#teknologi-yang-digunakan)
12. [Cara Menjalankan Program](#cara-menjalankan-program)
13. [Kesimpulan](#kesimpulan)

---

## 📖 INFORMASI UMUM

### Deskripsi Proyek
Sistem Kasir Toko adalah aplikasi manajemen toko yang dibangun menggunakan konsep Object Oriented Programming (OOP) dalam bahasa Java. Sistem ini terdiri dari dua versi:
1. **Versi CLI (Command Line Interface)** - Aplikasi berbasis teks dengan fitur lengkap
2. **Versi GUI (Graphical User Interface)** - Aplikasi berbasis desktop dengan database MySQL

### Tujuan Pengembangan
- Menerapkan konsep 4 pilar OOP (Encapsulation, Inheritance, Polymorphism, Abstraction)
- Membuat sistem manajemen toko yang efisien
- Menyediakan interface yang user-friendly
- Mengelola data barang dan transaksi penjualan

### Tim Pengembang
- **Mata Kuliah**: Pemrograman Berorientasi Objek
- **Institusi**: UNDIRA
- **Tahun**: 2024

---

## 📁 STRUKTUR PROYEK

```
TB PBO/
├── Toko Kasir/
│   ├── CLI/                          # Versi Command Line Interface
│   │   ├── Barang.java              # Kelas abstrak untuk barang
│   │   ├── BarangElektronik.java    # Kelas turunan untuk barang elektronik
│   │   ├── BarangSembako.java       # Kelas turunan untuk barang sembako
│   │   ├── Transaksi.java           # Kelas untuk mengelola transaksi
│   │   ├── TransaksiItem.java       # Kelas untuk item dalam transaksi
│   │   ├── TokoKasir.java           # Kelas utama program CLI
│   │   ├── DOC.md                   # Dokumentasi CLI
│   │   └── *.class                  # File bytecode Java
│   │
│   ├── GUI/                         # Versi Graphical User Interface
│   │   └── TokoKasirGUI/
│   │       ├── src/
│   │       │   ├── koneksi/
│   │       │   │   └── Koneksi.java # Kelas koneksi database
│   │       │   ├── model/
│   │       │   │   ├── Barang.java  # Model data barang
│   │       │   │   ├── Transaksi.java # Model data transaksi
│   │       │   │   └── DetailTransaksi.java # Model detail transaksi
│   │       │   ├── tokokasirgui/
│   │       │   │   └── TokoKasirGUI.java # Kelas utama GUI
│   │       │   └── view/
│   │       │       ├── FormBarang.java # Form manajemen barang
│   │       │       ├── FormBarang.form # File form NetBeans
│   │       │       ├── FromTransaksi.java # Form transaksi
│   │       │       └── FromTransaksi.form # File form NetBeans
│   │       ├── nbproject/           # Konfigurasi NetBeans
│   │       ├── build.xml            # File build Ant
│   │       └── manifest.mf          # Manifest file
│   │
│   └── Documents/                   # Dokumentasi tambahan
│       ├── TokoKasir.pdf           # Dokumentasi PDF
│       └── TokoKasir.pptx          # Presentasi PowerPoint
│
└── Tugas Besar PBO Sabtu.pdf       # Spesifikasi tugas
```

---

## 💻 ANALISIS KOMPONEN CLI

### 1. Kelas Barang (Abstract Class)
**File**: `CLI/Barang.java`

```java
public abstract class Barang {
    private static int counter = 1;
    private int id;
    private String nama;
    private int harga;
    private int stok;
    
    // Constructor, getter, setter, dan method abstrak
    public abstract void tampilkan();
}
```

**Fungsi**:
- Kelas abstrak yang menjadi parent untuk semua jenis barang
- Menggunakan static counter untuk auto-increment ID
- Menyimpan data dasar: id, nama, harga, stok
- Memiliki method abstrak `tampilkan()` yang harus diimplementasikan subclass

**Penerapan OOP**:
- **Abstraction**: Kelas abstrak dengan method abstrak
- **Encapsulation**: Atribut private dengan getter/setter

### 2. Kelas BarangSembako
**File**: `CLI/BarangSembako.java`

```java
public class BarangSembako extends Barang {
    @Override
    public void tampilkan() {
        System.out.printf("| %-2d | %-12s | %-6d | %-5d | Jenis: Sembako\n",
            getId(), getNama(), getHarga(), getStok());
    }
}
```

**Fungsi**:
- Kelas turunan dari Barang untuk barang sembako
- Mengimplementasikan method `tampilkan()` dengan format khusus

**Penerapan OOP**:
- **Inheritance**: Mewarisi dari kelas Barang
- **Polymorphism**: Override method tampilkan()

### 3. Kelas BarangElektronik
**File**: `CLI/BarangElektronik.java`

```java
public class BarangElektronik extends Barang {
    @Override
    public void tampilkan() {
        System.out.printf("| %-2d | %-12s | %-6d | %-5d | Jenis: Elektronik\n",
            getId(), getNama(), getHarga(), getStok());
    }
}
```

**Fungsi**:
- Kelas turunan dari Barang untuk barang elektronik
- Mengimplementasikan method `tampilkan()` dengan format khusus

### 4. Kelas TransaksiItem
**File**: `CLI/TransaksiItem.java`

```java
public class TransaksiItem {
    private Barang barang;
    private int jumlah;
    
    public int getSubtotal() { 
        return barang.getHarga() * jumlah; 
    }
}
```

**Fungsi**:
- Mewakili satu item dalam transaksi
- Menyimpan referensi ke objek Barang dan jumlah
- Menghitung subtotal otomatis

### 5. Kelas Transaksi
**File**: `CLI/Transaksi.java`

```java
public class Transaksi {
    private List<TransaksiItem> items = new ArrayList<>();
    private int total = 0;
    
    public void tambahItem(TransaksiItem item) {
        items.add(item);
        total += item.getSubtotal();
        item.getBarang().kurangiStok(item.getJumlah());
    }
}
```

**Fungsi**:
- Mengelola daftar item dalam satu transaksi
- Menghitung total otomatis
- Mengurangi stok barang secara otomatis

### 6. Kelas TokoKasir (Main Class)
**File**: `CLI/TokoKasir.java`

**Fitur Utama**:
- Menu interaktif dengan 5 pilihan
- Manajemen daftar barang (ArrayList)
- Sistem transaksi penjualan
- Laporan penjualan harian
- Pencarian barang berdasarkan ID

**Menu Program**:
```
1. Tambah Barang
2. Lihat Barang  
3. Transaksi Penjualan
4. Laporan Penjualan
5. Keluar
``` 