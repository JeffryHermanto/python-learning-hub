# Tutorial Python

Panduan belajar Python dari dasar sampai konsep menengah, dengan penjelasan **konseptual** (bukan cuma sintaks) — bagaimana Python mengelola data di balik layar, kenapa fitur tertentu dirancang seperti ini, dan jebakan umum yang perlu dihindari.

## 1. Pengantar: Kenapa Python

Python dibuat Guido van Rossum akhir 1980-an dengan filosofi **keterbacaan kode di atas segalanya** (lihat `import this` — "The Zen of Python"). Berbeda dari C yang memberi kontrol memori manual, Python adalah bahasa **high-level, interpreted, dan dynamically typed**:

- **Interpreted** — kode dijalankan baris demi baris oleh interpreter (CPython, implementasi paling umum), bukan dikompilasi langsung ke mesin biner seperti C. Ini membuat Python lebih lambat, tapi jauh lebih cepat untuk dikembangkan dan di-debug.
- **Dynamically typed** — tipe data melekat pada **nilai**, bukan pada **variabel**. Variabel yang sama bisa menunjuk ke `int` lalu ke `str` tanpa deklarasi ulang. Ini fleksibel, tapi berarti error tipe data sering baru ketahuan saat runtime, bukan saat "kompilasi".
- **Otomatis mengelola memori** — ada garbage collector (reference counting + cycle detector) yang membebaskan memori objek yang sudah tidak dipakai. Kamu tidak perlu `malloc`/`free` seperti di C.

**Konsep kunci yang akan berulang kali muncul:** di Python, **semuanya adalah objek** — angka, fungsi, bahkan class itu sendiri. Variabel hanyalah **nama yang menunjuk (reference) ke objek**, bukan kotak yang menyimpan nilai secara langsung. Ini menjelaskan banyak perilaku Python yang awalnya membingungkan (mutable default argument, `is` vs `==`, aliasing list).

## 2. Instalasi & Menjalankan Program

Python punya tiga cara utama untuk dijalankan:

```bash
python3 --version          # cek versi terpasang
python3                    # buka REPL (Read-Eval-Print Loop) interaktif
python3 program.py         # jalankan file script
```

- **REPL** — bagus untuk eksperimen cepat, tapi tidak untuk program nyata. Ketik `exit()` atau `Ctrl+D` untuk keluar.
- **Script** (`.py`) — cara utama menjalankan program. Interpreter membaca file dari atas ke bawah dan mengeksekusi tiap baris.

```python
# program.py
if __name__ == "__main__":
    print("Hello, World!")
```

- `__name__` adalah variabel bawaan yang berisi `"__main__"` **hanya jika** file dijalankan langsung (`python3 program.py`), dan berisi nama modul jika file di-*import* dari file lain. Pola `if __name__ == "__main__":` memisahkan kode yang harus jalan otomatis dari kode yang boleh dipakai ulang sebagai modul — konsep penting begitu kamu mulai memecah program jadi banyak file.

## 3. Variabel & Tipe Data

Konsep inti: **variabel adalah label yang menunjuk ke objek di memori**, bukan wadah nilai. `x = 5` berarti "buat objek integer 5, lalu tempelkan label `x` padanya" — bukan "salin angka 5 ke dalam kotak bernama x".

```python
umur = 25            # int
tinggi = 172.5        # float
nama = "Ayu"          # str
lulus = True          # bool
data = None           # NoneType -> representasi "tidak ada nilai"
```

| Tipe | Contoh | Keterangan |
|---|---|---|
| `int` | `10`, `-3` | presisi tak terbatas (tidak overflow seperti C) |
| `float` | `3.14` | presisi ganda (IEEE 754), sama seperti `double` di C |
| `str` | `"halo"` | immutable, dukung Unicode penuh |
| `bool` | `True`/`False` | sebenarnya subclass dari `int` (`True == 1`) |
| `NoneType` | `None` | representasi "kosong", bukan `0` atau `""` |
| `complex` | `2+3j` | bilangan kompleks, jarang dipakai kecuali sains |

Poin konseptual penting:

- **Tidak perlu deklarasi tipe.** Interpreter menentukan tipe dari nilai yang diberikan. Cek tipe dengan `type(x)`.
- **`int` di Python tidak overflow** — beda dari C yang punya batas `INT_MAX`. Python otomatis menggunakan representasi bignum kalau angkanya sangat besar.
- **Konversi tipe eksplisit** pakai fungsi konstruktor: `int("5")`, `float("3.14")`, `str(42)`, `bool(0)` (falsy).
- **Duck typing** — Python tidak peduli tipe konkret objek, hanya peduli apakah objek punya perilaku (method/atribut) yang dibutuhkan. "Kalau jalan seperti bebek dan bersuara seperti bebek, anggap saja bebek."

```python
x = "5"
y = int(x) + 3   # konversi wajib, "5" + 3 akan error (TypeError)
print(type(y))   # <class 'int'>
```

## 4. Operator

```python
# Aritmatika
+  -  *  /  //  %  **
# / -> pembagian float (selalu), // -> pembagian bulat (floor division), ** -> pangkat

# Perbandingan
==  !=  >  <  >=  <=

# Logika (short-circuit, pakai kata bukan simbol)
and  or  not

# Identity & membership
is  is not      # bandingkan identitas objek (alamat memori), bukan nilai
in  not in      # cek keanggotaan di list/str/dict/set

# Assignment
=  +=  -=  *=  /=  //=  %=  **=
```

Konsep yang sering menjebak pemula:

**`/` vs `//`.** Beda dari C, `/` di Python **selalu** menghasilkan `float`, walau kedua operand `int`. Untuk pembagian bulat gunakan `//` (floor division, dibulatkan ke bawah — termasuk untuk angka negatif):

```python
print(7 / 2)    # 3.5
print(7 // 2)   # 3
print(-7 // 2)  # -4  (bukan -3! dibulatkan ke bawah, bukan ke nol)
```

**`==` vs `is`.** `==` membandingkan **nilai**, `is` membandingkan **identitas objek** (apakah dua variabel menunjuk ke objek yang sama persis di memori). Untuk cek `None`, selalu pakai `is None`, bukan `== None`, karena ini idiom yang mengomunikasikan maksud dengan jelas dan menghindari kasus tepi objek yang mengoverride `__eq__`.

```python
a = [1, 2, 3]
b = [1, 2, 3]
print(a == b)   # True  -> isi sama
print(a is b)   # False -> objek berbeda di memori
print(a is a)   # True
```

## 5. String & Formatting

String di Python **immutable** — setiap "modifikasi" sebenarnya membuat objek string baru, bukan mengubah string asli di tempat.

```python
s = "Hello, Python"

s[0]            # 'H'          -> indexing
s[-1]           # 'n'          -> index negatif dari belakang
s[0:5]          # 'Hello'      -> slicing [mulai:akhir] (akhir tidak termasuk)
s[::-1]         # 'nohtyP ,olleH' -> trik membalik string dengan step -1
s.upper()       # 'HELLO, PYTHON'
s.lower()       # 'hello, python'
s.split(", ")   # ['Hello', 'Python']
s.replace("Python", "World")
s.strip()       # buang whitespace di awal/akhir
```

**f-string** (Python 3.6+) adalah cara modern dan direkomendasikan untuk memformat string — lebih terbaca daripada `%` atau `.format()`:

```python
nama = "Budi"
umur = 20
print(f"Nama saya {nama}, umur {umur} tahun")
print(f"Tahun depan umur {umur + 1}")     # ekspresi langsung di dalam {}
print(f"Pi = {3.14159:.2f}")               # format angka: 2 desimal -> "Pi = 3.14"
print(f"{umur=}")                          # debug shortcut (3.8+) -> "umur=20"
```

Karena string immutable, melakukan penggabungan string berulang kali di dalam loop (`s = s + x`) itu **tidak efisien** karena tiap iterasi membuat objek string baru. Untuk banyak penggabungan, kumpulkan dulu di list lalu gabungkan sekali dengan `"".join(list_string)`.

## 6. Struktur Kontrol

Python tidak memakai `{}` untuk blok kode — **indentasi (spasi) itu sendiri adalah sintaks**. Konvensi standar (PEP 8): 4 spasi per level, jangan campur tab dan spasi.

```python
# if / elif / else
nilai = 75
if nilai >= 90:
    grade = "A"
elif nilai >= 75:
    grade = "B"
else:
    grade = "C"

# for -> iterasi atas sequence, bukan counter seperti C
for i in range(5):        # 0, 1, 2, 3, 4
    print(i)

for buah in ["apel", "jeruk", "mangga"]:
    print(buah)

# while
n = 0
while n < 5:
    print(n)
    n += 1              # tidak ada n++ di Python

# break, continue, else pada loop (fitur unik Python!)
for i in range(10):
    if i == 5:
        break
else:
    print("loop selesai tanpa break")   # TIDAK dieksekusi karena ada break
```

- `range(5)` menghasilkan angka 0 sampai 4 (**5 tidak termasuk**) — sama seperti slicing, batas akhir eksklusif adalah konvensi konsisten di Python.
- **`for...else`** adalah fitur yang sering bikin bingung: blok `else` dieksekusi jika loop selesai **tanpa** dihentikan oleh `break`. Berguna untuk pola "cari sesuatu, kalau tidak ketemu lakukan X" tanpa flag boolean tambahan.
- Python tidak punya `switch/case` bawaan sebelum versi 3.10. Sejak 3.10 ada `match` statement (structural pattern matching):

```python
match nilai:
    case n if n >= 90:
        grade = "A"
    case n if n >= 75:
        grade = "B"
    case _:
        grade = "C"
```

## 7. List, Tuple, Set, Dict

Empat struktur data bawaan ini adalah fondasi hampir semua program Python.

### List — koleksi berurutan, mutable

```python
angka = [1, 2, 3]
angka.append(4)          # [1, 2, 3, 4]
angka.insert(0, 0)       # [0, 1, 2, 3, 4]
angka.remove(2)          # hapus nilai 2 (bukan index 2) -> [0, 1, 3, 4]
angka.pop()              # hapus & kembalikan elemen terakhir
len(angka)               # jumlah elemen
angka.sort()             # urutkan di tempat (in-place)
sorted(angka)            # kembalikan list baru terurut, list asli tidak berubah
```

### Tuple — koleksi berurutan, immutable

```python
titik = (3, 4)
x, y = titik              # unpacking
```

Tuple dipakai saat data **tidak boleh berubah** setelah dibuat (misal koordinat, RGB), dan lebih hemat memori & lebih cepat diakses daripada list. Kunci dict **harus** immutable, jadi tuple bisa jadi key, list tidak bisa.

### Set — koleksi tak berurutan, elemen unik

```python
unik = {1, 2, 2, 3}        # {1, 2, 3} -> duplikat otomatis hilang
a = {1, 2, 3}
b = {2, 3, 4}
a | b   # union -> {1, 2, 3, 4}
a & b   # interseksi -> {2, 3}
a - b   # selisih -> {1}
```

Set berguna untuk **cek keanggotaan cepat** (`in` di set adalah O(1) rata-rata, di list adalah O(n)) dan menghilangkan duplikat.

### Dict — pasangan key-value

```python
mahasiswa = {"nama": "Rina", "umur": 21, "jurusan": "TI"}
mahasiswa["nama"]              # "Rina"
mahasiswa["ipk"] = 3.8          # tambah key baru
mahasiswa.get("kota", "N/A")    # ambil aman, default "N/A" jika key tak ada
mahasiswa.keys()                 
mahasiswa.values()
mahasiswa.items()               # pasangan (key, value), biasa dipakai di loop

for key, value in mahasiswa.items():
    print(f"{key}: {value}")
```

Sejak Python 3.7, **dict menjaga urutan insersi** (dulu tidak dijamin) — ini implementation detail penting yang sekarang jadi bagian dari spesifikasi bahasa.

**Mutable vs immutable — kenapa ini penting:** `list`, `dict`, `set` bisa diubah di tempat; `int`, `float`, `str`, `tuple` tidak bisa. Ini berdampak langsung ke perilaku passing argumen ke fungsi (lihat Bab 9) dan bug klasik "mengubah list yang ternyata sama-sama dipakai dua variabel":

```python
a = [1, 2, 3]
b = a            # b menunjuk ke OBJEK LIST YANG SAMA, bukan salinan
b.append(4)
print(a)         # [1, 2, 3, 4] -> a ikut berubah!

c = a.copy()     # salinan independen (shallow copy)
```

## 8. Comprehension

Comprehension adalah cara ringkas membuat list/dict/set baru dari iterable lain — lebih Pythonic (idiomatis) daripada loop manual untuk kasus sederhana.

```python
# List comprehension
kuadrat = [x**2 for x in range(10)]
genap = [x for x in range(20) if x % 2 == 0]              # dengan filter
matriks = [[i*j for j in range(3)] for i in range(3)]      # nested

# Dict comprehension
kuadrat_dict = {x: x**2 for x in range(5)}

# Set comprehension
unik_huruf = {c for c in "mississippi"}

# Generator expression -> mirip list comprehension tapi malas (lazy), hemat memori
total = sum(x**2 for x in range(1000000))   # tidak membuat list 1 juta elemen di memori
```

Aturan praktis: pakai comprehension untuk transformasi/filter satu baris yang jelas; kalau logikanya butuh beberapa kondisi bersarang atau lebih dari ~2 baris, tulis sebagai loop biasa demi keterbacaan — jangan memaksakan comprehension yang malah sulit dibaca.

## 9. Fungsi

```python
def sapa(nama, sapaan="Halo"):     # "Halo" adalah default argument
    """Docstring: menjelaskan fungsi ini, bisa diakses lewat sapa.__doc__."""
    return f"{sapaan}, {nama}!"

print(sapa("Budi"))                # "Halo, Budi!"
print(sapa("Budi", "Hai"))         # positional argument
print(sapa(nama="Budi", sapaan="Hai"))  # keyword argument
```

**`*args` dan `**kwargs`** — menerima jumlah argumen yang tidak tetap:

```python
def total(*angka):                  # angka jadi tuple: (1, 2, 3)
    return sum(angka)

def info(**data):                   # data jadi dict: {"nama": "Budi", "umur": 20}
    for k, v in data.items():
        print(f"{k}: {v}")

total(1, 2, 3)                      # 6
info(nama="Budi", umur=20)
```

**Jebakan klasik: mutable default argument.** Default argument dievaluasi **sekali saja**, saat fungsi didefinisikan — bukan setiap kali dipanggil. Kalau defaultnya `list`/`dict`, objek yang sama dipakai bersama di semua pemanggilan:

```python
def tambah_item(item, daftar=[]):   # SALAH — jangan lakukan ini
    daftar.append(item)
    return daftar

print(tambah_item("a"))   # ['a']
print(tambah_item("b"))   # ['a', 'b']  -> tidak diduga! daftar lama "menempel"

# Perbaikan idiomatis:
def tambah_item_benar(item, daftar=None):
    if daftar is None:
        daftar = []
    daftar.append(item)
    return daftar
```

**Passing argumen: "pass by object reference".** Python bukan pass-by-value murni maupun pass-by-reference murni. Argumen dikirim sebagai referensi ke objek yang sama — jika objeknya *mutable* dan diubah **di tempat** (`.append`, `[i] = x`) di dalam fungsi, perubahan itu terlihat di luar fungsi. Tapi jika di dalam fungsi kamu **assign ulang** parameter ke objek baru, itu tidak memengaruhi variabel pemanggil.

```python
def ubah(lst):
    lst.append(99)     # mengubah objek yang sama -> terlihat di luar

def ganti(lst):
    lst = [0, 0, 0]     # bikin objek baru, cuma ubah nama lokal -> TIDAK terlihat di luar

data = [1, 2, 3]
ubah(data)
print(data)   # [1, 2, 3, 99]
```

## 10. Scope & Closures

Python mencari nama variabel dengan aturan **LEGB**: **L**ocal → **E**nclosing → **G**lobal → **B**uilt-in, berhenti di scope pertama yang cocok.

```python
x = "global"

def luar():
    x = "enclosing"
    def dalam():
        x = "local"
        print(x)        # "local" -> ditemukan di scope Local dulu
    dalam()
    print(x)            # "enclosing"

luar()
print(x)                # "global"
```

Untuk **mengubah** (bukan cuma membaca) variabel dari scope luar, perlu deklarasi eksplisit karena secara default assignment di dalam fungsi selalu membuat variabel lokal baru:

```python
counter = 0
def tambah():
    global counter          # tanpa ini, counter += 1 akan error UnboundLocalError
    counter += 1

def buat_counter():
    n = 0
    def increment():
        nonlocal n           # untuk mengubah variabel di enclosing scope (bukan global)
        n += 1
        return n
    return increment

hitung = buat_counter()
print(hitung())   # 1
print(hitung())   # 2 -> "n" tetap "diingat" antar pemanggilan, ini disebut closure
```

**Closure** adalah fungsi yang "mengingat" variabel dari scope tempat ia didefinisikan, meskipun scope itu sudah selesai dieksekusi. Ini fondasi konseptual untuk decorator (Bab 18).

## 11. Lambda & Functional Programming

**Lambda** adalah fungsi anonim satu ekspresi, dipakai saat butuh fungsi kecil sekali pakai (biasanya sebagai argumen ke fungsi lain):

```python
kuadrat = lambda x: x ** 2
print(kuadrat(5))    # 25

# Paling umum dipakai sebagai key untuk sorting
mahasiswa = [{"nama": "Budi", "nilai": 80}, {"nama": "Ani", "nilai": 95}]
mahasiswa.sort(key=lambda m: m["nilai"], reverse=True)
```

**`map`, `filter`, `sorted`** — fungsi bawaan yang menerapkan fungsi lain ke iterable (functional style, alternatif dari loop eksplisit):

```python
angka = [1, 2, 3, 4, 5]
dobel = list(map(lambda x: x * 2, angka))          # [2, 4, 6, 8, 10]
genap = list(filter(lambda x: x % 2 == 0, angka))  # [2, 4]

from functools import reduce
total = reduce(lambda acc, x: acc + x, angka)       # 15, akumulasi dari kiri ke kanan
```

Di banyak kasus, **list comprehension lebih Pythonic** daripada `map`/`filter` dengan lambda karena lebih terbaca — `[x*2 for x in angka]` umumnya lebih disukai daripada `map(lambda x: x*2, angka)`.

## 12. Modul & Package

Setiap file `.py` adalah **modul**. Kumpulan modul dalam folder (dengan `__init__.py`, opsional sejak Python 3.3+) adalah **package**.

```python
# math_utils.py
def tambah(a, b):
    return a + b

# main.py
import math_utils
print(math_utils.tambah(2, 3))

from math_utils import tambah      # import fungsi spesifik
from math_utils import tambah as t  # alias

import math                         # modul standard library
print(math.sqrt(16))
```

**Virtual environment & pip** — mengelola dependency per-proyek supaya tidak bentrok antar proyek:

```bash
python3 -m venv venv           # buat virtual environment di folder ./venv
source venv/bin/activate       # aktifkan (macOS/Linux)
venv\Scripts\activate          # aktifkan (Windows)

pip install requests            # install package
pip freeze > requirements.txt   # catat semua dependency terpasang
pip install -r requirements.txt # install ulang dari catatan (di mesin lain)
deactivate                       # keluar dari virtual environment
```

Kenapa venv penting: tanpa itu, semua package ter-install **global** di satu Python — dua proyek yang butuh versi library berbeda akan bentrok. Venv mengisolasi dependency per folder proyek.

## 13. Exception Handling

Python menangani error lewat **exception** — objek yang "dilempar" (`raise`) saat error terjadi, lalu bisa "ditangkap" (`except`) di level manapun di atas pemanggilan.

```python
try:
    angka = int(input("Masukkan angka: "))
    hasil = 10 / angka
except ValueError:
    print("Input harus berupa angka!")
except ZeroDivisionError:
    print("Tidak bisa membagi dengan nol!")
else:
    print(f"Hasil: {hasil}")     # dijalankan HANYA jika tidak ada exception
finally:
    print("Selesai.")            # SELALU dijalankan, ada exception atau tidak
```

- **Tangkap exception sespesifik mungkin.** `except Exception:` polos menangkap segalanya termasuk bug yang seharusnya kamu lihat (misal `KeyError` karena typo) — ini menyembunyikan bug, bukan menyelesaikannya.
- **`raise`** untuk melempar exception sendiri, termasuk custom exception dengan membuat class turunan dari `Exception`:

```python
class SaldoTidakCukupError(Exception):
    """Dilempar saat penarikan melebihi saldo."""
    pass

def tarik(saldo, jumlah):
    if jumlah > saldo:
        raise SaldoTidakCukupError(f"Saldo {saldo} kurang dari {jumlah}")
    return saldo - jumlah

try:
    tarik(50000, 100000)
except SaldoTidakCukupError as e:
    print(f"Error: {e}")
```

Filosofi Python soal error handling sering disingkat **EAFP** ("Easier to Ask Forgiveness than Permission") — coba jalankan operasinya dulu, tangkap exception kalau gagal, daripada mengecek semua syarat dulu (LBYL, "Look Before You Leap") seperti gaya C. `try/except` di Python murah secara performa selama tidak ada exception yang benar-benar terjadi.

## 14. File I/O

```python
# Menulis
with open("data.txt", "w") as f:
    f.write("Baris pertama\n")
    f.write("Baris kedua\n")

# Membaca
with open("data.txt", "r") as f:
    isi = f.read()          # baca semua jadi satu string

with open("data.txt", "r") as f:
    for baris in f:          # iterasi baris demi baris, hemat memori untuk file besar
        print(baris.strip())

# Menambahkan (append)
with open("data.txt", "a") as f:
    f.write("Baris tambahan\n")
```

- **`with` statement** (context manager) menjamin file **selalu ditutup**, bahkan jika terjadi exception di tengah blok — mirip RAII di bahasa lain, tapi eksplisit. Tanpa `with`, kamu harus manual panggil `f.close()` dan gampang lupa kalau ada error di tengah jalan.
- Mode: `"r"` baca (default), `"w"` tulis (menimpa isi lama!), `"a"` tambah di akhir, `"x"` buat file baru (error jika sudah ada), tambahkan `"b"` untuk mode biner (`"rb"`, `"wb"`).

Untuk data terstruktur, modul `json` mengubah dict/list Python jadi teks JSON dan sebaliknya — sangat umum dipakai untuk menyimpan/membaca konfigurasi atau data sederhana tanpa database:

```python
import json

data = {"nama": "Budi", "nilai": [80, 90, 75]}
with open("data.json", "w") as f:
    json.dump(data, f, indent=2)

with open("data.json", "r") as f:
    data_terbaca = json.load(f)
```

## 15. OOP Dasar

```python
class Mahasiswa:
    def __init__(self, nama, nilai):    # constructor, dipanggil otomatis saat objek dibuat
        self.nama = nama                 # atribut instance
        self.nilai = nilai

    def lulus(self):                     # method -> selalu terima "self" sebagai parameter pertama
        return self.nilai >= 60

    def __str__(self):                    # dipanggil otomatis oleh print()/str()
        return f"{self.nama} ({self.nilai})"

m1 = Mahasiswa("Budi", 85)
print(m1.lulus())   # True
print(m1)           # "Budi (85)" -> lewat __str__
```

- **`self`** adalah referensi ke instance objek itu sendiri — Python mewajibkannya eksplisit sebagai parameter pertama tiap method (beda dari bahasa lain yang menyembunyikannya sebagai `this` implisit). Saat kamu memanggil `m1.lulus()`, Python sebenarnya mengeksekusi `Mahasiswa.lulus(m1)`.
- **Class vs instance attribute.** Atribut yang didefinisikan di `__init__` dengan `self.x` adalah milik masing-masing instance. Atribut yang didefinisikan langsung di body class (di luar method) **dibagi** ke semua instance — jebakan yang mirip mutable default argument kalau nilainya berupa list/dict.

```python
class Keranjang:
    items = []          # BAHAYA: dibagi semua instance!

    def __init__(self):
        self.items = []  # AMAN: tiap instance punya list sendiri
```

## 16. OOP Lanjutan: Inheritance, Polimorfisme, Enkapsulasi

```python
class Hewan:
    def __init__(self, nama):
        self.nama = nama

    def bersuara(self):
        raise NotImplementedError("Subclass harus mengimplementasikan ini")

class Anjing(Hewan):                 # Anjing mewarisi dari Hewan
    def bersuara(self):
        return f"{self.nama}: Guk!"

class Kucing(Hewan):
    def bersuara(self):
        return f"{self.nama}: Meong!"

for hewan in [Anjing("Rex"), Kucing("Milo")]:
    print(hewan.bersuara())          # polimorfisme: pemanggilan sama, perilaku beda per class
```

- **`super()`** memanggil method dari parent class, biasa dipakai di `__init__` supaya tidak menulis ulang logika inisialisasi parent:

```python
class Karyawan:
    def __init__(self, nama, gaji):
        self.nama = nama
        self.gaji = gaji

class Manager(Karyawan):
    def __init__(self, nama, gaji, tim):
        super().__init__(nama, gaji)   # jalankan __init__ milik Karyawan dulu
        self.tim = tim
```

- **Enkapsulasi lewat konvensi, bukan paksaan compiler.** Python tidak punya `private` sungguhan seperti Java/C++. Konvensinya:
  - `_nama` (satu underscore) — "protected by convention", sinyal ke sesama developer "jangan diakses dari luar", tapi tetap bisa diakses.
  - `__nama` (dua underscore) — memicu **name mangling**, Python mengganti nama atribut jadi `_NamaClass__nama` untuk mempersulit akses tidak sengaja dari luar, tapi tetap bukan proteksi absolut.
- **Dunder (double underscore) methods** lain yang sering dipakai: `__eq__` (perilaku `==`), `__len__` (perilaku `len()`), `__repr__` (representasi untuk debugging), `__iter__`/`__next__` (jadi iterable, lihat Bab 17).

## 17. Iterator & Generator

**Iterable** adalah objek yang bisa di-loop (`list`, `str`, `dict`, ...). **Iterator** adalah objek yang "mengingat posisi" dan menghasilkan nilai berikutnya lewat `next()`. `for` di Python sebenarnya memanggil `iter()` lalu `next()` berulang kali di baliknya.

**Generator** adalah cara termudah membuat iterator: fungsi dengan `yield` yang "menjeda" eksekusi dan melanjutkan dari titik terakhir setiap dipanggil lagi — nilai dihasilkan **satu per satu, sesuai kebutuhan (lazy)**, bukan sekaligus dibuat semua di memori:

```python
def angka_genap(maks):
    n = 0
    while n <= maks:
        yield n          # jeda di sini, kembalikan n, lanjut dari sini di panggilan berikutnya
        n += 2

for x in angka_genap(10):
    print(x)             # 0, 2, 4, 6, 8, 10

# Generator dieksekusi malas -> hemat memori untuk data besar/tak terbatas
gen = angka_genap(1000000)
print(next(gen))   # 0 -> baru menghitung 1 nilai, bukan 500001 sekaligus
```

Bandingkan dengan fungsi biasa yang `return list` — itu langsung membuat seluruh list di memori. Untuk dataset besar atau stream data (baca file besar baris per baris, misalnya), generator jauh lebih hemat memori.

## 18. Decorator

Decorator adalah fungsi yang **membungkus fungsi lain** untuk menambah perilaku tanpa mengubah kode fungsi aslinya — memanfaatkan konsep bahwa fungsi di Python adalah objek biasa (bisa dilewatkan sebagai argumen) plus closure (Bab 10).

```python
import functools
import time

def ukur_waktu(func):
    @functools.wraps(func)              # jaga metadata fungsi asli (nama, docstring)
    def wrapper(*args, **kwargs):
        mulai = time.time()
        hasil = func(*args, **kwargs)
        print(f"{func.__name__} selesai dalam {time.time() - mulai:.4f} detik")
        return hasil
    return wrapper

@ukur_waktu                # sintaks ini setara dengan: proses_data = ukur_waktu(proses_data)
def proses_data(n):
    return sum(range(n))

proses_data(1000000)
```

Decorator sering dipakai untuk logging, autentikasi/otorisasi, caching (`@functools.lru_cache`), validasi input, dan retry logic — kasus di mana perilaku "tambahan" itu berulang di banyak fungsi dan ingin dipisah dari logika inti.

## 19. Context Manager

`with` statement (dipakai untuk file di Bab 14) bekerja dengan protokol **context manager**: objek apapun yang mengimplementasikan `__enter__` dan `__exit__` bisa dipakai dengan `with`.

```python
class Timer:
    def __enter__(self):
        self.mulai = time.time()
        return self                       # nilai ini yang diterima variabel "as x"

    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"Durasi: {time.time() - self.mulai:.4f} detik")
        return False                       # False -> jangan redam exception yang terjadi di dalam blok

with Timer():
    time.sleep(1)
```

Cara lebih ringkas dengan `contextlib`:

```python
from contextlib import contextmanager

@contextmanager
def timer():
    mulai = time.time()
    yield                                   # kode di dalam blok "with" jalan di titik ini
    print(f"Durasi: {time.time() - mulai:.4f} detik")

with timer():
    time.sleep(1)
```

Pola ini menjamin "cleanup" (tutup file, lepas lock, commit/rollback transaksi database) selalu jalan, apapun yang terjadi di dalam blok — termasuk saat terjadi exception.

## 20. Type Hinting

Python tetap dynamically typed saat runtime, tapi sejak Python 3.5 kamu bisa menambahkan **anotasi tipe** yang diperiksa oleh tool eksternal (`mypy`, atau linter di editor) — bukan oleh interpreter saat program jalan.

```python
def tambah(a: int, b: int) -> int:
    return a + b

def sapa(nama: str, umur: int | None = None) -> str:   # "int | None" -> union type (3.10+)
    if umur is None:
        return f"Halo, {nama}"
    return f"Halo, {nama} ({umur} tahun)"

from typing import List, Dict

def rata_rata(nilai: List[float]) -> float:
    return sum(nilai) / len(nilai)

def hitung_kata(teks: str) -> Dict[str, int]:
    ...
```

Type hint **tidak memaksa** apapun saat runtime — `tambah("a", "b")` tetap akan dieksekusi (dan mungkin error di tengah jalan atau menghasilkan `"ab"` alih-alih error). Manfaat utamanya: dokumentasi yang bisa diverifikasi tool, autocomplete lebih baik di editor, dan bug tertangkap lebih awal lewat `mypy` di CI, bukan saat runtime di production.

## 21. Kesalahan Umum & Best Practice

Ringkasan jebakan yang sudah disinggung di bab-bab sebelumnya, dikumpulkan sebagai checklist:

- **Mutable default argument** (`def f(x, lst=[])`) — gunakan `None` sebagai sentinel, buat objek baru di dalam fungsi (Bab 9).
- **Membandingkan float dengan `==`** — sama seperti C, representasi IEEE 754 tidak presisi sempurna. Gunakan `math.isclose(a, b)` untuk perbandingan float.
- **Mengubah list yang sedang di-iterasi** — `for x in lst: lst.remove(x)` melompati elemen karena index bergeser saat elemen dihapus. Iterasi di atas salinan (`for x in lst.copy():`) atau bangun list baru.
- **`is` untuk membandingkan nilai** (bukan identitas) — `if x is 200` bisa kebetulan `True` untuk angka kecil (Python meng-cache integer -5..256) tapi `False` untuk angka besar. Selalu pakai `==` untuk nilai, `is` hanya untuk `None`/singleton.
- **Menangkap `except Exception:` terlalu luas** — menyembunyikan bug asli. Tangkap exception spesifik.
- **Sirkular import** — dua modul saling `import` satu sama lain menyebabkan `ImportError`. Pisahkan kode bersama ke modul ketiga, atau import di dalam fungsi (lazy import) jika memang perlu.
- **Lupa `self`** saat mendefinisikan method di dalam class — akan error `TypeError: missing argument`.
- **Menginstall package tanpa virtual environment** — lama-lama semua proyek berbagi satu set dependency global yang saling bentrok versi.

> Baca `import this` di REPL Python kapan saja butuh pengingat filosofi desain bahasa ini — "Readability counts", "Explicit is better than implicit", "There should be one obvious way to do it".
