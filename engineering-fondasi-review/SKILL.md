---
name: engineering-fondasi-review
description: "Panduan review & generate kode backend/frontend (Bun/TypeScript, Go) berdasarkan prinsip Clean Code (Robert C. Martin), Clean Architecture (Robert C. Martin), Refactoring (Martin Fowler), dan The Pragmatic Programmer (Andy Hunt & Dave Thomas), dengan KISS (Keep It Simple, Stupid) sebagai prinsip payung yang menentukan kapan tiap prinsip lain worth diterapkan. WAJIB dipakai setiap kali menulis kode baru, review/refactor kode existing, mendesain fungsi/struktur data/API, atau mendiskusikan trade-off arsitektur — baik user eksplisit minta 'review pakai best practice' maupun saat generate kode baru dari nol. Skill ini juga dipakai saat user minta pendapat soal 'apakah ini overengineering' atau 'apakah perlu dipecah/di-abstract'."
---

# Engineering Fondasi Review

Skill ini adalah checklist & mental model gabungan dari 4 buku fondasi software engineering, dipakai untuk **generate kode baru** maupun **review kode existing**. Prinsip utamanya: **KISS adalah payung yang menentukan kapan semua prinsip di bawah ini worth diterapkan — bukan aturan yang harus selalu dipaksa jalan.**

## Prinsip Payung: KISS (Keep It Simple, Stupid)

Sebelum menerapkan prinsip apapun di bawah (DRY, polymorphism, layering, decoupling, dst), selalu tanya dulu:

> **"Apakah kompleksitas tambahan ini worth manfaatnya SEKARANG, atau cuma persiapan untuk kemungkinan yang belum tentu terjadi?"**

- Kalau requirement/skala **sudah nyata butuh** → terapkan prinsip terkait.
- Kalau cuma **spekulatif "siapa tau nanti butuh"** → JANGAN diterapkan, biarkan simple. Ini prinsip *YAGNI (You Aren't Gonna Need It)* dari Pragmatic Programmer, senafas dengan KISS.

Setiap section di bawah akan selalu punya bagian **"Kapan JANGAN diterapkan"** — bagian ini sama pentingnya dengan bagian "kapan diterapkan". Saat review kode, tandai juga kalau ada abstraksi/pattern yang dipasang **tanpa alasan konkret** — itu pelanggaran KISS, bukan cuma "boleh-boleh saja".

---

## 1. Clean Code — Level Fungsi & Kelas

### 1.1 Naming & Function Design
- Nama fungsi harus **jujur soal side-effect**. Fungsi yang I/O (DB write, network call, mutasi state) harus keliatan dari nama & return type — jangan disamakan dengan fungsi pure.
- Pecah fungsi karena **beda level abstraksi** (business rule + crypto + I/O + shaping response campur jadi 1), BUKAN karena "kepanjangan". Fungsi pendek yang berlebihan justru menambah indirection cost.
- **Cek KISS**: jangan pecah fungsi kalau 1 level abstraksi tapi cuma "kelihatan panjang" — itu nambah jump-to-definition tanpa manfaat readability nyata.

### 1.2 Code Smell Taxonomy
- **Duplicated Code**: gabung HANYA kalau lolos tes *"kalau requirement berubah, apakah SEMUA pemanggil WAJIB ikut berubah bareng?"*. Kalau tidak (cuma kebetulan sama nilainya sekarang) → biarkan duplikat. Duplikasi teks ≠ duplikasi pengetahuan.
- **Primitive Obsession**: primitif (`string`/`number`/`int`) tanpa tipe domain = bug (1) format/unit ambiguity lolos compiler, (2) positional swap antar parameter bertipe sama. Fix: named type (Go) / branded type (TS) — **tapi hanya di titik invariant kritis** (financial, ID sensitif), bukan semua primitif.
- **Feature Envy**: kalau method di class A lebih banyak akses field class B daripada field A sendiri → pindahkan **keputusan/kalkulasinya**, bukan sekadar bikin getter. Getter tanpa mindahin logic branching itu cosmetic, bukan fix.
- **Shotgun Surgery**: 1 perubahan kecil → banyak file tersebar tanpa kontrak eksplisit → sentralisasi lewat interface/map. **Awas kitchen sink**: kalau >30% field interface bakal optional & cuma dipakai 1-2 implementasi, itu tanda dipaksakan — jangan sentralisasi variannya, biarkan terpisah.

---

## 2. Refactoring (Martin Fowler)

### 2.1 Prinsip Dasar
Refactoring = ubah struktur internal **tanpa ubah behavior eksternal**, langkah kecil, tetap "green" (test passing) tiap step — bukan rewrite sekaligus.

### 2.2 Teknik Inti
| Teknik | Kapan Dipakai | Kapan JANGAN |
|---|---|---|
| Extract Function | Ada perubahan level abstraksi dalam 1 fungsi | Fungsi udah 1 level abstraksi, cuma "kelihatan panjang" |
| Replace Conditional with Polymorphism | Varian sering nambah, atau kompleksitas per-varian tinggi | Varian jarang berubah & logic simple — `switch` biasa lebih jelas |
| Introduce Parameter Object | 5+ parameter yang selalu jalan bareng | Parameter sedikit (≤3), gak ada resiko ketuker |
| Guard Clause | Nested if-else 3+ level | Cuma 1 kondisi simple, guard clause malah nambah baris tanpa manfaat |

### 2.3 Characterization Testing
- Untuk kode **legacy tanpa test**: catat behavior **apa adanya** (observed), bukan yang "seharusnya", sebagai safety net sebelum refactor.
- Kalau ada behavior aneh/bug saat characterization → tetap catat sebagai baseline dulu. Fix bug adalah proses TERPISAH setelah refactor aman.
- Untuk kode baru: langsung test berdasarkan requirement, bukan characterization test.

---

## 3. Clean Architecture (Robert C. Martin)

### 3.1 Dependency Rule
- Arah dependency dari luar ke dalam: **business logic tidak boleh langsung memanggil detail teknis** (DB client, HTTP framework, crypto library konkret).
- Domain logic terima interface/kontrak (`UserRepository`, `PasswordHasher`), implementasi konkret (Postgres, bcrypt) di layer adapter terpisah.
- Analogi cek cepat: *"apakah fungsi ini, kalau dibaca, nyebut nama teknologi spesifik (Postgres, Redis, Express) di tengah logic bisnisnya?"* — kalau ya, itu pelanggaran.

### 3.2 Layer Boundary Praktis — 3 Sinyal Valid Bikin Interface/Boundary
Bikin interface **HANYA** kalau minimal 1 dari 3 sinyal ini nyata:
1. **Beneran mungkin di-swap** implementasinya (bukan spekulatif)
2. **Logic kompleks**, butuh unit test cepat tanpa nyalain infra (DB/network)
3. **Sering nambah varian baru** (nyambung ke Replace Conditional with Polymorphism)

**Kapan JANGAN**: CRUD sederhana tanpa branching logic → langsung query di handler, jangan dipaksa full layered architecture (repository interface + use case + DTO mapper). Baru bikin interface kalau ada **implementasi kedua yang REAL** (Rule of Three), bukan "siapa tau nanti butuh".

> Catatan penting: struktur 4-layer versi buku (entities/use cases/interface adapters/frameworks) cocok untuk sistem enterprise tim besar. Untuk project solo/kecil, ambil **prinsipnya** (dependency arah ke dalam) di titik yang bersinyal — jangan cargo-cult struktur foldernya.

### 3.3 Testability sebagai Konsekuensi, Bukan Tujuan
- Kalau test butuh **banyak mock** untuk 1 fungsi → itu sinyal desain salah (I/O nyampur business logic), bukan alasan menambah tooling mocking yang lebih canggih.
- Fix: pisahkan pure function (kalkulasi, business rule — testable tanpa mock) dari I/O layer (DB/HTTP/cache — cukup integration test / smoke test).
- **Kapan JANGAN dipisah**: logic beneran simple (1-2 baris, tanpa branching) — overhead pemisahan lebih mahal dari manfaatnya.

---

## 4. The Pragmatic Programmer (Andy Hunt & Dave Thomas)

### 4.1 DRY yang Bener vs Salah Kaprah
- DRY sejatinya soal **PENGETAHUAN**, bukan soal **TEKS**. "Every piece of knowledge must have a single, authoritative representation."
- Tes cepat sebelum menggabungkan kode mirip: *"kalau requirement berubah, apakah semua pemanggil WAJIB ikut berubah bareng?"* → ya = 1 knowledge, gabung. Tidak = kebetulan sama, biarkan terpisah.
- Awas DRY tersembunyi: knowledge yang sama muncul dalam bentuk beda (angka di variable vs angka hardcoded di string/dokumentasi) — ini juga pelanggaran DRY yang sering kelewat saat review.

### 4.2 Tracer Bullet vs Prototype — Jangan Tertukar
| | Tracer Bullet | Prototype |
|---|---|---|
| Tujuan | Validasi "apakah arsitektur/integrasi nyambung end-to-end" | Validasi "apakah approach/algoritma cukup akurat/efektif" |
| Nasib kode | **Tetap dipakai**, diperkuat bertahap | **Selalu dibuang** — cocok maupun tidak cocok |

**Bahaya paling umum**: prototype yang "berhasil" dipoles langsung jadi production tanpa ditulis ulang. Validasi prototype cuma soal ide/approach, BUKAN soal kualitas kode (prototype lazim skip error handling, security, edge case).

### 4.3 Orthogonality & Decoupling
- Tes cepat: *"kalau saya ganti/hapus concern X, apakah saya harus edit fungsi yang tidak berhubungan dengan X?"* → kalau ya, tidak orthogonal.
- Sinyal pelanggaran: 1 fungsi punya banyak tanggung jawab campur (contoh: `createOrder` juga urus notifikasi + logging + loyalty point) — kegagalan di 1 concern bisa nge-block concern lain yang tidak berhubungan.
- **Kapan JANGAN pakai event-driven/decoupling berat**: concern sedikit & jarang berubah, project kecil — cukup pisah fungsi + panggil eksplisit, jangan bangun event bus (nambah indirection & kompleksitas eventual consistency yang tidak sepadan).

---

## Cara Pakai Skill Ini

### Saat generate kode baru
1. Tulis solusi paling simple yang memenuhi requirement (KISS/YAGNI dulu).
2. Cek section 1-4 di atas: apakah ada sinyal konkret (bukan spekulatif) yang butuh pattern tambahan (interface, parameter object, dsb)?
3. Kalau ada logic bisnis dengan invariant kritis (uniqueness, concurrency, currency/unit) → cek eksplisit: apakah ini butuh DB constraint, bukan cuma application-level check (hindari TOCTOU)?

### Saat review kode existing
1. Jalankan checklist tiap section sebagai pertanyaan, bukan vonis otomatis — selalu sertakan **alasan kenapa** suatu bagian melanggar/tidak melanggar prinsip.
2. Untuk tiap temuan, sebutkan **trade-off fix-nya** (bukan cuma "ini salah, ganti ke X") — termasuk kapan fix itu sendiri berisiko jadi overengineering.
3. Prioritaskan bug fundamental (race condition, data integrity, security) di atas gaya penulisan/readability.

### Nada output
- Kritik jujur, langsung ke inti masalah — bukan basa-basi.
- Selalu jelaskan **root cause** (level compiler/runtime/DB internals kalau relevan), bukan cuma "begini caranya".
- Beri contoh failure mode konkret (bukan cuma happy path) untuk tiap temuan.
