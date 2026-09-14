# Mini Project Latihan Python

Kumpulan latihan project untuk mempraktikkan konsep dari [Tutorial Python](tutorial.html), diurutkan dari mudah ke sulit. Kerjakan berurutan — tiap project sengaja dirancang memakai konsep dari project sebelumnya.

Cara pakai: baca **Tujuan** dan **Requirement**, coba kerjakan sendiri dulu sebelum melihat **Hint**. Semua project bisa dikerjakan hanya dengan Python standar (tanpa install package eksternal), kecuali disebutkan lain.

---

## Level Pemula

### Project 1 — Kalkulator Sederhana

**Konsep:** input/output, operator, percabangan, exception handling dasar.

**Tujuan:** program menerima dua angka dan satu operator (`+ - * /`) dari user, lalu menampilkan hasilnya.

**Requirement:**

- Baca dua angka dengan `input()` dan `float()`, dan satu operator sebagai string.
- Gunakan `if/elif/else` untuk memilih operasi.
- Tangani pembagian dengan nol dan input yang bukan angka pakai `try/except`, jangan biarkan program crash.

**Hint:** bungkus `float(input(...))` dalam `try/except ValueError`. Untuk pembagian nol: `except ZeroDivisionError`.

**Tantangan bonus:** ubah jadi loop `while True` supaya user bisa hitung berkali-kali sampai mengetik `q` untuk keluar.

---

### Project 2 — Tebak Angka

**Konsep:** perulangan, percabangan, modul `random`.

**Tujuan:** komputer memilih angka acak 1–100, user menebak sampai benar, program memberi petunjuk "lebih besar"/"lebih kecil".

**Requirement:**

- Gunakan `random.randint(1, 100)` untuk memilih angka rahasia.
- Gunakan `while` untuk mengulang sampai tebakan benar.
- Hitung dan tampilkan jumlah percobaan di akhir.

**Hint:** `import random` di paling atas file.

**Tantangan bonus:** batasi maksimal 7 percobaan, kalah jika habis. Tambahkan mode kesulitan yang mengubah rentang angka.

---

### Project 3 — Konversi Suhu & Menu Kalkulator

**Konsep:** fungsi, default argument, f-string.

**Tujuan:** buat beberapa fungsi konversi suhu (`celcius_ke_fahrenheit`, `celcius_ke_kelvin`, dst) dan sebuah menu yang memanggilnya.

**Requirement:**

- Minimal 3 fungsi konversi, masing-masing menerima `float` dan mengembalikan `float`.
- Tampilkan menu pilihan dengan `if/elif`, panggil fungsi sesuai pilihan user.
- Program berjalan dalam loop sampai user memilih keluar (misal input `"0"`).
- Gunakan f-string untuk menampilkan hasil dengan 2 angka desimal.

**Hint:** definisikan semua fungsi konversi di atas, baru definisikan `main()` yang memanggil menu, tutup dengan `if __name__ == "__main__": main()`.

---

## Level Menengah

### Project 4 — Manajemen Nilai Mahasiswa

**Konsep:** list, dict, fungsi dengan parameter list, comprehension, statistik dasar.

**Tujuan:** program menyimpan data N mahasiswa (nama + nilai) sebagai `list` berisi `dict`, lalu menghitung rata-rata, nilai tertinggi, nilai terendah, dan daftar mahasiswa yang lulus (nilai >= 60).

**Requirement:**

- Simpan tiap mahasiswa sebagai `{"nama": ..., "nilai": ...}`, semuanya di dalam satu list.
- Buat fungsi terpisah untuk masing-masing statistik (`hitung_rata_rata(data)`, `cari_tertinggi(data)`, dst) — **jangan** hitung semuanya langsung di alur utama.
- Gunakan list comprehension untuk memfilter mahasiswa yang lulus.
- Tampilkan hasil akhir dalam format rapi dengan f-string.

**Hint:** `[m for m in data if m["nilai"] >= 60]` untuk filter lulus. `sum(m["nilai"] for m in data) / len(data)` untuk rata-rata (generator expression).

**Tantangan bonus:** urutkan mahasiswa dari nilai tertinggi ke terendah pakai `sorted(data, key=lambda m: m["nilai"], reverse=True)`.

---

### Project 5 — Pengolah Teks & Validator Password

**Konsep:** string methods, slicing, `re` (regex), fungsi boolean.

**Tujuan:** dua fitur dalam satu program: (1) analisis teks — hitung jumlah kata, huruf vokal, dan kata terpanjang dari sebuah paragraf; (2) validator password — cek apakah password memenuhi syarat keamanan.

**Requirement:**

- Fungsi `analisis_teks(teks)` mengembalikan dict berisi jumlah kata, jumlah vokal, dan kata terpanjang.
- Fungsi `validasi_password(pw)` mengembalikan `True`/`False` plus daftar alasan jika gagal: minimal 8 karakter, ada huruf besar, ada huruf kecil, ada angka.
- Gunakan `str.isupper()`, `str.islower()`, `str.isdigit()`, atau `re` untuk pengecekan.

**Hint:** `teks.split()` untuk memecah jadi kata. `max(kata_list, key=len)` untuk kata terpanjang.

**Tantangan bonus:** tambahkan syarat minimal satu karakter simbol (`!@#$%^&*`) memakai regex `re.search(r"[!@#$%^&*]", pw)`.

---

### Project 6 — Buku Alamat dengan Penyimpanan File JSON

**Konsep:** dict bersarang, file I/O, modul `json`, exception handling, fungsi CRUD.

**Tujuan:** aplikasi CLI untuk menyimpan kontak (nama, telepon, email) yang **persisten** — data tidak hilang setelah program ditutup, karena disimpan ke file `kontak.json`.

**Requirement:**

- Fungsi `muat_data()` membaca `kontak.json` jika ada, atau mengembalikan dict kosong jika file belum ada (tangani `FileNotFoundError`).
- Fungsi `simpan_data(data)` menulis dict ke `kontak.json` dengan `json.dump(..., indent=2)`.
- Menu CLI: tambah kontak, cari kontak (by nama), tampilkan semua, hapus kontak — tiap aksi langsung memanggil `simpan_data()` supaya perubahan permanen.
- Validasi input dasar (nama tidak boleh kosong, dsb).

**Hint:** struktur data `{"budi": {"telepon": "0812...", "email": "budi@mail.com"}}` — key nama (huruf kecil semua untuk pencarian case-insensitive).

**Tantangan bonus:** tambahkan fitur edit kontak dan ekspor semua kontak ke format teks rapi.

---

## Level Lanjutan

### Project 7 — Sistem Perpustakaan Sederhana (OOP)

**Konsep:** class, inheritance, `super()`, dunder methods, enkapsulasi.

**Tujuan:** model sistem peminjaman buku perpustakaan dengan OOP — class `Buku`, `AnggotaPerpustakaan`, dan `Perpustakaan` yang mengelola keduanya.

**Requirement:**

- Class `Buku` punya atribut `judul`, `penulis`, `stok`, dan method `__str__` untuk tampilan rapi.
- Class `Perpustakaan` punya list buku dan list anggota, dengan method `tambah_buku()`, `pinjam_buku(anggota, judul)`, `kembalikan_buku(anggota, judul)`.
- `pinjam_buku` harus mengurangi stok dan menolak (raise custom exception) jika stok habis — buat class `StokHabisError(Exception)`.
- Gunakan `_stok` (protected by convention) dan buat method/`property` untuk mengakses/mengubahnya, bukan diakses langsung dari luar.

**Hint:** `@property` mengubah method jadi bisa diakses seperti atribut (`buku.stok` alih-alih `buku.stok()`) sambil tetap menjalankan validasi di dalamnya.

**Tantangan bonus:** buat subclass `BukuDigital(Buku)` yang tidak pernah kehabisan stok (override method peminjaman) — demonstrasi polimorfisme.

---

### Project 8 — Aplikasi Pencatat Keuangan dengan File & Decorator

**Konsep:** OOP, file JSON, decorator, generator, exception custom — menggabungkan hampir semua konsep dari tutorial.

**Tujuan:** aplikasi CLI pencatat pemasukan/pengeluaran yang menyimpan riwayat transaksi ke file, dengan laporan ringkasan dan log otomatis tiap transaksi.

**Requirement:**

- Class `Transaksi` menyimpan `tanggal`, `kategori`, `jumlah`, `tipe` (`"masuk"`/`"keluar"`).
- Class `Dompet` menyimpan list transaksi, dengan method `tambah_transaksi()`, `saldo()` (property), `laporan_per_kategori()` (mengembalikan dict total per kategori).
- Buat decorator `@catat_log` yang membungkus method `tambah_transaksi` — tiap kali dipanggil, otomatis mencetak log `"[LOG] Transaksi ditambahkan: ..."` tanpa mengubah kode method itu sendiri.
- Simpan seluruh riwayat ke `transaksi.json` setiap ada perubahan, dan muat kembali saat program dijalankan ulang.
- Buat generator `transaksi_per_bulan(dompet, bulan)` yang meng-`yield` transaksi satu per satu sesuai bulan yang diminta (tidak membuat list baru sekaligus).
- Tangani kasus jumlah negatif atau tipe transaksi tidak valid dengan custom exception `TransaksiTidakValidError`.

**Hint:** simpan tanggal sebagai string ISO (`datetime.date.today().isoformat()`) supaya mudah disimpan di JSON (JSON tidak punya tipe tanggal asli).

**Tantangan bonus:** tambahkan fungsi `saring_transaksi(dompet, min_jumlah=None, tipe=None)` dengan default argument, dan gunakan `functools.lru_cache` pada fungsi laporan yang mahal untuk menghindari perhitungan ulang jika data belum berubah.
