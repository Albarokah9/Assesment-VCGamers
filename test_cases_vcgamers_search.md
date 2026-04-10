# Laporan Assessment QA Engineer Intern - VCGamers: Search Feature

Dokumen ini berisi hasil pembuatan Test Case komprehensif untuk fitur **Search Produk di VCGamers**, pengujian (observasi) terhadap *actual behavior*, dan pelaporan isu (*Bug/Finding*) sesuai dengan instruksi Assessment serta standar QA *best practices*.

---

## 1. Traceability Matrix (Requirements Mapping)

Matriks berikut memastikan bahwa ke-8 *Requirement* yang disyaratkan dalam *Brief* Assessment telah ter-cover seluruhnya oleh serangkaian Skenario Pengujian (Test Case).

| Req ID | Requirement | Test Case ID | Status Coverage |
| :--- | :--- | :--- | :--- |
| **R-1** | User dapat memasukkan kata kunci pada kolom search. | TC-F-001, TC-E-001 | ✅ Covered |
| **R-2** | User dapat menjalankan search (termasuk via tombol Enter/Klik). | TC-F-001 | ✅ Covered |
| **R-3** | Sistem menampilkan halaman hasil pencarian setelah search dijalankan. | TC-F-001, TC-F-002 | ✅ Covered |
| **R-4** | Keyword yang dicari tetap terlihat setelah hasil pencarian tampil. | TC-F-003 | ✅ Covered |
| **R-5** | Sistem menampilkan hasil yang relevan dengan keyword. | TC-F-001, TC-F-002 | ✅ Covered |
| **R-6** | Jika tidak ada hasil, sistem menampilkan empty state yang jelas. | TC-N-001 | ✅ Covered |
| **R-7** | User dapat membuka detail produk dari hasil pencarian. | TC-F-005 | ✅ Covered |
| **R-8** | Search tidak boleh error/blank/rusak walaupun input tidak biasa. | TC-E-001, TC-E-002, TC-SEC-001 | ✅ Covered |

---

## 2. Test Case Summary Table (Format Assessment PDF)

Berikut rangkuman daftar Test Case sesuai format yang diminta dalam *Task Output* (Tabel minimal: No, Test Case, Expected Result, Status, Notes).

| No | Tipe | Test Case | Expected Result | Status | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | `Functional` | (TC-F-001) Validasi end-to-end pencarian produk valid dan relevansi pencarian | Sistem menampilkan hasil produk yang relevan dengan kata kunci secara *real-time* atau setelah menekan *Enter*. | **Pass** | Algoritma *instant search* berfungsi cepat tanpa *delay* berlebih. |
| **2** | `Functional` | (TC-F-002) Validasi pencarian tanpa terpengaruh huruf kapital (*Case Insensitive*) | Sistem mengabaikan kapitalisasi huruf dan tetap memunculkan hasil relevan. | **Pass** | - |
| **3** | `Functional` | (TC-F-003) Pengecekan *keyword* persisten di dalam kolom *search bar* pasca-pencarian | Kata kunci yang telah diketik tidak menghilang dan tetap terlihat di dalam kolom input *search bar* setelah hasil muncul. | **Pass** | Memastikan pemenuhan Requirement **R-4**. |
| **4** | `Negative` | (TC-N-001) Validasi UI/UX *Empty State* via Negative Search (Data Kosong) | Menampilkan pesan *"Tidak ada yang cocok dengan Keyword ini"*, daftar Rekomendasi Kategori (Cari di Kategori ini), dan tombol Kembali & Kirim Saran. | **Pass** | Tampilan *empty state* sangat komprehensif merujuk ke Requirement **R-6**. |
| **5** | `Functional` | (TC-F-005) Validasi navigasi dan rute URL menuju halaman Detail Produk dari hasil pencarian | Pengguna diarahkan ke URL rincian spesifik target tanpa mendapatkan *HTTP 404 (Not Found)* saat di-klik. | **Pass** | Rute valid dan param referensi (`?from=search`) sukses disematkan. |
| **6** | `Edge Case` | (TC-E-001) Manipulasi Spasi Berlebih di Awalan Kata (*Leading Spaces*) | Sistem secara dinamis men-nol-kan (*Trim*) spasi berlebih pada awal kata dan otomatis memfokuskan pencarian ke huruf. | **Fail (Potential Defect)** | *Bug Logic*: Sistem hanya bisa mentolerir maksimal 4 spasi awalan. Spasi ke-5 gagal diproses. |
| **7** | `Edge Case` | (TC-E-002) Manipulasi Spasi Berlebih di Pertengahan Kata (*Between Words*) | Sistem menormalisasi (Trim menjadi 1 spasi tunggal) jarak antar kata yang diinput. | **Fail (Potential Defect)** | *Bug Logic*: Sistem hanya mentolerir maksimal 3 spasi antar kata. Spasi ke-4 merusak hasil pencarian. |
| **8** | `Security` | (TC-SEC-001) *Vulnerability check*: Injeksi Payload dasar *SQL (SQLi)* | Sistem melucuti karakter meta SQL dan tidak mengeluarkan respon *error exception* dari layer Database, dialihkan ke *Empty State*. | **Pass** | Basis keamanan filter pada form input search stabil. |

---

## 3. Bug / Finding Report

Sesuai permintaan dokumen Assessment, berikut format pelaporan isu/celah bug *Logic* dan Inkonsistensi UX yang dijumpai selama observasi *Edge Case*.

### Bug 1: Inkonsistensi Toleransi "Trim Spaces" yang Mengakibatkan Kegagalan Pencarian Produk

*   **Title**: Algoritma Pencarian Gagal Membaca Keyword Jika Terdapat 4+ Spasi Awalan (*Leading*) atau 3+ Spasi Tengah (*Between*)
*   **Steps to Reproduce**:
    1. Buka halaman utama website `https://www.vcgamers.com/`.
    2. Klik kolom Search pencarian.
    3. Ketikkan keyword awalan dengan **5 spasi**. Contoh: `[space][space][space][space][space]free fire`
    4. Tunggu pemrosesan *Instant Search* (atau tekan *Enter*).
    5. Coba kasus lain: ketikkan keyword *multi-word* dengan **4 spasi** di tengah kata. Contoh: `free[space][space][space][space]fire`.
    6. Observasi respon sistem terhadap kemunculan produk.
*   **Expected Result**:
    Sistem seyogyanya menerapkan utilitas `String.trim()` bawaan terpusat untuk memangkas *unlimited trailing/leading spaces*, maupun `regex multiple whitespace` (*replace(/\s+/g, ' ')*) agar berapapun jumlah spasi berlebih dari *user typo* dapat direduksi secara halus *(Fail-safe)* sehingga sistem tetap menampilkan hasil "Free Fire".
*   **Actual Result**:
    Sistem **gagal mendeteksi** keyword.
    - Pada Spasi Awalan (*Leading*), toleransi maksimal hanya 4 kali. Pada ketukan spasi ke-5, halaman search mendadak `BLANK` / *Empty state* produk tidak tertera.
    - Pada Spasi Tengah (*Between words*), toleransi maksimal hanya 3 kali. Pada ketukan spasi ke-4, hasil *list* menjadi lenyap.
    Sebaliknya, pada Spasi Akhiran (*Trailing Spaces*), sistem dapat mentolerir berapapun jumlah spasinya (tidak ada *bug* di ekor kata).
*   **Severity / Impact**: 🟡 **Medium / Minor**
    Kesalahan (*typo* spasi berlebih akibat tombol tersangkut/dll) dari *user behavior* menyebabkan ia berasumsi produk terkait "tidak dijual" di *platform*, karena sistem tidak bisa membaca "free         fire" sama dengan "free fire". Mengurangi angka konversi penjualan (*Lost Sales Potential*).

---

## 4. Detailed Test Cases (Gaya Internal Standard / ClickUp Ready)

Berikut penjabaran tiap perancangan test case yang mematuhi standar *Double-Spaced Guidelines* guna implementasi test otomatisasi atau manual via TMS.

### TC-F-001: Validasi End-to-End Pencarian Produk Valid dan Relevansi Pencarian

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | R-1, R-2, R-3, R-5 |
| **Priority** | URGENT |
| **Test Type** | Functional |
| **Environment** | DEVELOPMENT |

#### Preconditions
Aplikasi berhasil memuat halaman Web utama tanpa intervensi pop-up blocking pada UI.
Test Data: Valid Keywords:
* Kata utuh (contoh: "Free Fire")
* Kata singkatan umum populer (contoh: "MLBB")
* Karakter Alpha-Numeric (contoh: "PUBG Mobile")

#### Test Steps
Kunjungi dan muat halaman utama VCGamers, tunggu seluruh *assets DOM* selesai.

Pusatkan kursor (klik) pada kolom input pencarian (*Search Target Box*) di wilayah *Header*.

Ketikkan spesifik subjek `Test Data` yang valid ke dalam kotak tersebut.

Verifikasi bahwa fitur *Instant Search Dropdown* menampakkan pratilik list relasi data *real-time*.

Tekan tombol kunci `Enter` (Return key) pada papan ketik.

Verifikasi bahwa secara fungsional antarmuka ditransisikan seutuhnya kepada halaman dedikasi Hasil (*Search Result Page*).

Lakukan penyisiran vertikal *listing component*.

Verifikasi *Title Component* dan korelasi meta-data item produk sinkron dengan subjek kata pencarian di langkah ketiga.

#### Expected Result
> Pemasukan teks pada alat pencari berhasil diterjemahkan dan diproses, baik via pratinjau *dropdown real-time* (Instant Search) maupun pasca-eksekusi tombol `Enter`. Halaman baru memberikan antarmuka hasil yang relevan serta terkurasi mutlak terhadap parameter *Search Query* yang disuplai pengguna.

#### Actual Result
> Algoritma fungsional terintegrasi mulus. *Keyword search* memicu *API Call* yang mengembalikan kumpulan produk (*list item*) relevan, dan halaman beralih (*redirect*) mulus berisikan komponen daftar hasil yang tepat.

<br>

---

### TC-F-002: Validasi Pencarian Tanpa Terpengaruh Huruf Kapital (Case Insensitive)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | R-5 |
| **Priority** | HIGH |
| **Test Type** | Functional |
| **Environment** | DEVELOPMENT |

#### Preconditions
Pengguna barada di antarmuka depan (*Homepage*).
Test Data: Manipulasi Case Styles:
* HURUF KAPITAL SEMUA (contoh: "MOBILE LEGENDS")
* huruf kecil semua (contoh: "mobile legends")
* Huruf AlTeRnAtif (contoh: "MoBiLe LeGeNdS")

#### Test Steps
Seleksi *search box* pada navigasi area peluncur halaman.

Suplai *input* kotak telusur menggunakan rentang variabel uji kapitalisasi.

Tekan kunci aksi `Enter`.

Inspeksi urutan relevansi judul *item* utama yang berada di susunan 5 tingkat teratas.

Lakukan validasi silang perbandingan *output* di antara gaya kapital yang berbeda pada `Test Data`.

#### Expected Result
> Sistem algoritma *query* harus mumpuni meredam nilai komparasi biner pada nilai huruf menjadi murni pemahaman kamus kata dasar (*absolute case-insensitive*). Ketiga gaya penulisan wajib mencetak deretan *index array output* data yang berbobot persis satu sama lain.

#### Actual Result
> Perbedaan format gaya kapital yang disemati oleh eksekutor *user* tidak berdampak pada perbedaan filterisasi respon *items*. Semua versi kapitalisasi meluncurkan hasil pemetaan produk yang 100% kongruen.

<br>

---

### TC-F-003: Pengecekan Keyword Persisten di Dalam Kolom Search Bar Pasca-Pencarian

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | R-4 |
| **Priority** | MEDIUM |
| **Test Type** | Functional |
| **Environment** | DEVELOPMENT |

#### Preconditions
Pengguna berada di layar Home siap untuk melaksanakan kueri pencarian.
Test Data: Keyword:
* "Genshin Impact"

#### Test Steps
Fokuskan tetikus / kursor pada elemen pencari atas.

Ketikan entri data `"Genshin Impact"`.

Picu *trigger default* `Enter` keyboard guna menyerahkan permintaan telusur.

Tunggu antarmuka sistem mem-pulih-kan proses struktur `HTML Document` seluruhnya (Page Loaded).

Lakukan pemantauan status aktual *value* form pencarian yang terletak konstan di batang Header.

Verifikasi kesamaan parameter teks internal dengan teks *input* yang didorong pada langkah ke-2.

#### Expected Result
> Elemen kotak kueri telusur menjaga serta memproyeksikan tulisan target yang telah dituang prapemusatan aksi; tidak merombaknya menjadi status *placeholder* kosong maupun memicu intervensi teks di luar dugaan, memfasilitasi kebutuhan perubahan minor *search user*.

#### Actual Result
> Setelah pemuatan halaman hasil (*Search Result Page*), formulir kata kunci VCGamers memegang wujud kata kunci awal secara kokoh sesuai prinsip visibilitas UX, membuat *keyword* yang dicari tetap terekspos.

<br>

---

### TC-N-001: Validasi UI/UX Empty State via Negative Search (Pencarian Data Kosong)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | R-6 |
| **Priority** | HIGH |
| **Test Type** | Negative |
| **Environment** | DEVELOPMENT |

#### Preconditions
Berada pada halaman fungsional telusur.
Test Data: Invalid Normal Keyword:
* "testing tidak ada"

#### Test Steps
Ketikkan parameter spesifik `"testing tidak ada"` dalam wadah *search box*.

Inisiasi telusur melalui injeksi `Enter`.

Observasi layar balasan pada kanvas pemetaan produk.

Verifikasi ketersediaan elemen eksistensi teks pusat `Tidak ada yang cocok dengan Keyword ini`.

Inspeksi keberadaan ornamen Tombol Interaktif `Kembali ke Home` dan `Kirim Saran`.

Lakukan penyisiran daftar Rekomendasi Terpadu bawah yang memuat klasifikasi alternatif `Cari di Kategori ini` (seperti *DLC, Top Up via Link, Item dan Skin, dll*).

#### Expected Result
> Ketika *Inventory Engine* mengembalikan data `NULL` untuk baris *Query* tersebut, sistem mentransmisikannya menjadi *User Experience Empty State* yang padat dan bersahabat. Fitur memunculkan petunjuk eksplisit, jalan keluar asisten (*Back to Home & Saran*), ditambah peta indeks perbantuan navigasi kategori demi mereduksi titik putus navigasi (*Dead End*).

#### Actual Result
> Representasi *Empty State* dieksekusi secara impresif. Antarmuka me-render *Message Container* *"Tidak ada yang cocok dengan Keyword ini"* diapit oleh opsi pintasan solutif, serta segenap kumpulan direktori navigasi *Chips Button* (Kategori) di sektor bawah secara proporsional.

<br>

---

### TC-F-005: Validasi Navigasi dan Rute URL Menuju Halaman Detail Produk dari Hasil Pencarian

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | R-7 |
| **Priority** | URGENT |
| **Test Type** | Functional |
| **Environment** | DEVELOPMENT |

#### Preconditions
Skema sedang menampilkan layar Hasil Pencarian (*Search Result*) karena tahap penelusuran kata telah dieksekusi tuntas (*contoh: "Free Fire"*).

#### Test Steps
Arahkan visibilitas layang kepada daftar komponen produk / *List View Card* tersuguh.

Klik elemen struktur satu unit komponen *(Card / Product Image / Product Title)* secara definitif.

Amati rotasi *Route transition* bilah *URL Web Browser* selama pemrosesan pindah ruang.

Tinjau render muatan halaman pendaratan utama (*Product Detail View*).

Verifikasi status tanggapan HTTP kode agar terbebas dari *Bugs* kelumpuhan alamat `404 Not Found`.

Verifikasi penataan elemen analitik pendeteksi sumber rujukan, yaitu atribusi param URL spesial tambahan `?from=search`.

#### Expected Result
> Pengeklikan produk memicu operasi transfer rute spesifik menuju kanvas detail *checkout* mutakhir benda yang diklik tanpa putus di jalan. Penyematan jejak kueri metrik (`?from=search`) terekat sempurna.

#### Actual Result
> Navigasi komponen list sukses tanpa halangan. Pemakai berpindah pada URL persis seperti `https://www.vcgamers.com/gercep/free-fire/top-up-game?from=search` membuktikan URL berhasil terbentuk dinamis tanpa komplikasi *HTTP 404* serta merangkul nilai *Data Analytics Param*.

<br>

---

### TC-E-001: Manipulasi Spasi Berlebih di Awalan Kata (Leading Spaces)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | R-8 |
| **Priority** | MEDIUM |
| **Test Type** | Edge Case |
| **Environment** | DEVELOPMENT |

#### Preconditions
Berada pada status awal *Home*.
Test Data:
* 4 spasi didepan: `"    free fire"`
* 5 spasi didepan: `"     free fire"`
* 20 spasi didepan: `"                    free fire"`

#### Test Steps
Ketuk input box penelusur demi mengaktifasi respon *caret text*.

Minta integrasi kueri pertama (*Test Data 4 Spasi*).

Amati pengeluaran data hasil *search* apakah *list* tampil sempurna.

Hapus isi penelusuran.

Masukkan eksekusi kueri rentang kedua (*Test Data 5+ Spasi*).

Pantau konsistensi ketahanan filter parameter mesin telusur melalui respon *List item* akhir.

#### Expected Result
> Operasi fungsional utilitas prapemrosesan harus selalu mereduksi *(Trim)* seluruh kekosongan aksara (*void whitespaces*) mutlak pada gerbang *string* yang berjejer awal berapa pun kuantitasnya, membalikan nilai suci target murni semata untuk dicerna algoritma dalam mencari *ID Relativitas Domain*. Hasil wajarnya selalu memunculkan entitas target "Free Fire" dalam situasi Spasi tak berhingga tersebut.

#### Actual Result
> Muncul diskriminasi kapasitas fungsional penyaringan *white-space* awalan. Form dapat melakukan *auto-trim* dan membawakan *list item* jika ruang spasi berada di ambang maksmum **4 Spasi**. Membubuhi **5++ spasi** meledakkan efisiensi sehingga hasil *rendered Items* lenyap / menjadi *Empty View*.

<br>

---

### TC-E-002: Manipulasi Spasi Berlebih di Pertengahan Kata (Between Words)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | R-8 |
| **Priority** | MEDIUM |
| **Test Type** | Edge Case |
| **Environment** | DEVELOPMENT |

#### Preconditions
Posisi ada pada UI Beranda.
Test Data:
* 3 spasi di tengah: `"free   fire"`
* 4 spasi di tengah: `"free    fire"`
* 20 spasi di tengah: `"free                    fire"`

#### Test Steps
Fokus di kolom penelusur.

Sematkan rincian parameter penguji kesatuan menengah (*Multi-words Limit Testing*) sejumlah 3 jarak.

Perhatikan dan verifikasi produk penemuan yang melayang di dropdown/kanvas list.

Hapus kolom ketikan *SearchBar*.

Masukkan data pengukur tegangan spasi ekstrim lanjutan (*Test Data > 4 spasi*).

Observasi status pendaratan elemen output.

#### Expected Result
> *Filter Engine* dituntut untuk membuang beban jarak multipel yang berada diantara konjungsi blok frasa membalikkannya secara *default* kedalam formasi 'Satu Batas Spasi', menggunakan regex pengganti universal, agar maksud fundamental ketikan user tidak terkontaminasi anomali ketidaksengajaan ruang yang lebar.

#### Actual Result
> Mekanika algoritma hanya melepaskan hasil murni jika pembatas tengah frasa berjumlah setinggi-tingginya **3 Spasi**. Mengungkit formasi jarak melebihi batas tersebut (*4+ Spaces*) melahirkan anomali cacat pengenalan *Search Algorithm*, membirakan kanvas layar nihil dari eksistensi produk.

<br>

---

### TC-SEC-001: Vulnerability Check - Injeksi Payload dasar SQL (SQLi)

| 🏷️ Field | ℹ️ Detail |
| :--- | :--- |
| **References** | R-8 |
| **Priority** | HIGH |
| **Test Type** | Security |
| **Environment** | DEVELOPMENT |

#### Preconditions
Form Penelusur dalam mode siaga input.
Test Data: Injection Payload:
* `' OR 1=1;--`
* `" OR "1"="1`
* `1' OR '1'='1`

#### Test Steps
Masukkan instruktur manipulasi karakter meta injeksi kedalam struktur *search box* berdasar kumpulan *Test Data*.

Tekan Enter per injeksi agar memaksa beban param *Query String* menyebrangi sistem *Routing* Backend.

Pantau letupan parameter layang UI atas segala potensi letupan pesan kelemahan *Database Syntax error* (`MySQL/Postgres Exception`).

Observasi keadaan *Landing View* apa ditujukan ke UI Normal, *Empty State*, atau halaman Rusak (`Blank/500 Internal Server error`).

#### Expected Result
> Penjagaan sanitasi dan arsitektur ORM / *Parameterized Query* VCGamers bereaksi keras meredam seluruh formasi aneh penguji perusakan, tanpa meluapkan respon telanjang kesalahan sistem operasi peladen. Situs melempar fasa fungsional kembali kepada UI ramah *Empty State* sembari menjaga stabilitas DOM (Terhadap Requirement R-8: Tidak error/blank/rusak).

#### Actual Result
> Filter dan parameter lolos disanitasi secara luar biasa. Situs memproteksi interaksi database dari mutasi injeksi secara sempurna dan memandu pengunjung jatuh ke mode laman visual ramah *Empty State* ("Tidak ada yang cocok..."). Antarmuka HTML stabil seratus persen penuh tanpa keretakan.

<br>

---
*Generated using Advanced SQA Methodologies for Assessment Purposes.*
