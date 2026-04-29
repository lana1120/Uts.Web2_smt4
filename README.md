# Ujian Tengah Semester

- **Nama : Maulana Malik Ibrahim**
- **NIM : 312410185**
- **Kelas : I241B**
- **Mata Kuliah : Pemrograman Web2**
- **Program Studi : Teknik Informatika**
- **Dosen Pengampu : Agung Nugroho, S.Kom., M.Kom.**
  
  ---
  
 # Eksperimen Keamanan Web: Simulasi SQL Injection (Bypass Login)

Repositori ini dibuat untuk memenuhi tugas UTS mata kuliah **Pemrograman Web 2**. Proyek ini mendemonstrasikan bagaimana celah keamanan SQL Injection dapat dieksploitasi pada halaman login sederhana dan bagaimana cara mengatasinya menggunakan *Prepared Statements*.


## Deskripsi Proyek
Proyek ini berisi simulasi serangan **SQL Injection (SQLi)** jenis *Tautology* pada form login berbasis PHP. Eksperimen ini bertujuan untuk memahami kerentanan "data-code confusion" di mana input pengguna dieksekusi sebagai perintah oleh database.

## Teknologi yang Digunakan
* **Bahasa**: PHP 8.x
* **Database**: MySQL / MariaDB
* **Server**: Apache (via XAMPP)
* **Editor**: Visual Studio Code

---

## Struktur File

```
uts-pemweb2-sqli/
├── assets/
│   └── img/                # Menyimpan gambar screenshot eksperimen
│       ├── db_setup.png
│       ├── login_normal.png
│       └── sqli_attack.png
├── database/
│   └── db_setup.sql        # Script SQL untuk konfigurasi database
├── src/
│   ├── koneksi.php         # File konfigurasi koneksi database
│   └── index.php           # Halaman login utama (Vulnerable Code)
└── README.md               # Dokumentasi proyek
```
---

## Langkah Eksperimen

### 1. Persiapan Database
Jalankan query berikut di phpMyAdmin untuk menyiapkan data:
```sql
CREATE DATABASE db_keamanan_uts;
USE db_keamanan_uts;

CREATE TABLE pengguna (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    password VARCHAR(50) NOT NULL
);

INSERT INTO pengguna (username, password) VALUES ('admin', 'admin123'), ('maulana', 'rahasia123');
```

### 2. Analisis Kode Rentan
Celah keamanan terletak pada penggabungan variabel input langsung ke dalam string query SQL:

PHP
$sql = "SELECT * FROM pengguna WHERE username = '$username' AND password = '$password'";
3. Skenario Serangan
Payload yang digunakan untuk melakukan bypass login tanpa password:

Username: ' OR '1'='1

Password: (kosongkan)

Hasil: Database mengevaluasi '1'='1' sebagai TRUE sehingga akses diberikan meskipun password tidak valid.

---

## Solusi Keamanan (Mitigasi)
Untuk mencegah serangan ini, kode harus diubah menggunakan Prepared Statements agar input pengguna dianggap sebagai data literal, bukan instruksi SQL:
```
PHP
$stmt = $conn->prepare("SELECT id, username FROM pengguna WHERE username = ? AND password = ?");
$stmt->bind_param("ss", $username, $password); 
$stmt->execute();
```

---

## Bukti Eksperimen
(Silakan unggah gambar screenshot Anda ke folder 'img' di repositori ini dan tautkan di bawah ini)
<img width="958" height="1001" alt="Cuplikan layar 2026-04-29 232532" src="https://github.com/user-attachments/assets/0504b5b6-fd5a-4fc1-b78d-d6af92102e93" />
<img width="958" height="1001" alt="Cuplikan layar 2026-04-29 232532" src="https://github.com/user-attachments/assets/d08369a7-e678-45cd-a014-4691a0616ab7" />
<img width="1920" height="1200" alt="Cuplikan layar 2026-04-29 234516" src="https://github.com/user-attachments/assets/0bfc5b36-c512-4bfb-9c75-0fb2edb51206" />


---

## Referensi

1. OWASP Foundation. (2021). OWASP Top 10:2021 - A03:2021 Injection.
2. Gustiyono, A., et al. (2024). Analisa kerentanan website terhadap serangan siber. Jurnal Informatika dan Keamanan Siber.
3. The PHP Group. (2024). PHP Manual: mysqli::prepare documentation.
4. Sulistiyani, E. (2026). Analisis keamanan database pada aplikasi berbasis web. Jurnal Teknologi Informasi.

# Bukti Hasil Plagiasi 

<img width="1600" height="732" alt="plagiasi artikel" src="https://github.com/user-attachments/assets/cec20e26-fd92-4ea2-a2a5-84165f1a5685" />


