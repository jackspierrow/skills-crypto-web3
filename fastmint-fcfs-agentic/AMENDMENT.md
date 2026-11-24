# AMENDMENT — fastmint-fcfs-agentic (local amendment)

Sumber: https://github.com/setyamickala/fastmint-fcfs-agentic, commit 55e744d
(2026-11-11). Clone: ~/workspace/fastmint-fcfs-agentic/repo/
Riset: ~/workspace/fastmint-fcfs-agentic/riset.md
Audit kode: ~/workspace/fastmint-fcfs-agentic/audit.md

## Vonis audit (2026-11-11, dibaca penuh semua file)

**CAUTION — kode bersih, pakai hanya dengan perubahan di bawah.**
- TIDAK ada perilaku jahat: tanpa eksfiltrasi, tanpa approve/transfer/permit
  tersembunyi, tanpa akses private key/seed/keystore, tanpa eval/base64,
  tanpa `baw auth signin`. Ethers 6.13.4 asli (spot-check).
- Safety rails README §6 benar-benar ditegakkan di kode: value > 0 wajib
  `--maxValue`, lock file sebelum execute + tidak ada retry setelah execute,
  risk block 351803 / RISK / allowance-authority → fatal, preview wajib
  simulationCode 000000000 + requestId + outflow ≤ cap + 0,003 BNB,
  PENDING_CONFIRMATION tidak dikirim ulang.
- CAUTION karena: (1) repo umur 1 hari dari author tak dikenal — tanpa track
  record; (2) `.env` bawaan berisi endpoint QuikNode milik author (kunci
  pihak ketiga — traffic terlihat operator, bisa dicabut).

## Aturan pakai (mengikat, di atas SKILL.md)

1. **`.env` bawaan TIDAK ikut di-install.** File `.env` di folder skill ini
   TIDAK ADA. Isi `.env` sendiri dari `.env.example` dengan RPC/WS milik
   sendiri, atau kosongkan — pool publik bawaan sudah cukup.
2. **Selalu `--dry` dulu** untuk tiap mint baru, dan baca baris `call = ...`
   di log — pastikan fungsi + argumen persis yang dimaksud.
3. **Free-only**: jalankan hanya dengan `--value 0`, kecuali user menyebut
   jumlah berbayar eksplisit — lalu pasang `--maxValue` yang sama.
4. **Jangan jalankan dua `fire` paralel** untuk mint yang sama (race lock).
5. **Jangan `bench --live`** tanpa persetujuan eksplisit user (biaya gas nyata).
6. **Jangan jalankan dengan wallet asli tanpa go-ahead user atas preview.**
   Preview harus ditunjukkan ke user dan disetujui dulu, kecuali mint gratis
   yang memenuhi SEMUA syarat auto-mint waiver 2026-10-31 (value 0, preview
   bersih, tanpa approval/transfer/permit/signature, kuota belum terpakai,
   BNB > 0,0001).
7. **Jangan hapus file lock** kecuali atas instruksi baru yang eksplisit.
8. Per-mint, verifikasi sendiri `--to` (alamat kontrak sale) dan
   `--data`/`--sig` dari sumber resmi proyek sebelum fire.

## File yang di-install vs dikecualikan

- Di-install: SKILL.md, fastmint.sh, fire.js, recon.js, bench.js, lib.js,
  ethers.umd.min.js (+ .env.example dan AMENDMENT.md ini, dibuat lokal).
- Dikecualikan: `.env` bawaan repo (endpoint QuikNode author), README.md,
  README.id.md, runs/ (log run lama).
- Sanity check 2026-11-11: `node --check` lolos untuk fire.js, recon.js,
  bench.js, lib.js (parse saja, tanpa eksekusi). TIDAK ada live run, TIDAK
  ada wallet terkonek, TIDAK ada signing, TIDAK ada dependensi di-install.
