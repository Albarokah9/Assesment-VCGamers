# Rekap Analisis Task QA Engineer Intern - VCGamers

Dokumen ini berisi rangkuman analisis fitur *Search Produk di VCGamers* berdasarkan PDF Assessment dan observasi pengujian langsung di aplikasi.

## 1. Ringkasan Assessment
- **Tujuan:** Menilai kemampuan dalam memahami requirement, menentukan scope, membuat test case (min. 5), dan menemukan bug/gap.
- **Fitur Utama:** Search Produk di VCGamers (pencarian game/produk).
- **Target Platform:** [vcgamers.com](https://www.vcgamers.com).
- **Output Diminta:** Tabel Test Case (No, Test Case, Expected Result, Status, Notes) dan Laporan Bug / Celah UX.

## 2. Peta Perilaku Sistem (Behavior Map) - Hasil Observasi
Berdasarkan pengujian eksploratori, berikut adalah perilaku (behavior) fitur pencarian:

### A. Initial State Behavior (Fase Pra-Pencarian)
*   **Trigger:** Mengklik kolom *search bar* di halaman Beranda.
*   **Behavior Aktual:** Area di bawah kolom pencarian dibiarkan putih kosong.
*   **Celah UX (Enhancement Request):** Disarankan agar area kosong diisi *Recent Searches* (Riwayat) atau *Trending Products* (Populer).

### B. Positive / Happy Path Behavior (Fase Pencarian Normal)
*   **Behavior Aktual:** Memiliki *Instant Search* (hasil *real-time*), rapi terklasifikasi, dan ada *Keyword Highlighting* (validasi relevansi warna).

### C. Negative & Security Behavior (Fase Keamanan & Respons Error)
*   **Behavior Aktual:** Sistem kebal terhadap *payload* SQL Injeksi (`1' OR '1'='1`) dan merespon dengan *Empty State* ramah yang berisi tombol *"Kembali ke Home"*.

### D. Functional Recovery Behavior (Fase Pembatalan Aksi)
*   **Behavior Aktual:** Eksekusi klik tombol 'X' memecah pencarian (*cleared*) dan mengembalikan *state* antarmuka pra-pencarian secara instan.

### E. String Validation & Trimming Behavior (Cacat Spasi - Bug)
*   **Behavior Aktual:** Mengenali manipulasi *case-insensitive* ("free fire") dan mengeliminasi *trailing space* dengan sempurna. 
*   **Kelemahan (Edge Case):** Hanya toleran maksimal **3 spasi** untuk awalan/tengah kata. Spasi ke-4 akan merusak fungsi konversi algoritma pencarian sehingga data produk gagal terdeteksi (Bug / Cacat Logika).

### F. Navigation & Routing Behavior (Fase Integrasi Checkout)
*   **Trigger:** Mengeklik salah satu pratinjau produk ("Free Fire (Top Up Game)") dari ruang hasil pencarian.
*   **Behavior Aktual:** Mengarahkan rute halaman ke URL rincian spesifik target (contoh: `https://www.vcgamers.com/gercep/free-fire/top-up-game?from=search`). 
*   **Konteks QA Analytics:** Penyematan param URL `?from=search` di ekor *link* sangat presisi untuk keperluan *Data Analytics* agar *Product Manager* tahu bahwa transaksi konversi ini murni bersumber dari pintu *Search Engine*. Integrasi navigasi berjalan sukses dan tidak tersesat (*no 404 error*).

## 3. Strategi Penyusunan Test Case (Implementasi SQA)
Dengan membedah peta pola di atas, *Test Case* dapat dirancang sangat kuat menggunakan teknik Ekuivalen dan Nilai Batas (*Boundary Value Analysis*):
1. **Positive Test:** Validasi akurasi *Instant Search* dan keberhasilan navigasi ke Halaman *Checkout* (R-7).
2. **Negative Test (Security):** Menyuntikkan SQL Injeksi `1' OR '1'='1` untuk menguji parameter lapisan sanitasi.
3. **Negative Test (Logic Bug):** Investigasi kelemahan filter spasi input di bagian awalan (*Leading spaces* > 3 kali).
4. **Boundary Test:** Eksekusi spasi tak terhingga di ujung kalimat pencarian.
5. **Usability Test:** Penilaian absennya ornamen rekomendasi tren di kanvas peluncur *Initial Search State* sebagai saran keunggulan UI.

---
*Dokumen ini diperbarui secara komprehensif sebagai kompas pemetaan skenario pengerjaan The Assessment of QA Engineer Intern VCGamers.*
