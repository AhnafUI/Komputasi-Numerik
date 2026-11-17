# Algoritma Penyelesaian Sistem Persamaan Linear 3 Variabel
**Metode: Eliminasi Gauss dengan Pivoting Parsial + Substitusi Balik**

Nama : Ahnaf Zaiyan Firdaus
NIM  : 202531420003

---

## 1. Bentuk Masalah

Sistem persamaan linear (SPL) 3 variabel:

```
a11·x + a12·y + a13·z = b1
a21·x + a22·y + a23·z = b2
a31·x + a32·y + a33·z = b3
```

Ditulis dalam bentuk matriks **A·X = B**, lalu digabung menjadi **matriks augmented [A | B]** berukuran 3 × 4.

## 2. Ide Dasar

1. **Eliminasi maju**: ubah matriks menjadi **segitiga atas** (semua angka di bawah diagonal utama = 0).
2. **Substitusi balik**: hitung z dulu dari baris terakhir, lalu y, lalu x.
3. **Pivoting parsial**: sebelum mengeliminasi tiap kolom, baris dengan nilai mutlak terbesar di kolom itu ditukar ke posisi pivot. Tujuannya menghindari pembagian dengan nol dan mengurangi galat pembulatan.

## 3. Langkah-Langkah Algoritma

**Masukan :** koefisien a11 … a33 dan konstanta b1, b2, b3
**Keluaran:** nilai x, y, z (atau pesan "tidak punya solusi tunggal")

```
LANGKAH 1  Mulai.
LANGKAH 2  Baca 9 koefisien dan 3 konstanta.
LANGKAH 3  Susun matriks augmented M (3 baris × 4 kolom):
           M[i] = [ai1, ai2, ai3, bi]   untuk i = 1, 2, 3
LANGKAH 4  Tetapkan toleransi eps = 1e-12 (angka yang dianggap nol).

           --- ELIMINASI MAJU ---
LANGKAH 5  Untuk k = 1 sampai 2 (kolom pivot):
   5a. Cari baris p (k ≤ p ≤ 3) dengan |M[p][k]| paling besar.
   5b. Jika |M[p][k]| < eps → SPL tidak punya solusi tunggal
       (determinan = 0). Tampilkan pesan, lalu ke LANGKAH 9.
   5c. Jika p ≠ k, tukar baris p dengan baris k.
   5d. Untuk setiap baris i = k+1 sampai 3:
         faktor   = M[i][k] / M[k][k]
         M[i][j]  = M[i][j] − faktor × M[k][j]   untuk j = k sampai 4
LANGKAH 6  Jika |M[3][3]| < eps → tidak punya solusi tunggal. Ke LANGKAH 9.

           --- SUBSTITUSI BALIK ---
LANGKAH 7  z = M[3][4] / M[3][3]
           y = (M[2][4] − M[2][3]·z) / M[2][2]
           x = (M[1][4] − M[1][2]·y − M[1][3]·z) / M[1][1]

           --- VERIFIKASI ---
LANGKAH 8  Masukkan x, y, z ke ketiga persamaan awal.
           Hitung residu r_i = (ai1·x + ai2·y + ai3·z) − bi.
           Jika semua |r_i| < 1e-9 → solusi benar. Tampilkan x, y, z.
LANGKAH 9  Selesai.
```

## 4. Flowchart (ringkas)

```
   ┌────────┐
   │ Mulai  │
   └───┬────┘
       ▼
 [Input koefisien & konstanta]
       ▼
 [Susun matriks augmented 3×4]
       ▼
 ┌─► k = 1..2 ──────────────────────────┐
 │     ▼                                │
 │  [Cari pivot terbesar di kolom k]    │
 │     ▼                                │
 │  <|pivot| < eps ?> ──Ya──► [Tidak ada solusi tunggal] ──► Selesai
 │     │ Tidak                          │
 │     ▼                                │
 │  [Tukar baris bila perlu]            │
 │     ▼                                │
 │  [Nolkan elemen di bawah pivot]      │
 └─────┘ (lanjut k berikutnya)          │
       ▼                                │
 [Substitusi balik: z → y → x]          │
       ▼                                │
 [Verifikasi residu]                    │
       ▼                                │
 [Tampilkan x, y, z] ───────────────────┘
       ▼
   ┌────────┐
   │Selesai │
   └────────┘
```

## 5. Contoh Perhitungan Manual

```
 2x +  y −  z =   8
−3x −  y + 2z = −11
−2x +  y + 2z =  −3
```

Matriks augmented awal:
```
[  2   1  −1 |   8 ]
[ −3  −1   2 | −11 ]
[ −2   1   2 |  −3 ]
```

**k = 1** → pivot terbesar di kolom 1 adalah |−3| (baris 2), tukar baris 1 ↔ 2:
```
[ −3  −1   2 | −11 ]
[  2   1  −1 |   8 ]
[ −2   1   2 |  −3 ]
```
Faktor baris 2 = 2/(−3) = −0,667 ; faktor baris 3 = (−2)/(−3) = 0,667
```
[ −3  −1      2      | −11    ]
[  0   0,333  0,333  |   0,667 ]
[  0   1,667  0,667  |   4,333 ]
```

**k = 2** → pivot terbesar di kolom 2 adalah 1,667 (baris 3), tukar baris 2 ↔ 3:
```
[ −3  −1      2      | −11    ]
[  0   1,667  0,667  |   4,333 ]
[  0   0,333  0,333  |   0,667 ]
```
Faktor baris 3 = 0,333/1,667 = 0,2
```
[ −3  −1      2      | −11    ]
[  0   1,667  0,667  |   4,333 ]
[  0   0      0,2    |  −0,2   ]
```

**Substitusi balik**
```
z = −0,2 / 0,2                     = −1
y = (4,333 − 0,667·(−1)) / 1,667   =  3
x = (−11 − (−1)·3 − 2·(−1)) / (−3) =  2
```

**Hasil: x = 2, y = 3, z = −1.**
Cek: 2(2) + 3 − (−1) = 8 ✓

## 6. Kompleksitas

Untuk n variabel, eliminasi Gauss memerlukan sekitar ⅔·n³ operasi. Untuk n = 3, jumlahnya hanya puluhan operasi, jadi sangat cepat.
