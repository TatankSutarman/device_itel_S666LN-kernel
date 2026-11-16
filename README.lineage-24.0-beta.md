# lineage-24.0-beta — kernel set (WIP)

Branch ini disiapkan untuk **Linux 6.12** di itel RS4 (S666LN / MT6789),
dipasangkan dengan LineageOS 24.0 device tree branch `lineage-24.0-beta`.

## Status

⚠️ **BELUM bisa dipakai untuk build ROM.** Ini baru input dasar hasil ekstraksi
firmware stok V1661 + referensi branch `sixteen`.

### Yang sudah ada
- `dtb/mt6789.dtb` — DTB dari `vendor_boot.img` stok (200.368 B, perlu 64-byte
  MediaTek header dibuang dan direkonstruksi sebagai DTS sebelum dipakai).
- `dtbo.img` — DTBO stok (8 MB), harus dipasangkan dengan DTB baru.
- `ramdisk/modules.load` — 171 entri (stok; identik dengan `sixteen`).
- `ramdisk/modules.load.recovery` — 199 entri (stok).
- `vendor_dlkm/modules.load` — 177 entri (stok).

### Yang WAJIB diganti sebelum build
- `Image.gz` — **belum ada**. Harus berasal dari GKI
  `android16-6.12-2026-09_r10`, CI `16736897` (Kleaf mixed build).
- Semua `.ko` di `ramdisk/` dan `vendor_dlkm/` — masih **5.10.237-android12**
  (diverifikasi dari `vermagic=`). Harus diganti dengan modul hasil Kleaf DDK
  yang dibangun terhadap GKI 6.12 tersebut. Jangan campur modul 5.10 dengan
  kernel 6.12.
- `system_dlkm/flatten/lib/modules/*.ko` + `system_dlkm/modules.load` — modul
  GKI belum di-stage. `BoardConfig.mk` di device tree sudah menyediakan blok
  bersyarat untuk ini.

## Gate sebelum flash
1. `vermagic` semua `.ko` == `uname -r` kernel 6.12.
2. Tidak ada simbol unresolved (`modpost`); `module_layout` + CRC cocok dengan
   metadata CI 16736897.
3. Tidak ada tabrakan dengan driver built-in GKI.
4. Import simbol harus masuk Google `gki/aarch64/symbols/*` (tanpa allowlist
   privat).
5. Tepat satu `.dtb` di `dtb/`; DTBO dipasangkan.

Lihat `PLAYBOOK.md` dan `CHECKLIST.md` di workspace persiapan untuk detail.
