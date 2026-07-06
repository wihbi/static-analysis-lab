# 🦠 Malware Analysis Write-Up: Emotet / Heodo Payload

## 📝 Deskripsi Singkat
Analisis ini berfokus pada sampel *executable* Windows berbahaya yang teridentifikasi sebagai **Emotet** (juga dikenal sebagai Heodo). Emotet dikenal secara luas sebagai *botnet* dan *malware downloader* tingkat lanjut yang bertugas mendistribusikan *payload* sekunder (seperti *ransomware* atau trojan perbankan) setelah berhasil mendapatkan akses awal ke sistem target.

---

## 🔍 1. Informasi Hash (Indicators of Compromise)
Identifikasi kriptografik dari sampel untuk keperluan *tracking* dan *hunting*:

* **SHA-256:** `c64ca381d3329fbaea7e63fa5dd2a07c60ca3e267c882121e34837074fd81ac9`
* **SHA3-384:** `1fce68ddbaa1b09c1808b27266668426978e89cbba93d92bae2217a524959af38fae6fd172c7ab7b31dd97d40c6ec23b`
* **Kategori:** Trojan / Downloader / Botnet

---

## 📁 2. Tipe File
* **Format:** `PE32 executable (GUI) Intel 80386, for MS Windows`
* **Ekstensi Asli:** `.exe`
* **Karakteristik:** Berjalan di arsitektur 32-bit (kompatibel dengan sistem x64). Sampel ini memanfaatkan *packer* kustom untuk menghindari deteksi analisis statis pada tahap awal eksekusi.

---

## 🧵 3. Analisis Strings
Ekstraksi *strings* secara statis tidak langsung mengekspos URL Command & Control (C2) atau fungsi *malicious* yang eksplisit akibat penggunaan teknik kompresi dan *obfuscation*. Karakteristik *strings* pada sampel ini meliputi:

* **Junk / Random Strings:** Ditemukan banyak *strings* acak yang sengaja disisipkan untuk mengelabui analisis heuristik dan menyembunyikan alur logika utama.
* **API Obfuscation:** Nama-nama fungsi Windows API yang kritis tidak terlihat jelas di dalam blok *strings*. Resolusi API dilakukan secara dinamis saat proses berjalan *(runtime)* menggunakan teknik seperti pemanggilan `LoadLibrary` dan `GetProcAddress`.
* **Indikasi Packer/Cryptor:** Rasio *strings* yang tidak proporsional dibandingkan dengan ukuran file secara keseluruhan menandakan bahwa *payload* inti dienkripsi di dalam *section* `.data` atau `.rsrc`.

---

## 🔗 4. Import Table (IAT Analysis)
Meskipun diselubungi oleh *packer*, observasi pada Import Address Table (IAT) dan API yang di-*resolve* secara dinamis di memori mengindikasikan kapabilitas inti dari *malware*:

* **`KERNEL32.dll`:**
    * `LoadLibraryA` / `GetProcAddress`: Digunakan untuk memuat modul DLL lain dan memanggil fungsi secara dinamis.
    * `VirtualAlloc` / `VirtualProtect`: Indikator kuat adanya teknik *process hollowing* atau *unpacking* langsung ke dalam memori.
    * `CreateProcessA`: Digunakan untuk memijahkan (spawn) *child process* atau menjalankan *payload* sekunder.
* **`ADVAPI32.dll`:**
    * Terdapat fungsi terkait manipulasi *Registry* (`RegOpenKey`, `RegSetValueEx`) yang digunakan untuk memastikan persistensi, biasanya dengan menambahkan entri pada *Run key* di sistem Windows.
* **`WININET.dll` / `WS2_32.dll`:**
    * `InternetOpen`, `InternetConnect`, `HttpSendRequest`: Digunakan untuk membangun komunikasi dengan server C2 *(Command and Control)* melalui port HTTP/HTTPS untuk menerima instruksi atau mengunduh modul tambahan.

---

## 💡 5. Kesimpulan
Berdasarkan analisis statis dan observasi perilaku awal, sampel dengan hash `c64ca381d3329fbaea7e63fa5dd2a07c60ca3e267c882121e34837074fd81ac9` merupakan varian dari **Emotet**. 

*Malware* ini membungkus dirinya menggunakan *packer/cryptor* untuk menyembunyikan *strings* dan membatasi eksposur pada *Import Table*, yang mana secara efektif mempersulit analisis statis (*static analysis*). Saat dieksekusi, *malware* akan mengalokasikan ruang memori baru, melakukan *unpacking payload* aslinya, membangun persistensi melalui *registry*, dan melakukan *beaconing* ke infrastruktur C2.

Mengingat karakteristiknya yang dinamis, disarankan untuk melanjutkan analisis menggunakan pendekatan dinamis (menggunakan *debugger* seperti GDB, x64dbg, atau IDA) guna melakukan ekstraksi *memory dump* *(unpacking)* dan meneliti logika *payload* inti secara lebih komprehensif.
