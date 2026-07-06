# 🔬 Static Analysis Summary

Analisis statis dilakukan terhadap file **WannaCry.EXE** untuk memahami struktur dan perilaku malware tanpa mengeksekusinya. Fokus utama analisis adalah mengidentifikasi karakteristik file, struktur PE, string penting, serta indikasi awal fungsi berbahaya.

---

## 📌 Identifikasi File

Hasil pemeriksaan menggunakan `file` menunjukkan bahwa sampel merupakan:

- Format: **PE32 Executable**
- Arsitektur: **Intel x86 (32-bit)**
- Tipe: **Windows Executable**
- Status: **Not Stripped**
- Compiler: **Microsoft Visual C/C++**

Hal ini menunjukkan bahwa binary masih memiliki informasi simbol yang cukup untuk dianalisis lebih lanjut.

---

## 🔍 Analisis Struktur PE

Dari hasil pemeriksaan menggunakan tools seperti **Detect It Easy (DIE)** dan hex analysis:

- File menggunakan format standar **Portable Executable (PE)**
- Terdapat section umum seperti:
  - `.text` (kode program)
  - `.data` (data terinisialisasi)
  - `.rdata` (string & konstanta)
- Terdapat indikasi **packed/compressed section (.rsrc)**

Hal ini mengindikasikan adanya kemungkinan payload tersembunyi dalam resource section.

---

## 🔤 Analisis String

Hasil ekstraksi string menunjukkan pola aktivitas ransomware:

- Target file:
  - `.doc`, `.docx`, `.xls`, `.xlsx`, `.pdf`, `.jpg`, `.zip`
- Ekstensi hasil enkripsi:
  - `.wnry`
- API Windows yang digunakan:
  - `CreateFile`
  - `WriteFile`
  - `MoveFile`
  - `RegSetValueExA`

String tersebut menunjukkan bahwa malware melakukan **file enumeration dan manipulasi sistem secara langsung**.

---

## 🧠 Indikasi Awal Perilaku

Berdasarkan hasil static analysis, dapat disimpulkan bahwa:

- Malware memiliki karakteristik **ransomware**
- Melakukan akses, modifikasi, dan enkripsi file pengguna
- Menggunakan API Windows untuk manipulasi file dan registry
- Mengubah ekstensi file menjadi `.wnry` setelah enkripsi
- Memiliki potensi komunikasi jaringan untuk mendukung serangan

---

## 📊 Kesimpulan Static Analysis

Hasil analisis statis menunjukkan bahwa file **WannaCry.EXE** merupakan malware tipe **ransomware** yang menargetkan file pengguna pada sistem Windows.

Melalui struktur PE, string analysis, dan API reference, dapat diidentifikasi bahwa malware memiliki kemampuan untuk:

- Mengakses file sistem
- Melakukan enkripsi massal
- Mengubah ekstensi file
- Memodifikasi registry Windows

Static analysis ini berhasil memberikan gambaran awal mengenai perilaku malware tanpa perlu mengeksekusi file.
