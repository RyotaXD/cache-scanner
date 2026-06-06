# Auto Scanner Cache & Malware (V1.0)

Script Python berbasis CLI yang dirancang untuk membersihkan file sampah tersembunyi, mendeteksi berkas duplikat sejati, serta memindai anomali/malware yang sering lolos dari sistem keamanan standar Android. Dioptimalkan penuh untuk dijalankan di lingkungan **Termux** maupun **Linux**.

---

## 📌 Penjelasan Alur Kerja Script

Script ini dirancang dengan pendekatan berlapis untuk memastikan memori internal tetap bersih tanpa mengorbankan data penting milik pengguna:

1. **Fase Inisialisasi & Proteksi** Script mendeteksi jalur penyimpanan utama (`/storage/emulated/0`). Untuk mencegah terjadinya eror pada sistem Android atau aplikasi game besar, direktori krusial seperti `Android/obb` sengaja dilewati (*skipped*) sejak awal pengindeksan berkas.
2. **Pembersihan Otomatis** Script menyisir file satu per satu. Jika file tersebut berakhiran ekstensi sampah umum (`.tmp`, `.log`, `.cache`) atau merupakan file **"gaib"** buatan OS lain yang tidak terbaca oleh file manager standar Android (seperti berkas metadata Apple Mac berawalan `._` atau `.DS_Store`), script akan langsung menghapusnya secara permanen.
3. **Analisis Ancaman & Keamanan** File-file yang memiliki ekstensi ganda mencurigakan (trik malware seperti `dokumen.pdf.apk`) atau file script desktop yang tidak wajar berada di memori HP (`.exe`, `.bat`, `.sh`) akan langsung ditangkap. Demi keamanan data Anda, file ini **tidak dihapus otomatis** melainkan dikarantina dalam bentuk daftar laporan *Alert* di akhir proses untuk kontrol manual.
4. **Pelacakan File Duplikat** Bukan sekadar mencocokkan nama file yang sama, script ini menghitung sidik jari biner sejati menggunakan **MD5 Hash** untuk file di atas 100 KB. Jika ada dua gambar atau video yang isinya 100% sama tetapi namanya berbeda, script akan mendeteksinya sebagai duplikat (tidak dihapus otomatis).
5. **Optimasi Kernel & Loop Standby:** Jika perangkat mendukung perintah ADB, script akan mengirimkan sinyal `trim-caches` untuk memangkas sisa cache tersembunyi di sistem. Setelah semua selesai, tabel laporan Rich akan dimunculkan, dan script memasuki mode *standby* sebelum otomatis melakukan pemindaian ulang pada loop berikutnya.

---

## ⚙️ Fitur Utama

* **Deep Junk Purge:** Membersihkan file `.tmp`, `.log`, `.temp`, `.apk.tmp`, `.cache`, `.bk', '.thumb', '.chk', '.old', '.gid', '.DS_Store' secara menyeluruh.

* **Hidden OS Metadata Scanner:** Menghapus file sampah gaib berawalan titik (`.`) atau prefix metadata hasil transfer antar OS seperti `._` dan `.DS_Store`.

* **Anomaly & Malware Detection:** Memindai trik penyamaran malware klasik seperti *double extension* (`.pdf.apk`), script eksekusi desktop berbahaya (`.exe`, `.bat`, `.sh`), serta direktori tersembunyi yang mencurigakan.

* **MD5 Duplicate Tracker:** Mendeteksi file duplikat (*hash MD5*), bukan cuma berdasarkan kesamaan nama file.

* **Automation Loop:** Berjalan secara *real-time* di latar belakang dengan jeda waktu otomatis (*standby engine*).

* **Rich UI Engine:** Tampilan antarmuka terminal yang interaktif menggunakan progress bar animasi dan tabel ringkasan yang rapi.

---

## 📦 Panduan Instalasi (Termux & Linux)

Pilih perintah di bawah ini sesuai dengan lingkungan (*environment*) perangkat yang Anda gunakan:

### Perintah Terminal:

* **Untuk Pengguna Termux**
  ```bash
  termux-setup-storage
  pkg update && pkg upgrade -y
  pkg install python -y
  pip install rich
  git clone https://RyotaXD/cache-scanner
  cd cache-scanner
  python scanner.py

* **Untuk Pengguna Linux**
  ```bash
  sudo apt update && sudo apt upgrade -y
  sudo apt install python3 python3-pip -y
  pip install rich
  git clone https://RyotaXD/cache-scanner
  cd cache-scanner
  python3 scanner.py
  
