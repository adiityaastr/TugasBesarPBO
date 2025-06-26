# DOKUMENTASI SISTEM KASIR TOKO - BAGIAN 2
## Analisis GUI dan Penerapan OOP

---

## 🖥️ ANALISIS KOMPONEN GUI

### 1. Kelas Koneksi Database
**File**: `GUI/TokoKasirGUI/src/koneksi/Koneksi.java`

```java
public class Koneksi {
    public static Connection getConnection() throws SQLException {
        String url = "jdbc:mysql://localhost:3306/toko_kasir";
        String user = "root";
        String pass = "";
        return DriverManager.getConnection(url, user, pass);
    }
}
```

**Fungsi**:
- Menyediakan koneksi ke database MySQL
- Menggunakan JDBC driver
- Database: `toko_kasir` di localhost

### 2. Model Data

#### Kelas Barang (GUI Version)
**File**: `GUI/TokoKasirGUI/src/model/Barang.java`

```java
public class Barang {
    private int id;
    private String nama;
    private double harga;
    private int stok;
    // Getter dan Setter lengkap
}
```

**Perbedaan dengan CLI**:
- Menggunakan `double` untuk harga (lebih presisi)
- Memiliki setter method (untuk operasi CRUD)
- Tidak ada method abstrak

#### Kelas Transaksi (GUI Version)
**File**: `GUI/TokoKasirGUI/src/model/Transaksi.java`

```java
public class Transaksi {
    private int id;
    private Date tanggal;
    private double totalHarga;
    private int jumlahItem;
    // Getter dan Setter lengkap
}
```

**Fitur**:
- Menyimpan tanggal transaksi
- Menghitung jumlah item
- Total harga dalam format double

#### Kelas DetailTransaksi
**File**: `GUI/TokoKasirGUI/src/model/DetailTransaksi.java`

**Status**: Kelas kosong (belum diimplementasikan)

### 3. View Components

#### FormBarang
**File**: `GUI/TokoKasirGUI/src/view/FormBarang.java`

**Fitur**:
- CRUD operasi untuk barang
- Tabel untuk menampilkan data
- Form input: nama, harga, stok
- Tombol: Tambah, Edit, Hapus

**Komponen UI**:
- JTextField untuk input
- JTable untuk menampilkan data
- JButton untuk aksi
- JLabel untuk label

**Fungsi Database**:
```java
// Tampil data
SELECT * FROM barang

// Insert data
INSERT INTO barang (nama, harga, stok) VALUES (?, ?, ?)

// Update data
UPDATE barang SET nama=?, harga=?, stok=? WHERE id=?

// Delete data
DELETE FROM barang WHERE id=?
```

#### FromTransaksi
**File**: `GUI/TokoKasirGUI/src/view/FromTransaksi.java`

**Status**: Form dasar (belum diimplementasikan lengkap)

**Komponen**:
- JComboBox untuk pilih barang
- JTextField untuk jumlah
- JTable untuk keranjang belanja
- Label untuk total harga

### 4. Main GUI Class
**File**: `GUI/TokoKasirGUI/src/tokokasirgui/TokoKasirGUI.java`

**Status**: Kelas kosong (belum diimplementasikan)

---

## 🏗️ PENERAPAN 4 PILAR OOP

### 1. ENKAPSULASI (Encapsulation)

**Definisi**: Membungkus data dan method dalam satu unit (kelas) dan membatasi akses langsung ke data.

**Penerapan dalam Proyek**:

#### CLI Version:
```java
// Barang.java
private int id;
private String nama;
private int harga;
private int stok;

public int getId() { return id; }
public String getNama() { return nama; }
public int getHarga() { return harga; }
public int getStok() { return stok; }
```

#### GUI Version:
```java
// Barang.java (GUI)
private int id;
private String nama;
private double harga;
private int stok;

// Getter dan Setter lengkap
public int getId() { return id; }
public void setId(int id) { this.id = id; }
// ... setter lainnya
```

**Keuntungan**:
- Data terlindungi dari akses langsung
- Validasi dapat ditambahkan di setter
- Fleksibilitas dalam mengubah implementasi internal

### 2. PEWARISAN (Inheritance)

**Definisi**: Kelas dapat mewarisi atribut dan method dari kelas lain.

**Penerapan dalam Proyek**:

```java
// Hierarki kelas
Barang (abstract)
├── BarangSembako
└── BarangElektronik

// Implementasi
public class BarangSembako extends Barang {
    public BarangSembako(String nama, int harga, int stok) {
        super(nama, harga, stok); // Memanggil constructor parent
    }
}
```

**Keuntungan**:
- Code reusability
- Hierarki yang jelas
- Polymorphism memungkinkan

### 3. POLIMORFISME (Polymorphism)

**Definisi**: Kemampuan objek untuk memiliki banyak bentuk.

**Penerapan dalam Proyek**:

#### Method Overriding:
```java
// Barang.java (abstract)
public abstract void tampilkan();

// BarangSembako.java
@Override
public void tampilkan() {
    System.out.printf("| %-2d | %-12s | %-6d | %-5d | Jenis: Sembako\n",
        getId(), getNama(), getHarga(), getStok());
}

// BarangElektronik.java
@Override
public void tampilkan() {
    System.out.printf("| %-2d | %-12s | %-6d | %-5d | Jenis: Elektronik\n",
        getId(), getNama(), getHarga(), getStok());
}
```

#### Polymorphic Behavior:
```java
// TokoKasir.java
for (Barang b : daftarBarang) {
    b.tampilkan(); // Polimorfisme: tampilkan sesuai jenisnya
}
```

**Keuntungan**:
- Fleksibilitas dalam pemanggilan method
- Extensibility yang baik
- Code yang lebih clean

### 4. ABSTRAKSI (Abstraction)

**Definisi**: Menyembunyikan detail implementasi dan menampilkan fungsionalitas penting.

**Penerapan dalam Proyek**:

```java
// Kelas abstrak
public abstract class Barang {
    // Atribut dan method konkret
    private int id;
    private String nama;
    private int harga;
    private int stok;
    
    public int getId() { return id; }
    public void kurangiStok(int jumlah) { this.stok -= jumlah; }
    
    // Method abstrak - harus diimplementasikan subclass
    public abstract void tampilkan();
}
```

**Keuntungan**:
- Interface yang konsisten
- Memaksa implementasi method tertentu
- Mengurangi kompleksitas

---

## 🏛️ ARSITEKTUR SISTEM

### 1. Arsitektur CLI Version

```
┌─────────────────┐
│   TokoKasir     │ ← Main Class (Controller)
│   (Main)        │
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│   Transaksi     │ ← Business Logic
│   TransaksiItem │
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│   Barang        │ ← Data Model (Abstract)
│   ├─ Sembako    │
│   └─ Elektronik │
└─────────────────┘
```

### 2. Arsitektur GUI Version

```
┌─────────────────┐
│   View Layer    │ ← FormBarang, FromTransaksi
│   (Swing UI)    │
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│   Model Layer   │ ← Barang, Transaksi, DetailTransaksi
│   (Data Class)  │
└─────────┬───────┘
          │
          ▼
┌─────────────────┐
│   Data Layer    │ ← Koneksi (MySQL)
│   (Database)    │
└─────────────────┘
```

### 3. Design Patterns yang Digunakan

#### MVC Pattern (GUI Version):
- **Model**: Kelas Barang, Transaksi, DetailTransaksi
- **View**: FormBarang, FromTransaksi
- **Controller**: Logic dalam event handler

#### Factory Pattern (CLI Version):
- Pembuatan objek Barang berdasarkan jenis

---

## 🗄️ DATABASE DESIGN

### 1. Struktur Database

**Database**: `toko_kasir`

#### Tabel: `barang`
```sql
CREATE TABLE barang (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nama VARCHAR(100) NOT NULL,
    harga DOUBLE NOT NULL,
    stok INT NOT NULL
);
```

#### Tabel: `transaksi` (Direncanakan)
```sql
CREATE TABLE transaksi (
    id INT PRIMARY KEY AUTO_INCREMENT,
    tanggal DATETIME DEFAULT CURRENT_TIMESTAMP,
    total_harga DOUBLE NOT NULL,
    jumlah_item INT NOT NULL
);
```

#### Tabel: `detail_transaksi` (Direncanakan)
```sql
CREATE TABLE detail_transaksi (
    id INT PRIMARY KEY AUTO_INCREMENT,
    transaksi_id INT,
    barang_id INT,
    jumlah INT,
    harga_satuan DOUBLE,
    subtotal DOUBLE,
    FOREIGN KEY (transaksi_id) REFERENCES transaksi(id),
    FOREIGN KEY (barang_id) REFERENCES barang(id)
);
```

### 2. Relasi Database

```
transaksi (1) ──── (N) detail_transaksi
barang (1) ─────── (N) detail_transaksi
``` 