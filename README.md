# Belajar Python — Tutorial & Cheatsheet

Website statis untuk belajar Python: tutorial konseptual, cheatsheet referensi cepat, dan mini project latihan.

## Struktur

```
.
├── index.html            # shell website (sidebar, TOC, search)
├── assets/
│   ├── style.css
│   └── app.js             # router hash + render markdown (marked.js + highlight.js)
├── content/
│   ├── tutorial.md         # tutorial Python: dasar -> OOP, decorator, generator, type hinting
│   ├── cheatsheet.md       # referensi cepat sintaks & standard library
│   └── projects.md         # 8 mini project latihan, pemula -> lanjutan
└── .claude/launch.json     # konfigurasi dev server lokal
```

## Menjalankan

File markdown dimuat lewat `fetch`, jadi harus diakses lewat HTTP server lokal (tidak bisa dibuka langsung sebagai `file://`):

```bash
python3 -m http.server 8843
```

Lalu buka `http://localhost:8843`.

## Konten

- **Tutorial** — 21 bab, mencakup instalasi & menjalankan program, tipe data, operator, string & formatting, struktur kontrol, list/tuple/set/dict, comprehension, fungsi (`*args`/`**kwargs`), scope & closure, lambda & functional programming, modul & package (venv/pip), exception handling, file I/O & JSON, OOP dasar & lanjutan, iterator & generator, decorator, context manager, type hinting, dan kesalahan umum.
- **Cheatsheet** — tabel referensi cepat: tipe data & konversi, operator, string, struktur data bawaan, comprehension, fungsi, exception, file I/O, OOP, decorator, modul standard library umum (`os`, `sys`, `random`, `datetime`, `math`, `re`), dan daftar kesalahan umum.
- **Mini Project** — 8 latihan bertingkat (kalkulator, tebak angka, konversi suhu, manajemen nilai mahasiswa, pengolah teks & validator password, buku alamat dengan JSON, sistem perpustakaan OOP, pencatat keuangan dengan decorator & generator) untuk mempraktikkan konsep dari tutorial.

## Tech stack

Vanilla HTML/CSS/JS, [marked.js](https://marked.js.org/) untuk parsing markdown, dan [highlight.js](https://highlightjs.org/) untuk syntax highlighting — semua dimuat lewat CDN, tanpa build step.
