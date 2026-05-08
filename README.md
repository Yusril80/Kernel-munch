# femboy_v1 Kernel (POCO F4 / munch)

Kernel ini disiapkan sebagai basis porting untuk **POCO F4 (munch)** menggunakan source dari kernel **e404** (workflow/struktur umum), dengan nama kernel diganti menjadi:

- `femboy_v1`

## Target

- Device: POCO F4 (`munch`)
- Kernel name: `femboy_v1`
- Fokus: performa stabil harian + manajemen RAM yang lebih baik
- Integrasi: dukungan **KernelSU Next** (KSU Next)

## Fitur Utama

### 1) Rename Kernel
Nama kernel diubah menjadi `femboy_v1` pada konfigurasi build sehingga string identitas kernel mengikuti branding ini saat runtime.

### 2) Manajemen RAM yang lebih baik
Tune yang dipakai berfokus pada responsivitas multitasking tanpa terlalu agresif membunuh aplikasi latar belakang:

- **Multi-Gen LRU (MGLRU)** untuk reclaim memory yang lebih efisien.
- **PSI + vmpressure awareness** untuk perilaku reclaim yang lebih adaptif.
- **zRAM tuning** (kompresi + size policy) untuk menjaga free memory lebih lama.
- **Swap readahead & swappiness tuning** agar transisi ke swap lebih halus.
- **Perbaikan LMK profile** agar foreground app lebih aman saat tekanan RAM tinggi.

Contoh parameter runtime (bisa di-override per-ROM):

```bash
vm.swappiness=140
vm.watermark_boost_factor=0
vm.watermark_scale_factor=125
vm.page-cluster=0
vm.vfs_cache_pressure=80
```

### 3) KernelSU Next Ready
Kernel disiapkan dengan alur integrasi **KernelSU Next** agar manajemen root modern tetap kompatibel pada kernel branch terbaru, termasuk penyesuaian patch set secukupnya mengikuti base source yang dipakai.

## Catatan Implementasi

Karena repository ini saat ini minimal, implementasi low-level kernel source (Kconfig/defconfig, mm/*, drivers/*) belum dimasukkan di sini.
Dokumen ini dipakai sebagai baseline requirement untuk:

1. Import source kernel e404 yang sesuai dengan `munch` branch.
2. Rename string kernel menjadi `femboy_v1`.
3. Terapkan patch memory-management dan KSU Next di source tree kernel.
4. Build, boot test, dan stress test (RAM pressure, gaming, kamera, idle drain).

## TODO Teknis Lanjutan

- [ ] Sync source e404 terbaru yang stabil untuk `munch`.
- [ ] Tambah patchset RAM management (MGLRU + LMK tuning) di branch kerja.
- [ ] Integrasi KernelSU Next dengan commit yang kompatibel.
- [ ] Siapkan AnyKernel3 package untuk flashing.
- [ ] Uji pada MIUI/AOSP (Android 13/14) dan dokumentasikan regression.

---

Jika kamu mau, next step aku bisa langsung bikinin struktur branch kerja lengkap (`defconfig`, preset sysctl, dan template changelog) biar tinggal masukin source kernel e404 asli.
