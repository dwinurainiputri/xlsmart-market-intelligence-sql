# XLSMART Competitor Price Benchmarking: Analisis Harga Paket Data Seluler dengan MySQL

Proyek portofolio Marketing Intelligence: membandingkan harga paket data prabayar XLSMART (XL, AXIS, Smartfren) dengan Telkomsel dan Indosat/Tri menggunakan SQL.

## 1. Latar Belakang dan Tujuan
XL Axiata dan Smartfren resmi bergabung menjadi XLSMART pada 16 April 2025 (sumber: siaran pers XL Axiata, lihat `data/data_sources.md`). Perusahaan menjalankan beberapa brand sekaligus (XL, AXIS, Smartfren), sehingga posisi harga tiap brand terhadap kompetitor perlu dipetakan.

**Pertanyaan utama:** Bagaimana posisi harga dan nilai paket data bulanan XLSMART dibanding Telkomsel dan Indosat/Tri pada kelas kuota yang sama?

## 2. Dataset
- **Jenis:** data primer hasil pengumpulan manual (tracker harga paket data prabayar).
- **Jumlah data:** `<isi jumlah baris>` paket, `<isi jumlah brand>` brand, diambil pada `<tanggal>`.
- **Sumber:** aplikasi resmi operator (MyXL, MyTelkomsel, MyIM3, dsb.), situs resmi operator, dan artikel media sebagai pembanding silang. Rincian di `data/data_sources.md`.
- **Tabel:** `brand` dan `paket_data`; view `v_paket_normal` untuk normalisasi.

| Kolom | Keterangan |
|---|---|
| harga | Harga paket (Rp) |
| kuota_utama_gb | Kuota utama (GB) |
| masa_aktif_hari | Masa aktif apa adanya (28/30/7 dst.) |
| kondisi_harga | katalog_umum / personalisasi / promo_terbatas |
| usia_kartu_bulan | Usia nomor saat data diambil (jika diketahui) |
| tanggal_ambil | Tanggal pengambilan data |

## 3. Metodologi
**Normalisasi durasi.** Sebagian paket Telkomsel, Indosat, dan Tri berdurasi 28 hari, sedangkan XL umumnya 30 hari (periksa ulang saat pengambilan data). Karena itu dihitung:
- `harga_setara_30hr = harga x 30 / masa_aktif`
- `kuota_setara_30hr = kuota x 30 / masa_aktif`

Asumsi: kuota proporsional terhadap durasi.

**Kelompok pembanding.** Paket dibandingkan hanya dalam kelompok durasi yang sama (bulanan 28-30 hari) dan kelas kuota yang sama (<10, 10-29, 30-59, 60+ GB).

**Harga personal.** Telkomsel menyatakan harga dan varian paket dapat berbeda per nomor (skema tarif, umur kartu, rata-rata pemakaian kuota). Penanganannya:
- Setiap data ditandai `kondisi_harga`.
- Perbandingan lintas operator memakai harga katalog umum.
- Selisih harga personal dianalisis terpisah dari `<n>` sampel nomor (kode anonim, tanpa nomor asli).
- Hasil dilaporkan sebagai rentang (min, rata-rata, maks).

## 4. Struktur Repo
```
/data    CSV bersih + data_sources.md
/sql     schema.sql, queries.sql
/docs    screenshot hasil query
README.md
```

## 5. Cara Menjalankan
1. Install MySQL Community Server dan MySQL Workbench (versi yang dipakai: `<isi versi>`).
2. Jalankan `sql/schema.sql`.
3. Impor CSV di `/data` lewat Table Data Import Wizard.
4. Jalankan `sql/queries.sql` per bagian.

## 6. Pertanyaan Bisnis dan Temuan
> Isi hanya dengan hasil query kamu sendiri. Jangan menulis temuan sebelum datanya ada.

| No | Pertanyaan | Query | Temuan |
|---|---|---|---|
| Q1 | Paket prabayar < Rp100.000 termurah? | queries.sql | `<isi>` |
| Q2 | 10 paket dengan harga per GB terendah? | queries.sql | `<isi>` |
| Q7 | Rata-rata harga per GB per brand? | queries.sql | `<isi>` |
| Q11 | Rentang harga bulanan per kelas kuota per grup operator? | queries.sql | `<isi>` |
| Q13 | Selisih harga paket sama antar usia kartu Telkomsel? | queries.sql | `<isi>` |

Screenshot hasil ada di `/docs`.

## 7. Insight dan Rekomendasi
`<2-3 rekomendasi yang diturunkan dari temuan, mis. celah harga di kelas kuota tertentu>`

## 8. Keterbatasan
- Data diambil pada tanggal tertentu; harga paket dapat berubah.
- Sampel nomor untuk harga personal terbatas dan tidak mewakili seluruh pelanggan.
- Normalisasi 30 hari mengasumsikan kuota proporsional terhadap durasi.
- Hanya paket prabayar kuota utama; benefit lain (aplikasi, kuota malam) tidak dibobotkan.

## 9. Rencana Lanjutan
- Tambah tabel data pasar BPS (penetrasi internet per provinsi) untuk latihan JOIN.
- Dashboard di Power BI 
- Survei outlet (RO) untuk melengkapi sisi lapangan.
