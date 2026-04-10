# Test Cases & Test Scenarios — VCGamers Search Produk

> **Project:** Assessment QA Engineer Intern — VCGamers  
> **Fitur:** Search Produk ([vcgamers.com](https://www.vcgamers.com))  
> **Dibuat oleh:** QA Engineer  
> **Tanggal:** 10 April 2026  
> **Referensi:** `Assesment QA Engineer Intern.pdf`, `REKAP_ANALISIS_QA.md`, Observasi Langsung

---

## Traceability Matrix (Requirements Mapping)

Matriks berikut memastikan bahwa ke-8 *Requirement* yang disyaratkan dalam *Brief* Assessment telah ter-cover seluruhnya oleh serangkaian Skenario Pengujian (Test Case).

| Req ID | Requirement | Test Case ID | Status Coverage |
| :--- | :--- | :--- | :--- |
| **R-1** | User dapat memasukkan kata kunci pada kolom search. | TC-UX-001, TC-F-001 | ✅ Covered |
| **R-2** | User dapat menjalankan search (termasuk via tombol Enter/Klik). | TC-F-001 | ✅ Covered |
| **R-3** | Sistem menampilkan halaman hasil pencarian setelah search dijalankan. | TC-F-001, TC-F-002 | ✅ Covered |
| **R-4** | Keyword yang dicari tetap terlihat setelah hasil pencarian tampil. | TC-F-003 | ✅ Covered |
| **R-5** | Sistem menampilkan hasil yang relevan dengan keyword. | TC-F-001, TC-F-002 | ✅ Covered |
| **R-6** | Jika tidak ada hasil, sistem menampilkan empty state yang jelas. | TC-N-001, TC-SEC-001 | ✅ Covered |
| **R-7** | User dapat membuka detail produk dari hasil pencarian. | TC-F-004, TC-F-005 | ✅ Covered |
| **R-8** | Search tidak boleh error/blank/rusak walaupun input tidak biasa. | TC-E-001 s/d TC-E-004, TC-SEC-001 | ✅ Covered |

---

## Daftar Test Scenario

| No | Scenario ID | Nama Skenario | Jumlah TC |
| :--- | :--- | :--- | :--- |
| 1 | TS-INIT | Initial State — Tampilan Awal Search Bar | 1 |
| 2 | TS-HAPPY | Happy Path — Pencarian Normal & Akurat | 2 |
| 3 | TS-SEC | Security — Ketahanan Terhadap Injeksi SQL | 1 |
| 4 | TS-RECOVERY | Functional Recovery — Pembatalan Pencarian | 1 |
| 5 | TS-NEG | Negative Testing — Pencarian Data Tidak Valid (Empty State) | 1 |
| 6 | TS-TRIM | String Validation & Trimming — Toleransi Spasi | 4 |
| 7 | TS-NAV | Navigation & Routing — Integrasi ke Halaman Produk | 2 |
| | | **Total** | **12** |

---

## Scenario 1: Initial State — Tampilan Awal Search Bar (TS-INIT)

Skenario ini memverifikasi apa yang terjadi ketika pengguna mengklik kolom pencarian tanpa mengetik apapun. Tujuannya untuk menilai kesiapan antarmuka dan pengalaman pengguna (UX) sebelum pencarian dimulai.

---

## TC-ID: TC-UX-001: Validasi Tampilan Awal Saat Search Bar Diklik Tanpa Input

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-INIT / Behavior Map Section A |
| **Priority** | MEDIUM |
| **Test Type** | UI/UX |
| **Environment** | DEVELOPMENT |
| **Notes** | 📌 **PM/UX:** Saat ini, jika user tidak punya riwayat pencarian, area di bawah search bar tampil kosong tanpa rekomendasi trending. Pertimbangkan untuk menambahkan fitur "Produk Populer" atau "Trending Searches" sebagai fallback agar user tetap mendapat panduan saat initial state. |

### Preconditions
User sudah berada di halaman utama VCGamers (`https://www.vcgamers.com`), browser dalam kondisi bersih (tanpa riwayat pencarian sebelumnya).
Test Data: Browser:
* Google Chrome (versi terbaru)
* Cache dan cookies sudah dihapus

### Test Steps
Buka halaman utama VCGamers di browser

Cari dan klik kolom search bar yang ada di bagian atas halaman

Jangan ketik apapun — biarkan kolom pencarian kosong

Amati area di bawah kolom pencarian, apakah muncul elemen seperti "Pencarian Terakhir" (Recent Searches), "Produk Populer" (Trending), atau rekomendasi lainnya

Verifikasi bahwa tampilan tidak menunjukkan ruang kosong tanpa informasi apapun

### Expected Result
> Saat search bar diklik tanpa mengetik apapun, sistem mengarahkan pengguna ke halaman `/search` dan menampilkan area yang informatif — misalnya daftar "Pencarian Terakhir" jika ada riwayat, atau saran pencarian populer. Area di bawah kolom pencarian tidak boleh dibiarkan kosong tanpa konten yang berguna bagi pengguna.

### Actual Result
> Sistem mengarahkan ke halaman `/search`. Jika ada riwayat pencarian, muncul section "Pencarian Terakhir" lengkap dengan tombol "Hapus Semua". Jika tidak ada riwayat, area tampil relatif kosong tanpa rekomendasi produk trending.

<br>

---

## Scenario 2: Happy Path — Pencarian Normal & Akurat (TS-HAPPY)

Skenario ini menguji alur pencarian standar di mana pengguna mengetik kata kunci yang valid dan dapat menghasilkan output pencarian. Fokus utama: akurasi pencarian, kecepatan respons (instant search), dan relevansi klasifikasi hasil.

---

## TC-ID: TC-F-001: Pencarian Produk dengan Kata Kunci Valid (Instant Search)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-HAPPY / Behavior Map Section B |
| **Priority** | HIGH |
| **Test Type** | Functional |
| **Environment** | DEVELOPMENT |
| **Notes** | — Tidak ada catatan khusus. Fitur instant search berjalan sesuai ekspektasi. |

### Preconditions
User berada di halaman utama atau halaman `/search` VCGamers.
Test Data: Kata Kunci:
* "free fire"

### Test Steps
Klik kolom search bar di halaman VCGamers

Ketikkan kata kunci "free fire" pada kolom pencarian

Amati apakah hasil pencarian muncul secara otomatis (real-time / instant search) tanpa perlu menekan tombol Enter

Verifikasi bahwa hasil pencarian terklasifikasi ke dalam kategori yang rapi (misalnya: "Rekomendasi" dan "Produk")

Verifikasi bahwa kata kunci yang diketik ("free fire") ditandai secara visual (keyword highlighting) di dalam setiap item hasil pencarian

Verifikasi bahwa produk yang muncul relevan dengan kata kunci yang dimasukkan (contoh: "Free Fire Top Up Game", "Free Fire MAX", "Akun Game Free Fire")

### Expected Result
> Segera setelah pengguna mengetik "free fire", sistem menampilkan daftar hasil pencarian secara instan (tanpa reload halaman). Hasil terklasifikasi dalam dua kategori utama yaitu "Rekomendasi" dan "Produk". Kata kunci "free fire" ter-highlight secara visual di setiap hasil, dan semua produk yang ditampilkan relevan dengan kata kunci tersebut.

### Actual Result
> Instant search aktif dan hasil muncul segera setelah pengguna mengetik. Hasil terklasifikasi dalam section "Rekomendasi" dan "Produk". Keyword highlighting berfungsi, menampilkan "FREE FIRE" dengan warna/bobot yang berbeda. Produk yang tampil termasuk "Free Fire (Top Up Game)", "Free Fire MAX", dan "Akun Game".

<br>

---

## TC-ID: TC-F-002: Pencarian dengan Variasi Huruf Besar-Kecil (Case-Insensitive)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-HAPPY / Behavior Map Section E |
| **Priority** | MEDIUM |
| **Test Type** | Functional |
| **Environment** | DEVELOPMENT |
| **Notes** | — Tidak ada catatan khusus. Case-insensitive search berfungsi dengan benar di semua variasi. |

### Preconditions
User berada di halaman `/search` VCGamers.
Test Data: Variasi Kata Kunci:
* "free fire" (huruf kecil semua)
* "FREE FIRE" (huruf besar semua)
* "Free Fire" (campuran)
* "fReE fIrE" (acak)

### Test Steps
Ketikkan "free fire" pada kolom pencarian, catat jumlah dan daftar produk yang muncul

Hapus kolom pencarian, lalu ketikkan "FREE FIRE", catat hasilnya

Ulangi langkah yang sama untuk "Free Fire" dan "fReE fIrE"

Bandingkan seluruh hasil pencarian dari keempat variasi tersebut

Verifikasi bahwa jumlah dan jenis produk yang muncul identik untuk semua variasi penulisan huruf besar-kecil

### Expected Result
> Sistem pencarian bersifat case-insensitive, sehingga seluruh variasi penulisan huruf besar dan kecil menghasilkan daftar produk yang sama persis. Tidak ada perbedaan hasil antara "free fire", "FREE FIRE", "Free Fire", maupun "fReE fIrE".

### Actual Result
> Sistem berhasil mengenali semua variasi huruf besar-kecil dan mengembalikan hasil pencarian yang identik untuk setiap variasi. Case-insensitive berfungsi dengan benar.

<br>

---

## Scenario 3: Security — Ketahanan Terhadap Injeksi SQL (TS-SEC)

Skenario ini menguji apakah kolom pencarian mampu bertahan dari serangan injeksi SQL. Penting untuk memastikan bahwa input berbahaya ditangani dengan aman dan tidak mengekspos data sensitif atau merusak sistem.

---

## TC-ID: TC-SEC-001: Injeksi SQL Melalui Kolom Pencarian

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-SEC / Behavior Map Section C |
| **Priority** | URGENT |
| **Test Type** | Security |
| **Environment** | DEVELOPMENT |
| **Notes** | 📌 **BE/Security:** Input sanitization berjalan baik untuk payload basic SQL injection. Disarankan untuk melakukan pengujian lanjutan dengan payload yang lebih kompleks (Blind SQL Injection, UNION-based) dan XSS payload (`<script>alert(1)</script>`) untuk memastikan keamanan menyeluruh. |

### Preconditions
User berada di halaman `/search` VCGamers.
Test Data: Payload Injeksi:
* `1' OR '1'='1`

### Test Steps
Klik kolom search bar di halaman VCGamers

Ketikkan payload injeksi SQL: `1' OR '1'='1`

Amati respons sistem terhadap input injeksi tersebut

Verifikasi bahwa sistem TIDAK menampilkan data yang tidak seharusnya terlihat (misalnya: seluruh database produk atau error teknis berupa pesan SQL)

Verifikasi bahwa sistem menampilkan pesan "Empty State" yang ramah pengguna, misalnya "Tidak ada yang cocok dengan Keyword ini"

Verifikasi bahwa terdapat tombol navigasi "Kembali ke Home" pada tampilan empty state sebagai jalan keluar bagi pengguna

### Expected Result
> Sistem memperlakukan payload SQL injection sebagai string biasa (literal) dan melakukan sanitasi input dengan benar. Tidak ada data sensitif yang bocor, tidak ada error teknis yang tampil. Sistem menampilkan halaman "Empty State" dengan pesan informasi "Tidak ada yang cocok dengan Keyword ini" beserta tombol "Kembali ke Home" untuk mengarahkan pengguna kembali ke halaman utama.

### Actual Result
> Sistem kebal terhadap payload SQL injection. Input `1' OR '1'='1` diperlakukan sebagai string pencarian biasa. Tampil halaman empty state dengan pesan "Tidak ada yang cocok dengan Keyword ini" dan tombol "Kembali ke Home" serta saran kategori produk. Tidak ada error teknis atau data bocor.

<br>

---

## Scenario 4: Functional Recovery — Pembatalan Pencarian (TS-RECOVERY)

Skenario ini memverifikasi apakah pengguna bisa membatalkan pencarian yang sedang berlangsung dan kembali ke keadaan awal dengan mulus, tanpa sisa-sisa data pencarian sebelumnya yang mengganggu.

---

## TC-ID: TC-F-003: Pembatalan Pencarian dengan Tombol X (Clear)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-RECOVERY / Behavior Map Section D |
| **Priority** | HIGH |
| **Test Type** | Functional |
| **Environment** | DEVELOPMENT |
| **Notes** | — Tidak ada catatan khusus. Tombol clear berfungsi dengan baik dan UI recovery berjalan mulus. |

### Preconditions
User sudah melakukan pencarian dengan kata kunci apapun (misalnya "free fire") dan hasil pencarian sedang ditampilkan di layar.
Test Data: Kata Kunci Awal:
* "free fire"

### Test Steps
Pastikan kolom pencarian sudah berisi kata kunci "free fire" dan hasil pencarian instant sudah muncul di layar

Klik tombol "X" (ikon clear/hapus) yang muncul di bagian kanan kolom pencarian

Verifikasi bahwa teks di dalam kolom pencarian langsung terhapus dan kolom menjadi kosong

Verifikasi bahwa hasil pencarian sebelumnya (daftar produk "Free Fire") sudah tidak tampil lagi

Verifikasi bahwa antarmuka kembali ke kondisi pra-pencarian (Initial State), misalnya menampilkan "Pencarian Terakhir"

### Expected Result
> Saat tombol "X" diklik, kolom pencarian langsung bersih dari teks, hasil pencarian sebelumnya langsung menghilang, dan tampilan antarmuka kembali ke kondisi pra-pencarian (menampilkan "Pencarian Terakhir" atau area kosong informatif) secara instan tanpa jeda atau glitch visual.

### Actual Result
> Tombol "X" bekerja dengan sempurna. Kolom pencarian langsung kosong, daftar hasil pencarian hilang, dan UI kembali ke state "Pencarian Terakhir" secara instan.

<br>

---

## Scenario 5: Negative Testing — Pencarian Data Tidak Valid / Empty State (TS-NEG)

Skenario ini memverifikasi perilaku sistem ketika pengguna mencari produk yang tidak ada dalam database. Tujuannya untuk memastikan bahwa sistem tidak menampilkan error teknis, melainkan memberikan antarmuka Empty State yang informatif dan ramah pengguna — sesuai Requirement R-6.

---

## TC-ID: TC-N-001: Validasi Tampilan Empty State Saat Pencarian Produk Tidak Ditemukan

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-NEG / Requirement R-6 |
| **Priority** | HIGH |
| **Test Type** | Negative |
| **Environment** | DEVELOPMENT |
| **Notes** | — Tampilan empty state sudah sangat komprehensif: pesan informatif, tombol navigasi kembali, opsi kirim saran, dan rekomendasi kategori alternatif. Memenuhi standar UX terbaik untuk penanganan *dead-end search*. |

### Preconditions
User berada di halaman utama atau halaman `/search` VCGamers.
Test Data: Kata Kunci Tidak Valid (produk yang dipastikan tidak ada):
* "testing data invalid"
* "xyzabc123produk"
* "barang tidak ada di dunia"

### Test Steps
Klik kolom search bar di halaman VCGamers

Ketikkan kata kunci yang dipastikan tidak memiliki hasil di database, misalnya "testing data invalid"

Tekan tombol Enter atau tunggu proses Instant Search selesai

Verifikasi bahwa sistem menampilkan halaman Empty State — bukan halaman error teknis, blank, atau crash

Verifikasi ketersediaan elemen teks utama: **"Tidak ada yang cocok dengan Keyword ini"**

Verifikasi ketersediaan tombol **"Kembali ke Home"** yang berfungsi mengarahkan pengguna kembali ke halaman utama

Verifikasi ketersediaan tombol **"Kirim Saran"** sebagai kanal feedback pengguna

Verifikasi ketersediaan section **"Cari di Kategori ini"** yang menampilkan daftar kategori alternatif (DLC, Top Up via Link, Item dan Skin, Game Gift, Top Up Game, Jasa Joki, Akun Game, Game Coins, Voucher, Pulsa dan PLN, Aplikasi Live, Top Up Via Login)

Klik tombol "Kembali ke Home" dan verifikasi bahwa pengguna berhasil diarahkan kembali ke halaman utama tanpa error

### Expected Result
> Ketika pengguna mencari produk yang tidak ada dalam database, sistem tidak menampilkan error teknis atau halaman kosong. Sebaliknya, sistem menampilkan halaman Empty State yang informatif dan bersahabat, terdiri dari: (1) Pesan "Tidak ada yang cocok dengan Keyword ini", (2) Tombol "Kembali ke Home" untuk navigasi kembali, (3) Tombol "Kirim Saran" untuk feedback, dan (4) Section "Cari di Kategori ini" berisi chip-chip kategori produk alternatif — sehingga pengguna tidak mengalami *dead-end* dan tetap bisa melanjutkan eksplorasi.

### Actual Result
> Sistem menampilkan halaman Empty State yang sangat komprehensif. Terdapat ilustrasi karakter maskot dengan tanda tanya, pesan "Tidak ada yang cocok dengan Keyword ini" tercentral dengan jelas, tombol "Kembali ke Home" dan "Kirim Saran" tersedia dan berfungsi, serta section "Cari di Kategori ini" menampilkan 12 chip kategori alternatif (DLC, Top Up via Link, Item dan Skin, Game Gift, Top Up Game, Jasa Joki, Akun Game, Game Coins, Voucher, Pulsa dan PLN, Aplikasi Live, Top Up Via Login). Tidak ada error teknis, crash, atau blank page.

<br>

---

## Scenario 6: String Validation & Trimming — Toleransi Spasi (TS-TRIM)

Skenario ini menguji bagaimana algoritma pencarian menangani karakter spasi yang tidak biasa — baik di awal (leading), di akhir (trailing), maupun di tengah kata kunci. Ini adalah area di mana bug logika sering ditemukan.

> [!IMPORTANT]
> **Konteks Penting untuk Semua Stakeholder:** QA tidak memiliki akses ke dokumen spesifikasi desain (design spec) yang mendefinisikan perilaku trimming yang diharapkan. Temuan di bawah ini dilaporkan sebagai **Potential Defect** berdasarkan analisis inkonsistensi perilaku — bukan berdasarkan spec. Mohon klarifikasi dari PO/PM apakah batasan ini *by design* atau merupakan defect.

---

## TC-ID: TC-E-001: Pencarian dengan Spasi di Akhir Kata Kunci (Trailing Spaces)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-TRIM / Behavior Map Section E |
| **Priority** | MEDIUM |
| **Test Type** | Functional |
| **Environment** | DEVELOPMENT |
| **Notes** | — Tidak ada catatan khusus. Trailing space trimming berfungsi tanpa batasan jumlah. Ini menjadi **baseline pembanding** untuk inkonsistensi yang ditemukan pada leading dan mid-word spaces. |

### Preconditions
User berada di halaman `/search` VCGamers.
Test Data: Kata Kunci (dengan spasi akhir):
* "free fire   " (3 spasi di belakang)
* "free fire          " (10 spasi di belakang)
* "free fire                    " (20 spasi di belakang)

### Test Steps
Ketikkan "free fire   " (kata kunci diikuti 3 karakter spasi di belakang) pada kolom pencarian, catat hasil yang muncul

Hapus kolom pencarian, lalu ketikkan "free fire          " (10 spasi di belakang), catat hasilnya

Hapus kolom pencarian, lalu ketikkan "free fire                    " (20 spasi di belakang), catat hasilnya

Bandingkan ketiga hasil pencarian tersebut dengan hasil pencarian normal "free fire" tanpa spasi

Verifikasi bahwa semua variasi menghasilkan produk yang sama — tidak ada batasan jumlah spasi di akhir

### Expected Result
> Sistem berhasil mengeliminasi spasi di akhir kata kunci (trailing space trimming) tanpa batasan jumlah spasi. Berapapun jumlah spasi yang ditambahkan di akhir kata kunci, sistem tetap menampilkan hasil pencarian yang identik dengan pencarian "free fire" tanpa spasi. Produk yang muncul relevan dan sesuai.

### Actual Result
> Trailing spaces berhasil di-trim tanpa batasan. Baik 3, 10, maupun 20 spasi di belakang kata kunci, sistem tetap menampilkan hasil pencarian yang identik dengan pencarian normal. Tidak ditemukan batas atas toleransi spasi di akhir kata kunci.

<br>

---

## TC-ID: TC-E-002: Pencarian dengan 1–4 Spasi di Awal Kata Kunci (Leading Spaces — Toleran)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-TRIM / Behavior Map Section E |
| **Priority** | HIGH |
| **Test Type** | Functional |
| **Environment** | DEVELOPMENT |
| **Notes** | 📌 **BE/FE:** Sistem dapat menghandle leading spaces hingga 4 karakter. Namun, berbeda dengan trailing spaces yang tanpa batasan, ini menunjukkan inkonsistensi penanganan. Lihat TC-E-003 untuk detail kegagalan di boundary +1 (5 spasi). Mohon klarifikasi apakah batasan ini *by design* atau perlu diperbaiki. |

### Preconditions
User berada di halaman `/search` VCGamers.
Test Data: Kata Kunci (dengan leading space):
* " free fire" (1 spasi)
* "  free fire" (2 spasi)
* "   free fire" (3 spasi)
* "    free fire" (4 spasi)

### Test Steps
Ketikkan " free fire" (1 spasi di depan) pada kolom pencarian, catat hasil yang muncul

Hapus kolom pencarian, lalu ketikkan "  free fire" (2 spasi di depan), catat hasilnya

Hapus kolom pencarian, lalu ketikkan "   free fire" (3 spasi di depan), catat hasilnya

Hapus kolom pencarian, lalu ketikkan "    free fire" (4 spasi di depan), catat hasilnya

Bandingkan keempat hasil pencarian tersebut dengan hasil pencarian normal "free fire" tanpa spasi

Verifikasi bahwa semua variasi (1–4 spasi) menghasilkan produk yang sama

### Expected Result
> Sistem mampu mentoleransi 1 hingga 4 karakter spasi di awal kata kunci. Algoritma pencarian memotong spasi tersebut (trim) dan mengembalikan hasil pencarian yang identik dengan pencarian tanpa spasi. Semua produk "Free Fire" muncul dengan benar.

### Actual Result
> Leading spaces 1–4 karakter berhasil di-trim oleh sistem. Hasil pencarian identik dengan pencarian "free fire" tanpa spasi. Batas toleransi leading spaces adalah **maksimal 4 spasi tambahan**.

<br>

---

## TC-ID: TC-E-003: Pencarian dengan 5+ Spasi di Awal Kata Kunci (Leading Spaces — Batas Toleransi / Potential Defect)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-TRIM / Behavior Map Section E / Boundary Value Analysis |
| **Priority** | HIGH |
| **Test Type** | Negative |
| **Environment** | DEVELOPMENT |
| **Notes** | ⚠️ **Potential Defect — Needs Clarification dari PM/BE.** Sistem gagal menampilkan hasil pencarian ketika leading spaces ≥ 5. Perilaku ini **inkonsisten** dengan trailing spaces yang berhasil di-trim tanpa batasan (TC-E-001). Jika design spec tidak mengatur batasan khusus, maka ini merupakan defect pada logika trimming. Referensi: Google Search, Tokopedia, Shopee — semua menghandle unlimited leading spaces. |

### Preconditions
User berada di halaman `/search` VCGamers.
Test Data: Kata Kunci (spasi berlebih — melampaui batas toleransi):
* "     free fire" (5 spasi di depan — **boundary +1, di luar toleransi**)
* "      free fire" (6 spasi di depan)
* "          free fire" (10 spasi di depan)

### Test Steps
Ketikkan "     free fire" (5 spasi di depan) pada kolom pencarian, catat apakah produk muncul atau tidak

Hapus kolom pencarian, lalu ketikkan "      free fire" (6 spasi di depan), catat hasilnya

Hapus kolom pencarian, lalu ketikkan "          free fire" (10 spasi di depan), catat hasilnya

Verifikasi apakah produk "Free Fire" masih muncul untuk setiap variasi di atas

Jika produk tidak muncul (empty state), catat pesan error yang ditampilkan oleh sistem — ini mengkonfirmasi kelemahan pada algoritma trimming yang hanya toleran hingga 4 spasi

### Expected Result
> Idealnya, sistem seharusnya mampu memotong spasi berapapun jumlahnya di awal kata kunci dan tetap mengembalikan hasil pencarian yang akurat — konsisten dengan perilaku trailing spaces. Jika gagal menampilkan hasil pada 5+ spasi, maka ini merupakan **potential defect** pada logika trimming yang hanya toleran sampai **maksimal 4 spasi**.

### Actual Result
> Sistem **gagal menampilkan hasil pencarian** ketika jumlah leading spaces melebihi 4 karakter. Pada 5 spasi ke atas, sistem menampilkan empty state alih-alih hasil produk "Free Fire". Ini mengkonfirmasi bahwa algoritma trimming untuk leading spaces memiliki **batas toleransi 4 spasi** — input dengan 5+ spasi di awal tidak di-trim dengan benar, menghasilkan query kosong atau tidak valid.

<br>

---

## TC-ID: TC-E-004: Pencarian dengan Spasi Berlebih di Tengah Kata Kunci (Mid-Word Spaces)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-TRIM / Behavior Map Section E / Boundary Value Analysis |
| **Priority** | MEDIUM |
| **Test Type** | Negative |
| **Environment** | DEVELOPMENT |
| **Notes** | ⚠️ **Potential Defect — Needs Clarification dari PM/BE.** Sistem hanya toleran hingga 3 spasi tambahan di tengah kata kunci (total 4 karakter spasi). Mulai dari 4 spasi tambahan (total 5 karakter), pencarian gagal. Perilaku ini **inkonsisten** dengan trailing spaces yang unlimited. Kemungkinan penyebab: normalisasi spasi di backend menggunakan regex/logika yang berbeda untuk masing-masing posisi. Saran: gunakan `trim()` + `replace(/\\s+/g, ' ')` secara universal sebelum query diproses. |

### Preconditions
User berada di halaman `/search` VCGamers.
Test Data: Kata Kunci (spasi di tengah):
* "free  fire" (2 spasi — dalam batas toleransi)
* "free   fire" (3 spasi — **batas maksimal toleransi**)
* "free    fire" (4 spasi — **boundary +1, di luar toleransi**)
* "free          fire" (10 spasi — jauh melampaui batas)

### Test Steps
Ketikkan "free  fire" (2 spasi di tengah) pada kolom pencarian, catat apakah produk muncul

Hapus kolom, lalu ketikkan "free   fire" (3 spasi di tengah), catat hasilnya — ini adalah **batas maksimal toleransi**

Hapus kolom, lalu ketikkan "free    fire" (4 spasi di tengah), catat hasilnya — ini adalah **boundary +1** yang diharapkan gagal

Hapus kolom, lalu ketikkan "free          fire" (10 spasi di tengah), catat hasilnya

Bandingkan hasil dari semua variasi dengan pencarian normal "free fire" (1 spasi)

Verifikasi bahwa 1–3 spasi tambahan di tengah menghasilkan produk yang sama, dan 4+ spasi di tengah mulai gagal

### Expected Result
> Sistem seharusnya tetap mengenali kata kunci meskipun terdapat spasi berlebih di antara kata. Algoritma pencarian yang baik akan menormalisasi multiple spaces menjadi single space sebelum memproses query. Pada kenyataannya, batas toleransi adalah **3 spasi tambahan di tengah** — 4 spasi ke atas menjadi temuan potential defect.

### Actual Result
> Sistem mampu menangani hingga **3 spasi tambahan di tengah** kata kunci (total 4 karakter spasi termasuk spasi normal) dan tetap menampilkan hasil pencarian yang benar. Namun, mulai dari **4 spasi tambahan** (total 5 karakter spasi), sistem gagal mendeteksi produk dan menampilkan empty state. Ini mengkonfirmasi bahwa algoritma normalisasi spasi di tengah kata kunci memiliki **batas toleransi 3 spasi tambahan**.

<br>

---

## Scenario 7: Navigation & Routing — Integrasi ke Halaman Produk (TS-NAV)

Skenario ini memverifikasi bahwa klik pada hasil pencarian mengarahkan pengguna ke halaman produk yang benar, dengan URL tracking parameter yang tepat untuk keperluan analitik.

---

## TC-ID: TC-F-004: Navigasi dari Hasil Pencarian ke Halaman Detail Produk

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-NAV / Behavior Map Section F |
| **Priority** | HIGH |
| **Test Type** | Functional |
| **Environment** | DEVELOPMENT |
| **Notes** | 📌 **PM/Data Analytics:** Parameter `?from=search` terpasang dengan benar pada URL navigasi. Tim analytics dapat menggunakan parameter ini untuk tracking conversion rate dari fitur pencarian. Pastikan parameter ini tetap dipertahankan di semua rilis mendatang. |

### Preconditions
User sudah melakukan pencarian dengan kata kunci "free fire" dan hasil pencarian instant sudah tampil di layar.
Test Data: Produk Target:
* "Free Fire (Top Up Game)"

### Test Steps
Dari daftar hasil pencarian instant, klik pada item produk "Free Fire (Top Up Game)"

Tunggu hingga halaman detail produk selesai dimuat sepenuhnya

Verifikasi bahwa halaman yang terbuka adalah halaman rincian produk yang benar (bukan halaman 404 atau halaman produk lain)

Periksa URL di address bar browser, pastikan mengandung parameter `?from=search` — parameter ini menandakan bahwa navigasi berasal dari fitur pencarian (digunakan untuk keperluan data analytics)

Contoh URL yang benar: `https://www.vcgamers.com/gercep/free-fire/top-up-game?from=search`

Verifikasi bahwa konten halaman produk (nama, gambar, info top-up) sesuai dengan item yang diklik di hasil pencarian

### Expected Result
> Klik pada item hasil pencarian membawa pengguna ke halaman detail produk yang sesuai tanpa error (tidak ada 404 atau redirect ke halaman yang salah). URL mengandung parameter query `?from=search` yang berfungsi sebagai penanda sumber traffic dari fitur pencarian untuk keperluan data analytics oleh tim Product Manager. Konten halaman produk sesuai dengan produk yang dipilih.

### Actual Result
> Navigasi berhasil. Halaman produk terbuka dengan URL `https://www.vcgamers.com/gercep/free-fire/top-up-game?from=search`. Parameter `?from=search` terpasang dengan benar. Konten halaman sesuai dan tidak ada error 404.

<br>

---

## TC-ID: TC-F-005: Validasi Tidak Ada Broken Link (Error 404) dari Hasil Pencarian

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | TS-NAV / Behavior Map Section F |
| **Priority** | HIGH |
| **Test Type** | Regression |
| **Environment** | DEVELOPMENT |
| **Notes** | 📌 **FE/BE:** Test case ini perlu dieksekusi secara berkala (regression) setiap kali ada perubahan pada routing atau penambahan/penghapusan produk. Satu link yang mengarah ke 404 dapat menurunkan kepercayaan user terhadap fitur pencarian. |

### Preconditions
User sudah melakukan pencarian dengan beberapa kata kunci berbeda yang menghasilkan produk.
Test Data: Kata Kunci:
* "free fire"
* "mobile legends"
* "valorant"

### Test Steps
Cari "free fire", klik produk pertama dari hasil pencarian, verifikasi halaman produk terbuka tanpa error 404

Kembali ke halaman pencarian, cari "mobile legends", klik produk pertama, verifikasi halaman terbuka tanpa error

Kembali ke halaman pencarian, cari "valorant", klik produk pertama, verifikasi halaman terbuka tanpa error

Untuk setiap navigasi, pastikan URL mengandung parameter `?from=search`

Verifikasi bahwa tidak ada satupun klik hasil pencarian yang menghasilkan halaman error 404 atau halaman kosong

### Expected Result
> Seluruh link produk dari hasil pencarian mengarah ke halaman yang valid dan bisa diakses. Tidak ada broken link yang menghasilkan error 404, error 500, atau halaman kosong. Setiap navigasi tetap mempertahankan parameter `?from=search` pada URL.

### Actual Result
> _(Perlu dieksekusi secara menyeluruh untuk setiap kombinasi kata kunci)_

<br>

---

## Bug / Finding Report

Sesuai permintaan dokumen Assessment, berikut format pelaporan isu/celah bug *Logic* dan Inkonsistensi yang dijumpai selama observasi *Edge Case*.

### Bug 1: Inkonsistensi Toleransi "Trim Spaces" yang Mengakibatkan Kegagalan Pencarian Produk

*   **Title**: Algoritma Pencarian Gagal Membaca Keyword Jika Terdapat 5+ Spasi Awalan (*Leading*) atau 4+ Spasi Tengah (*Between Words*)
*   **Steps to Reproduce**:
    1. Buka halaman utama website `https://www.vcgamers.com/`.
    2. Klik kolom Search pencarian.
    3. Ketikkan keyword awalan dengan **5 spasi**. Contoh: `[space][space][space][space][space]free fire`
    4. Tunggu pemrosesan *Instant Search* (atau tekan *Enter*).
    5. Coba kasus lain: ketikkan keyword *multi-word* dengan **4 spasi** di tengah kata. Contoh: `free[space][space][space][space]fire`.
    6. Observasi respon sistem terhadap kemunculan produk.
*   **Expected Result**:
    Sistem seyogyanya menerapkan utilitas `String.trim()` bawaan terpusat untuk memangkas *unlimited trailing/leading spaces*, maupun `regex multiple whitespace` (*replace(/\\s+/g, ' ')*) agar berapapun jumlah spasi berlebih dari *user typo* dapat direduksi secara halus *(Fail-safe)* sehingga sistem tetap menampilkan hasil "Free Fire".
*   **Actual Result**:
    Sistem **gagal mendeteksi** keyword.
    - Pada Spasi Awalan (*Leading*), toleransi maksimal hanya 4 kali. Pada ketukan spasi ke-5, halaman search mendadak `BLANK` / *Empty state* produk tidak tertera.
    - Pada Spasi Tengah (*Between words*), toleransi maksimal hanya 3 kali. Pada ketukan spasi ke-4, hasil *list* menjadi lenyap.
    Sebaliknya, pada Spasi Akhiran (*Trailing Spaces*), sistem dapat mentolerir berapapun jumlah spasinya (tidak ada *bug* di ekor kata).
*   **Severity / Impact**: 🟡 **Medium / Minor**
    Kesalahan (*typo* spasi berlebih akibat tombol tersangkut/dll) dari *user behavior* menyebabkan ia berasumsi produk terkait "tidak dijual" di *platform*, karena sistem tidak bisa membaca "free         fire" sama dengan "free fire". Mengurangi angka konversi penjualan (*Lost Sales Potential*).

---

## Lampiran: Ringkasan Temuan Observasi Langsung

| No | Temuan | Kategori | Status | Notes untuk Tim |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Instant Search berfungsi — hasil muncul real-time tanpa reload | ✅ Sesuai Harapan | PASS | — |
| 2 | Keyword Highlighting aktif pada hasil pencarian | ✅ Sesuai Harapan | PASS | — |
| 3 | Kategori hasil terpisah (Rekomendasi & Produk) | ✅ Sesuai Harapan | PASS | — |
| 4 | SQL Injection ditangani aman, empty state friendly | ✅ Sesuai Harapan | PASS | **BE:** Lanjutkan testing dengan payload XSS dan Blind SQLi |
| 5 | Tombol clear (X) mengembalikan state ke awal | ✅ Sesuai Harapan | PASS | — |
| 6 | Case-insensitive search berfungsi | ✅ Sesuai Harapan | PASS | — |
| 7 | Trailing spaces di-trim otomatis — **tanpa batasan jumlah** | ✅ Sesuai Harapan | PASS | Menjadi baseline pembanding inkonsistensi |
| 8 | Leading spaces (1–4) di-trim otomatis | ✅ Sesuai Harapan | PASS | — |
| 9 | Leading spaces (5+) — gagal di-trim, muncul empty state | ⚠️ Potential Defect | READY | ⚠️ Potential Defect — Needs Clarification dari PM/BE. Sistem gagal menampilkan hasil pencarian ketika leading spaces ≥ 5. Perilaku ini **inkonsisten** dengan trailing spaces yang berhasil di-trim tanpa batasan (TC-E-001). Jika design spec tidak mengatur batasan khusus, maka ini merupakan defect pada logika trimming. Referensi: Google Search, Tokopedia, Shopee — semua menghandle unlimited leading spaces. |
| 10 | Mid-word spaces (1–3 tambahan) di-normalisasi otomatis | ✅ Sesuai Harapan | PASS | — |
| 11 | Mid-word spaces (4+ tambahan) — gagal mendeteksi produk | ⚠️ Potential Defect | READY | ⚠️ Potential Defect — Needs Clarification dari PM/BE. Sistem gagal mendeteksi produk ketika mid-word spaces ≥ 4 tambahan. Perilaku ini **inkonsisten** dengan trailing spaces yang berhasil di-trim tanpa batasan (TC-E-001). Jika design spec tidak mengatur batasan khusus, maka ini merupakan defect pada logika normalisasi spasi. Saran perbaikan: terapkan `replace(/\s+/g, ' ')` secara universal sebelum query diproses. |
| 12 | Parameter `?from=search` terpasang pada URL navigasi | ✅ Sesuai Harapan | PASS | **PM/Analytics:** Pertahankan di semua rilis |
| 13 | Tidak ada error 404 saat navigasi dari pencarian | ✅ Sesuai Harapan | PASS | **FE:** Jalankan regression rutin |
| 14 | Initial state tanpa riwayat — area kosong tanpa rekomendasi trending | 💡 Enhancement | SUGGESTION | **PM/UX:** Tambahkan fallback trending |

---

## Lampiran: Rangkuman Inkonsistensi Penanganan Spasi

> [!WARNING]
> **Untuk PM/PO — Perlu Klarifikasi Desain**
>
> Ditemukan inkonsistensi pada penanganan spasi tambahan yang menunjukkan kemungkinan implementasi trimming yang tidak seragam:

| Posisi Spasi | Batas Toleransi | Status | Konsisten? |
| :--- | :--- | :--- | :--- |
| **Trailing** (di akhir) | ♾️ Tanpa batas | ✅ PASS | Baseline ✅ |
| **Leading** (di awal) | Maks 4 spasi tambahan | ⚠️ Boundary: 5 spasi = FAIL | ❌ Tidak konsisten |
| **Mid-word** (di tengah) | Maks 3 spasi tambahan | ⚠️ Boundary: 4 spasi = FAIL | ❌ Tidak konsisten |

**Pertanyaan klarifikasi untuk PO/PM:**
1. Apakah batasan leading spaces (maks 4) dan mid-word spaces (maks 3) merupakan keputusan desain yang disengaja?
2. Jika ya, mohon dokumentasikan di acceptance criteria agar QA bisa menyesuaikan expected result.
3. Jika tidak, ini merupakan defect yang perlu ditangani oleh tim BE — saran perbaikan: terapkan `trim()` + `replace(/\s+/g, ' ')` secara universal sebelum query diproses.

**Referensi industri:** Platform seperti Google Search, Tokopedia, dan Shopee menghandle spasi tambahan tanpa batasan di posisi manapun.

---

> **Catatan untuk Tim:**  
> Dokumen ini dirancang agar bisa dipahami oleh seluruh anggota tim, termasuk developer yang bukan berlatar belakang QA. Setiap test case ditulis dengan bahasa teknis yang lugas — menjelaskan *apa yang dilakukan*, *apa yang diamati*, dan *apa yang seharusnya terjadi* — tanpa jargon pengujian yang berlebihan. Kolom **Notes** di setiap test case ditujukan untuk memberikan konteks dan rekomendasi langsung kepada stakeholder terkait (FE/BE/PM/UX). Jika ada pertanyaan tentang salah satu test case, silakan diskusikan bersama tim QA.
