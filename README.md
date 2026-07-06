# 🧬 Static Analysis Laboratory

> “Understanding binaries without executing them.”

---

## 🔬 Overview

Praktikum ini berfokus pada **Static Analysis terhadap binary Windows (PE - Portable Executable)** menggunakan pendekatan reverse engineering modern.

Tujuan utama dari lab ini adalah untuk memahami bagaimana sebuah program bekerja hanya melalui struktur internalnya, tanpa mengeksekusi binary secara langsung.

Analisis dilakukan dengan pendekatan **intel-based reverse engineering**, mencakup pembacaan struktur PE, string analysis, hingga dekompilasi logika program.

---

## 🎯 Objectives

- Memahami struktur internal **PE (Portable Executable)**.
- Menganalisis perilaku program tanpa eksekusi.
- Mengidentifikasi indikator penting dalam binary (strings, functions, imports).
- Mereconstruct logika program melalui hasil decompiler.
- Melatih kemampuan reading assembly-level abstraction dari Ghidra.

---

## 🧠 Methodology

Analisis dilakukan menggunakan pendekatan **layered static analysis**, dengan tahapan sebagai berikut:

### 1. File Identification Layer
Mengidentifikasi tipe binary, arsitektur, serta karakteristik dasar file menggunakan tools seperti `file` dan Detect It Easy.

### 2. Metadata & Structure Layer
Menganalisis struktur PE, section, entry point, serta import table untuk memahami konteks eksekusi program.

### 3. String Reconnaissance Layer
Melakukan ekstraksi string untuk menemukan indikasi logika program seperti:
- input validation
- credential check
- error message
- hidden flag format

### 4. Control Flow Discovery
Menggunakan **Cross Reference (XREF)** untuk menemukan hubungan antara string dan fungsi yang menggunakannya.

### 5. Decompilation Layer
Menganalisis pseudocode hasil decompiler Ghidra untuk memahami logika program pada level high-level C-like representation.

### 6. Code Refinement
Melakukan:
- Function Signature correction
- Variable renaming
- readability enhancement

agar struktur program lebih mudah dianalisis.

---

## 🛠️ Toolset

- Ghidra (Primary Reverse Engineering Tool)
- Detect It Easy (DIE)
- PE Structure Analyzer (PE-bear / CFF Explorer)
- Strings Utility
- Binary inspection tools

---

## 🧩 Analysis Focus

Setiap binary dianalisis dengan fokus pada:

- Input validation logic
- Credential / flag checking mechanism
- Hidden conditional branches
- Hardcoded values
- Function call relationships
- Security weakness patterns

---

## 📊 Output of Analysis

Hasil akhir dari setiap analisis mencakup:

- Pemahaman struktur program
- Rekonstruksi logika pseudo-code
- Identifikasi kondisi validasi
- Penemuan input/flag yang benar
- Insight terhadap teknik proteksi sederhana dalam binary

---

## 🚀 Insight

Static analysis bukan hanya tentang membaca kode, tetapi tentang:

> “Membongkar logika tersembunyi yang tidak pernah dimaksudkan untuk dibaca manusia biasa.”

---

## 🧾 Conclusion

Lab ini memberikan pemahaman mendalam tentang bagaimana sebuah executable Windows bekerja dari dalam.

Dengan pendekatan static analysis, kita dapat merekonstruksi logika program secara utuh hanya dari struktur binary, tanpa perlu menjalankannya sama sekali.

Ini menjadi fondasi penting dalam bidang:
- Reverse Engineering
- Malware Analysis
- Binary Exploitation
- Application Security Research
