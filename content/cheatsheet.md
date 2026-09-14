# Cheatsheet Python

Referensi cepat sintaks dan fungsi standard library Python. Untuk penjelasan konsep, lihat [Tutorial Python](tutorial.html).

## Menjalankan

```bash
python3 program.py            # jalankan script
python3                       # buka REPL interaktif
python3 -m venv venv          # buat virtual environment
source venv/bin/activate      # aktifkan venv (macOS/Linux)
pip install nama_paket        # install package
pip freeze > requirements.txt # catat dependency
pip install -r requirements.txt
```

## Tipe Data & Konversi

| Tipe | Contoh | Konversi |
|---|---|---|
| `int` | `10`, `-3` | `int("10")`, `int(3.9)` -> `3` |
| `float` | `3.14` | `float("3.14")`, `float(10)` -> `10.0` |
| `str` | `"halo"` | `str(42)` -> `"42"` |
| `bool` | `True`/`False` | `bool(0)` -> `False`, `bool("")` -> `False` |
| `list` | `[1, 2]` | `list("ab")` -> `['a', 'b']` |
| `tuple` | `(1, 2)` | `tuple([1, 2])` |
| `dict` | `{"a": 1}` | `dict(a=1, b=2)` |
| `set` | `{1, 2}` | `set([1, 1, 2])` -> `{1, 2}` |
| `None` | `None` | representasi "tidak ada nilai" |

`type(x)` -> cek tipe. `isinstance(x, int)` -> cek tipe (dukung inheritance, lebih disarankan daripada `type(x) == int`).

## Operator

```python
+ - * / // % **        # aritmatika (/ selalu float, // pembagian bulat)
== != > < >= <=         # perbandingan
and or not               # logika (kata, bukan simbol)
is  is not               # identitas objek
in  not in                # keanggotaan (list/str/dict/set)
= += -= *= /= //= %= **=  # assignment
```

## String

```python
s = "Hello, Python"
s[0]; s[-1]; s[0:5]; s[::-1]        # indexing & slicing
s.upper(); s.lower(); s.title()
s.strip(); s.lstrip(); s.rstrip()
s.split(", "); "-".join(["a","b"])
s.replace("Python", "World")
s.find("Py")          # index, -1 jika tidak ketemu
s.startswith("He"); s.endswith("on")
s.isdigit(); s.isalpha(); s.isspace()
len(s)

nama, umur = "Budi", 20
f"Nama: {nama}, umur: {umur}"        # f-string
f"{3.14159:.2f}"                      # "3.14"
f"{umur:>5}"                          # rata kanan lebar 5
f"{umur=}"                            # debug -> "umur=20"
```

## Struktur Kontrol

```python
if kondisi:
    ...
elif kondisi2:
    ...
else:
    ...

for i in range(5):        # 0..4
    ...
for i in range(2, 10, 2): # start, stop, step
    ...
for item in daftar:
    ...
for i, item in enumerate(daftar):     # dengan index
    ...
for a, b in zip(list1, list2):        # iterasi paralel
    ...

while kondisi:
    ...

break        # keluar loop
continue     # lanjut iterasi berikutnya
```

## List

```python
lst = [1, 2, 3]
lst.append(4)          # tambah di akhir
lst.insert(0, 0)        # sisipkan di index
lst.remove(2)            # hapus by value
lst.pop()                 # hapus & return elemen terakhir
lst.pop(0)                # hapus & return by index
lst.extend([5, 6])        # gabung list lain
lst.sort()                 # urutkan in-place
lst.sort(reverse=True)
lst.sort(key=lambda x: -x)
sorted(lst)                 # return list baru, tidak ubah asli
lst.reverse()
lst.index(3)                 # cari index by value
lst.count(2)                  # hitung kemunculan
len(lst); sum(lst); max(lst); min(lst)
lst.copy()                     # shallow copy
lst[1:3]                        # slicing
```

## Tuple

```python
t = (1, 2, 3)
x, y, z = t             # unpacking
a, *rest = [1, 2, 3, 4]  # a=1, rest=[2, 3, 4]
```

## Dict

```python
d = {"nama": "Budi", "umur": 20}
d["nama"]                       # akses (error jika key tak ada)
d.get("kota", "N/A")            # akses aman dengan default
d["kota"] = "Jakarta"           # tambah/update
d.pop("umur")                   # hapus & return value
d.keys(); d.values(); d.items()
"nama" in d                     # cek key ada
for k, v in d.items():
    ...
{**d1, **d2}                     # merge dua dict
d.update({"umur": 21})
```

## Set

```python
s = {1, 2, 3}
s.add(4)
s.remove(2)          # error jika tidak ada
s.discard(2)          # aman walau tidak ada
a | b     # union
a & b     # intersection
a - b     # difference
a ^ b     # symmetric difference
a <= b    # subset?
```

## Comprehension

```python
[x**2 for x in range(10)]
[x for x in range(20) if x % 2 == 0]
{x: x**2 for x in range(5)}
{c for c in "mississippi"}
(x**2 for x in range(10))      # generator expression (lazy)
```

## Fungsi

```python
def f(a, b=10, *args, **kwargs):
    return a + b

f(1)                        # positional
f(a=1, b=2)                 # keyword
f(1, 2, 3, 4, x=5)          # args=(3,4), kwargs={'x':5}

lambda x, y: x + y           # fungsi anonim satu ekspresi

def gen(n):
    for i in range(n):
        yield i               # generator

map(func, iterable)
filter(func, iterable)
sorted(iterable, key=func, reverse=True)
```

## Exception Handling

```python
try:
    ...
except ValueError as e:
    print(e)
except (TypeError, KeyError):
    ...
else:
    ...          # jalan jika TIDAK ada exception
finally:
    ...          # selalu jalan

raise ValueError("pesan error")

class MyError(Exception):
    pass
```

Exception umum: `ValueError`, `TypeError`, `KeyError`, `IndexError`, `ZeroDivisionError`, `FileNotFoundError`, `AttributeError`, `ImportError`.

## File I/O

```python
with open("file.txt", "r") as f:
    isi = f.read()
    # atau: f.readlines() -> list per baris, atau iterasi "for baris in f"

with open("file.txt", "w") as f:
    f.write("teks\n")

with open("file.txt", "a") as f:
    f.write("tambahan\n")

import json
with open("data.json", "w") as f:
    json.dump(data, f, indent=2)
with open("data.json") as f:
    data = json.load(f)
```

Mode: `"r"` baca, `"w"` tulis (timpa), `"a"` tambah, `"x"` buat baru, `+"b"` untuk biner.

## OOP

```python
class Hewan:
    def __init__(self, nama):
        self.nama = nama

    def bersuara(self):
        return "..."

    def __str__(self):
        return f"Hewan({self.nama})"

class Anjing(Hewan):
    def __init__(self, nama, ras):
        super().__init__(nama)
        self.ras = ras

    def bersuara(self):
        return "Guk!"

obj = Anjing("Rex", "Labrador")
isinstance(obj, Hewan)     # True (cek termasuk inheritance)
```

Dunder umum: `__init__`, `__str__`, `__repr__`, `__eq__`, `__len__`, `__iter__`, `__next__`, `__enter__`/`__exit__`.

## Decorator & Context Manager

```python
def decorator(func):
    def wrapper(*args, **kwargs):
        # sebelum
        hasil = func(*args, **kwargs)
        # sesudah
        return hasil
    return wrapper

@decorator
def f(): ...

@functools.lru_cache
def fib(n): ...             # caching hasil pemanggilan

with open("f.txt") as f:
    ...                       # context manager bawaan
```

## Modul Standard Library yang Sering Dipakai

```python
import os
os.getcwd(); os.listdir(); os.path.join("a", "b"); os.path.exists("f.txt")

import sys
sys.argv                     # argumen command-line
sys.exit(1)

import random
random.randint(1, 100); random.choice([1,2,3]); random.shuffle(lst)

import datetime
datetime.datetime.now(); datetime.date.today()

import math
math.sqrt(16); math.floor(3.7); math.ceil(3.2); math.isclose(a, b)

import re
re.match(r"\d+", s); re.findall(r"[a-z]+", s); re.sub(r"\s+", " ", s)
```

## Kesalahan Umum

| Kesalahan | Penjelasan singkat |
|---|---|
| `def f(x, lst=[])` | mutable default argument "menempel" antar pemanggilan — pakai `None` |
| `if x == None` | pakai `is None`, bukan `==` |
| `0.1 + 0.2 == 0.3` | `False` karena presisi float — pakai `math.isclose` |
| `for x in lst: lst.remove(x)` | mengubah list saat diiterasi melompati elemen — iterasi salinan |
| `except Exception:` polos | menyembunyikan bug asli — tangkap exception spesifik |
| Lupa `self` di method | `TypeError: missing 1 required positional argument` |
| `import` melingkar antar modul | `ImportError` — pisah kode bersama ke modul ketiga |
| Install package tanpa venv | bentrok versi dependency antar proyek |
| `"5" + 3` | `TypeError` — Python tidak auto-convert str+int, konversi manual dulu |
