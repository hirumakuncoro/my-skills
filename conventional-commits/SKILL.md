---
name: conventional-commits
description: Paraphrased and translated Conventional Commits v1.0.0 specification for commit message standards.
license: CC-BY-3.0
metadata:
  author: Ikhsan Heriyawan
  version: "1.0.0"
---

# Conventional Commits v1.0.0 (Paraphrase + Terjemahan)

Catatan: Dokumen ini berisi parafrase berbahasa Inggris dari spesifikasi resmi, diikuti terjemahan Bahasa Indonesia, agar dapat dipakai sebagai referensi skill agent. Untuk teks asli silakan rujuk spesifikasi resmi.

## Summary (EN, paraphrase)
Conventional Commits is a lightweight convention layered on top of commit messages. It defines a simple structure so commit history is explicit and can be processed by tools. It aligns with Semantic Versioning by indicating features, fixes, and breaking changes in commit messages.

**Commit structure:**
```
<type>[optional scope]: <description>

[optional body]
[optional footer(s)]
```

**Core elements and intent:**
1. `fix`: patch-level bug fix (SemVer PATCH).
2. `feat`: new feature (SemVer MINOR).
3. `BREAKING CHANGE`: breaking API change, indicated either by a footer or by `!` in the header (SemVer MAJOR). Can appear with any type.
4. Other types are allowed (e.g., `build`, `chore`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`).
5. Footers other than `BREAKING CHANGE` may exist and follow a trailer-like format.
6. Scope is optional and clarifies the area, e.g., `feat(parser): ...`.

## Ringkasan (ID, terjemahan)
Conventional Commits adalah konvensi ringan di atas pesan commit. Konvensi ini menentukan struktur sederhana agar riwayat commit eksplisit dan mudah diproses alat otomatis. Ia selaras dengan Semantic Versioning karena fitur, perbaikan, dan perubahan yang memutus kompatibilitas bisa dikenali langsung dari pesan commit.

**Struktur pesan commit:**
```
<type>[optional scope]: <description>

[optional body]
[optional footer(s)]
```

**Elemen inti dan maksudnya:**
1. `fix`: perbaikan bug tingkat patch (SemVer PATCH).
2. `feat`: fitur baru (SemVer MINOR).
3. `BREAKING CHANGE`: perubahan API yang merusak kompatibilitas, ditandai melalui footer atau `!` di header (SemVer MAJOR). Bisa dipakai dengan tipe apa pun.
4. Tipe lain diperbolehkan (misalnya `build`, `chore`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`).
5. Footer selain `BREAKING CHANGE` boleh ada dan mengikuti format trailer.
6. Scope opsional untuk konteks area, misalnya `feat(parser): ...`.

## Examples (EN, paraphrase)
- A feature commit with a breaking change explained in the footer.
- A feature commit marked as breaking by `!` in the header.
- A scoped feature commit marked as breaking by `!`.
- A `chore` commit marked as breaking with a breaking-change footer.
- A `docs` commit with only a description line.
- A scoped feature commit for adding a language.
- A fix commit with a multi-paragraph body and multiple footers (e.g., review and issue refs).

## Contoh (ID, terjemahan)
- Commit `feat` dengan penjelasan breaking change di footer.
- Commit `feat` yang diberi tanda `!` sebagai breaking change.
- Commit `feat` dengan scope dan `!` untuk menandai breaking change.
- Commit `chore` yang breaking, plus footer breaking change.
- Commit `docs` tanpa body (hanya deskripsi).
- Commit `feat` dengan scope untuk penambahan bahasa.
- Commit `fix` dengan body multi-paragraf dan beberapa footer (mis. review dan referensi isu).

## Specification (EN, paraphrase)
Keywords like MUST, SHOULD, MAY follow RFC 2119 semantics. The rules:
1. Commits MUST start with a type (noun like `feat`, `fix`, etc.), optional scope, optional `!`, and a required `: `.
2. Use `feat` for new features.
3. Use `fix` for bug fixes.
4. Scope MAY be provided and must be a noun inside parentheses, e.g., `fix(parser):`.
5. A short description MUST follow after `: `.
6. A body MAY follow after a blank line, adding context.
7. Body is free-form, multiple paragraphs allowed.
8. One or more footers MAY appear after another blank line. Each footer uses a token plus `: ` or ` #` separator, then a value (trailer-like).
9. Footer tokens MUST use hyphens instead of spaces (except `BREAKING CHANGE` token).
10. Footer values may include spaces and newlines; parsing stops at the next valid footer.
11. Breaking changes MUST be indicated either by `!` in header or by a footer.
12. If in footer, format is `BREAKING CHANGE: <description>`.
13. If `!` is used in header, the footer `BREAKING CHANGE` MAY be omitted; the description should explain the break.
14. Types beyond `feat` and `fix` MAY be used.
15. Commit info is case-insensitive except `BREAKING CHANGE` which MUST be uppercase.
16. `BREAKING-CHANGE` is synonymous with `BREAKING CHANGE` as a footer token.

## Spesifikasi (ID, terjemahan)
Kata kunci seperti MUST, SHOULD, MAY mengikuti definisi RFC 2119. Aturannya:
1. Commit HARUS diawali tipe (kata benda seperti `feat`, `fix`, dll.), scope opsional, `!` opsional, serta diakhiri `: `.
2. `feat` HARUS dipakai untuk fitur baru.
3. `fix` HARUS dipakai untuk perbaikan bug.
4. Scope BOLEH ada dan harus berupa kata benda di dalam tanda kurung, mis. `fix(parser):`.
5. Deskripsi singkat HARUS mengikuti setelah `: `.
6. Body BOLEH ada setelah satu baris kosong untuk konteks tambahan.
7. Body bebas format dan boleh multi-paragraf.
8. Satu atau lebih footer BOLEH ada setelah satu baris kosong lagi. Tiap footer memakai token plus `: ` atau ` #`, lalu nilai (mirip trailer).
9. Token footer HARUS memakai tanda hubung sebagai pengganti spasi (kecuali token `BREAKING CHANGE`).
10. Nilai footer boleh berisi spasi dan baris baru; parsing berhenti saat menemukan footer valid berikutnya.
11. Breaking change HARUS ditandai lewat `!` di header atau lewat footer.
12. Jika di footer, formatnya `BREAKING CHANGE: <deskripsi>`.
13. Jika memakai `!` di header, footer `BREAKING CHANGE` BOLEH dihilangkan; deskripsi harus menjelaskan perubahan yang memutus kompatibilitas.
14. Tipe selain `feat` dan `fix` BOLEH dipakai.
15. Informasi commit tidak peka huruf besar/kecil, kecuali `BREAKING CHANGE` yang HARUS huruf besar.
16. `BREAKING-CHANGE` dianggap sama dengan `BREAKING CHANGE` untuk token footer.

## Why Use Conventional Commits (EN, paraphrase)
- Enables automatic changelog generation.
- Automates semantic version bumping.
- Communicates change intent to teammates and stakeholders.
- Can trigger build and release workflows.
- Eases contributions by keeping history structured.

## Mengapa Memakai Conventional Commits (ID, terjemahan)
- Memungkinkan pembuatan changelog otomatis.
- Memudahkan penentuan kenaikan versi semantik.
- Menjelaskan maksud perubahan ke tim dan pemangku kepentingan.
- Bisa memicu proses build dan rilis.
- Membantu kontribusi karena riwayat lebih terstruktur.

## FAQ (EN, paraphrase)
- **Early development:** act as if the product is already used; commit clarity still matters.
- **Case of types:** any casing is allowed, but consistency is recommended.
- **Multiple types fit:** prefer splitting into multiple commits.
- **Fast iteration concern:** discourages disorganized speed; helps long-term velocity.
- **Fear of limited types:** types can evolve; focus on clarity.
- **Relation to SemVer:** `fix` -> PATCH, `feat` -> MINOR, `BREAKING CHANGE` -> MAJOR.
- **Versioning extensions:** use SemVer for your own spec extensions.
- **Wrong type used:** before release, amend via interactive rebase; after release, follow your process.
- **Non-spec type used:** not ideal but not catastrophic; tooling may ignore it.
- **All contributors required?** No. Squash workflows let maintainers fix messages at merge time.
- **Revert commits:** spec doesn’t define revert behavior; tooling decides. One recommendation is a `revert` type plus a footer listing reverted SHAs.

## FAQ (ID, terjemahan)
- **Tahap awal pengembangan:** anggap produk sudah dipakai; kejelasan commit tetap penting.
- **Huruf besar/kecil pada tipe:** bebas, tapi sebaiknya konsisten.
- **Cocok lebih dari satu tipe:** sebaiknya pecah jadi beberapa commit.
- **Khawatir menghambat cepat-iterasi:** ini mencegah kecepatan yang tidak teratur dan membantu kecepatan jangka panjang.
- **Takut tipe membatasi:** tipe bisa disesuaikan; utamakan kejelasan.
- **Relasi ke SemVer:** `fix` -> PATCH, `feat` -> MINOR, `BREAKING CHANGE` -> MAJOR.
- **Versi untuk ekstensi:** gunakan SemVer.
- **Salah pilih tipe:** sebelum rilis, perbaiki via `git rebase -i`; setelah rilis, ikuti proses yang berlaku.
- **Tipe di luar spesifikasi:** tidak ideal tetapi tidak fatal; tooling mungkin mengabaikan commit itu.
- **Apakah semua kontributor wajib?** Tidak. Dalam workflow squash, maintainer dapat memperbaiki pesan saat merge.
- **Commit revert:** tidak ditentukan spesifik; tooling menentukannya. Rekomendasi: gunakan tipe `revert` dan footer berisi SHA yang direvert.

## License (EN, paraphrase)
The specification is available under the Creative Commons CC BY 3.0 license.

## Lisensi (ID, terjemahan)
Spesifikasi ini tersedia di bawah lisensi Creative Commons CC BY 3.0.
