# Analisis Kekeringan Meteorologis SPI-12 (Tahunan)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

## Deskripsi Proyek

Repositori ini memuat implementasi kode Python untuk menghitung **Standardized Precipitation Index 12-Bulanan (SPI-12)** di Provinsi Jambi.

SPI-12 dirancang untuk memantau anomali curah hujan kumulatif selama satu tahun penuh. Indeks ini adalah indikator yang sangat baik untuk **kekeringan hidrologis**, yang berdampak pada penurunan volume air di waduk, danau, dan level air tanah dalam jangka panjang.

## Metodologi: Temporal Buffering (12 Bulan)

Karena perhitungan SPI-12 membutuhkan data historis 12 bulan ke belakang, tantangan utamanya adalah menghindari hilangnya data (*data gap*) di awal tahun target.
* Contoh: Untuk menghitung SPI bulan **Januari 2010**, diperlukan data mulai **Februari 2009 hingga Januari 2010**.

Solusi dalam script ini adalah mekanisme **Temporal Buffering**:
1.  Saat memproses suatu periode (misal: `2010-2011`), script akan memuat data tambahan dari tahun sebelumnya (`2009`).
2.  Perhitungan SPI dilakukan pada gabungan data `2009-2011`.
3.  Hasilnya dipotong (*sliced*) hanya untuk rentang `2010-2011` untuk disimpan, sehingga bulan Januari 2010 memiliki nilai SPI yang valid.

## Klasifikasi Kekeringan

Nilai SPI diklasifikasikan menjadi 5 kelas tingkat kekeringan berdasarkan standar BMKG/WMO:

| Nilai SPI | Kelas Kekeringan | Label |
| :--- | :--- | :--- |
| $\geq$ -1.00 | **Tidak Ada Kekeringan** | 1 (Biru) |
| -0.99 s.d 0.99 | **Mendekati Normal** | 2 (Hijau Muda) |
| -1.00 s.d -1.49 | **Agak Kering** | 3 (Kuning) |
| -1.50 s.d -1.99 | **Kering** | 4 (Oranye) |
| $\leq$ -2.00 | **Sangat Kering** | 5 (Merah) |

*> Catatan: Dalam script ini, nilai SPI positif (Basah) disederhanakan menjadi satu kelas "Tidak Ada Kekeringan" karena fokus studi adalah bencana kekeringan.*

## Struktur Direktori Data

Script dirancang untuk eksekusi di **Google Colab** dengan asumsi struktur Google Drive berikut:

```text
/My Drive/Colab Notebooks/Skripsi/
├── Batas Adm Jambi/            # Shapefile Batas Administrasi
├── CHIRPS v3/
│   └── bias_corrected_regression/
│       └── downscaled_1km_metric/  # Input: Raster CHIRPS
└── SPI12-Output/               # Output: Folder hasil

```

## Prasyarat Instalasi

Pustaka geospasial yang dibutuhkan:

```bash
pip install rioxarray xarray geopandas rasterio scipy leafmap

```

## Cara Penggunaan

1. Pastikan data CHIRPS 1km (termasuk tahun buffer) tersedia.
2. Jalankan script `analisis_spi12.ipynb`.
3. Script akan berjalan secara sekuensial per periode (batch processing).
4. Hasil berupa GeoTIFF (nilai kontinu SPI dan kelas diskrit) serta Shapefile (.shp) tersimpan di folder `SPI12-Output`.

## Penulis

**Jariyan Arifudin** Mahasiswa Geografi Lingkungan

Universitas Gadjah Mada (UGM)

## Lisensi & Sitasi

Kode ini didistribusikan di bawah **MIT License**.
Jika Anda menggunakan metode ini untuk penelitian, silakan sitasi repositori ini:

> Arifudin, J. (2026). *Analisis Kekeringan Meteorologis (SPI-12) Provinsi Jambi*. GitHub Repository.
