# Audit Keamanan Sistem Operasi Unix/Linux (Proxmox VE) pada Rumah Sakit Umum XYZ

Repositori ini memuat hasil evaluasi dan dokumentasi program kerja **IT Auditing** yang berfokus pada audit sistem operasi berbasis Unix/Linux (sistem *.nix*). Studi kasus dilakukan pada infrastruktur server **Proxmox Virtual Environment (Proxmox VE)** yang berbasis Debian Linux di Perusahaan Rumah Sakit Umum XYZ.

---

## 📌 Latar Belakang Perusahaan
* **Profil Institusi:** Rumah Sakit Umum XYZ merupakan penyedia layanan kesehatan masyarakat skala menengah hingga besar dengan volume rata-rata 500–700 pasien per hari.
* **Infrastruktur IT:** Mengimplementasikan Sistem Informasi Manajemen Rumah Sakit (SIMRS) dan Rekam Medis Elektronik (RME). Operasional utama didukung oleh server berbasis Unix/Linux untuk menjamin stabilitas, efisiensi, dan keamanan.
* **Tantangan Utama:** Mengamankan data sensitif pasien (Electronic Medical Record/EMR) sesuai dengan kepatuhan standar internasional seperti **ISO 27001** dan **HIPAA** di tengah keterbatasan anggaran.

---

## 🛠️ Cakupan Audit & Metodologi
Proses audit mengacu pada standar kontrol organisasi dan dibagi ke dalam 4 pilar utama:
1. **Account Management**
2. **Permissions Management**
3. **Network Security and Controls**
4. **Security Monitoring and Other General Controls**

### Peralatan & Komponen Evaluasi (Tools Used)
* **Analisis Teks & Skrip:** Memanfaatkan utilitas Unix seperti `awk`, `sed`, `grep`, dan perintah internal shell untuk otomatisasi pengecekan konfigurasi.
* **Referensi Standar:** Kerangka kerja pengujian mengacu pada panduan dari ISACA, SANS Institute, Center for Internet Security (CIS) Top 20, NSA, dan NIST.

---

## 📊 Ringkasan Hasil Evaluasi (Audit Summary)

Dari total **43 poin kontrol** yang diperiksa berdasarkan baseline keamanan standar:
* 🟢 **30 Poin** Terpenuhi sepenuhnya (Aman / Sesuai kriteria standard bawaan).
* 🟡 **7 Poin** Terpenuhi Sebagian (*Partially Compliant*).
* 🔴 **6 Poin** Belum Terpenuhi (*Non-Compliant*).

### Kesenjangan Keamanan Teridentifikasi (Security Gaps)

#### 1. Masalah Kepatuhan Sebagian (*Partially Compliant*)
* **Permission Berisiko:** File `/etc/shadow` ber-permission `-rw-r-----` (bawaan Proxmox) dan `/var/run/utmp` masih mengizinkan hak akses *write* bagi Group.
* **Autentikasi & Akun:** Autentikasi berbasis SSH keys sudah menggunakan cipher modern, tetapi pengaturan `PasswordAuthentication` masih aktif (`yes`). Panjang password sistem perlu ditingkatkan dan belum ada implementasi Multi-Factor Authentication (MFA).
* **Akses Root Bersama:** Akun superuser `root` Proxmox VE saat ini digunakan bersama (*shared account*) oleh dua orang staf IT, sehingga mengurangi faktor *traceability* individu.
* **Retensi Log:** Retensi konfigurasi log saat ini masih berdurasi pendek (4 minggu), belum memenuhi standar kepatuhan jangka panjang.

#### 2. Masalah Belum Terpenuhi (*Non-Compliant*)
* **Password Controls:** Kontrol penuaan kata sandi (*password aging*) belum diterapkan.
* **Vulnerability Scanning:** Pemindaian kerentanan jaringan secara berkala belum berjalan karena risiko gangguan operasional layanan klinis.
* **Banners:** Belum menampilkan *legal warning banner* khusus saat user melakukan koneksi remote ke server.
* **Auditing & Monitoring:** Fungsi logging untuk command sensitif (`su` dan `sudo`) tidak terimplementasi karena ketiadaan service `rsyslog` / `syslog` yang memadai. Monitoring proaktif (IDS/IPS) juga belum diaktifkan.

---

## 🚀 Rekomendasi Tindakan (Action Plan)

| ID Kontrol | Komponen | Rekomendasi Teknis |
| :--- | :--- | :--- |
| **Kontrol 11** | Akun Pengguna | Membuat akun khusus individual bagi masing-masing administrator untuk menggantikan penggunaan *shared root account* demi pemenuhan aspek akuntabilitas. |
| **Kontrol 4 & 40** | Hak Akses File | Melakukan pengujian di lingkungan *staging/development* untuk mengubah permission `/etc/shadow` dan `/var/run/utmp` guna memastikan pengetatan tidak merusak fungsi bawaan Proxmox. |
| **Kontrol 5 & 6** | Autentikasi | Mewajibkan kebijakan password yang lebih panjang, mengimplementasikan *MFA*, serta mengaktifkan skema *password aging*. |
| **Kontrol 28** | Asesmen Jaringan | Menyusun jadwal pemeliharaan khusus bersama Kepala IT untuk melakukan *Network Vulnerability Scanning* di luar jam operasional padat rumah sakit. |
| **Kontrol 30** | Pengamanan SSH | Mengubah parameter konfigurasi `PasswordAuthentication no` pada SSH daemon demi memaksakan login berbasis kunci kriptografi saja. |
| **Kontrol 35** | Warning Banner | Membuat teks sanggahan hukum (*legal warning banner*) formal pada konfigurasi banner SSH server. |
| **Kontrol 37 & 38** | Logging System | Mengonfigurasi audit log khusus untuk merekam eksekusi perintah `su` dan `sudo`, serta mengaktifkan kembali mekanisme pengumpulan log terpusat (`syslog`). |
| **Kontrol 39** | Manajemen Log | Mengubah parameter konfigurasi aturan `logrotate.conf` untuk memperpanjang masa retensi log dari 4 minggu menjadi 3-6 bulan guna keperluan forensik. |
| **Kontrol 41** | Pemantauan | Memulai perencanaan implementasi sistem pemantauan keamanan berkala (seperti Host-based scanner, IDS, atau IPS). |

---

## 📚 Referensi Akademis
1. Kegerreis, M., Schiller, M., & Davis, C. (2019). *IT Auditing Using Controls to Protect Information Assets, Third Edition*. McGraw-Hill Education.
2. Proxmox Server Solutions GmbH. (2024). *Proxmox VE Administration Guide, Release 8.3.1*.
3. Standar Dokumentasi Keamanan & Kontrol Komputasi (*CIS Controls, NIST Guidelines, ISACA Frameworks*).
