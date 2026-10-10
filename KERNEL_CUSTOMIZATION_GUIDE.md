# KitsuneDnm OC GPU Kernel - Panduan Kustomisasi untuk Android 16 A52

Dokumen ini menjelaskan struktur kernel custom untuk Samsung Galaxy A52 (SM7125) dengan optimasi GPU OC.

## 📋 Informasi Kernel
- **Kernel Version**: Linux 4.14.356
- **Nama Custom**: KitsuneDnm_oc_gpu_700
- **Device Target**: Samsung Galaxy A52 (A52Q, SM-A520F, SM-A525F, SM-A528B)
- **OS Target**: Android 16 (LineageOS 23.0)
- **Chipset**: Snapdragon 720G (SM7125)

## 📁 Struktur File Penting

```
android_kernel_samsung_sm7125/
├── Makefile                          # Konfigurasi build kernel
├── arch/arm64/                       # Arsitektur ARM64
│   ├── boot/                         # Boot loader config
│   ├── configs/                      # Defconfig files untuk A52
│   └── ...
├── drivers/                          # Device drivers
│   ├── gpu/                          # GPU drivers (Adreno)
│   └── ...
├── .github/workflows/
│   └── main.yml                      # Workflow CI/CD untuk build otomatis
└── Documentation/                    # Dokumentasi kernel
```

## 🔧 Cara Menggunakan Workflow

### 1. **Trigger Build Manual**
- Buka: https://github.com/amirarjenso2003-dot/android_kernel_samsung_sm7125/actions
- Klik workflow "Build KitsuneDnm_oc_gpu_700"
- Pilih branch: `lineage-23.0`
- Klik "Run workflow"

### 2. **Hasil Build**
Setelah ~120 menit, akan ada file:
- `KitsuneDnm_oc_gpu_700_A52.zip` - Flashable kernel untuk A52

## 🎯 Kustomisasi Kernel

### Mengubah GPU Frequency (OC)
File yang perlu dimodifikasi:
```
drivers/gpu/msm/
```
Cari config GPU frequency, ubah nilai max frequency dari default ke nilai yang diinginkan.

### Mengubah CPU Frequency Scaling
File:
```
drivers/cpufreq/
```

### Mengubah I/O Scheduler
File:
```
arch/arm64/configs/a52q_defconfig
```

## ⚙️ Konfigurasi Build

### Target Device
- Model: A52Q (SM7125)
- Alternative: SM-A520F, SM-A525F, SM-A528B

### Compiler
- Clang r522817 (AOSP Release)
- Fallback: LineageOS Clang (jika Google CDN fail)
- GCC Cross-compile: aarch64-linux-gnu

### Hasil Output
- **Image**: `out/arch/arm64/boot/Image.gz` (~30-50MB)
- **Modules**: `out/arch/arm64/boot/modules`
- **DTB** (Device Tree Binary): Include in AnyKernel3

## 📝 Langkah Kustomisasi yang Tepat

1. **Clone & Buat Branch Sendiri** (opsional)
   ```bash
   git checkout -b custom-kernel-v1 lineage-23.0
   ```

2. **Modifikasi Konfigurasi**
   - Edit defconfig sesuai kebutuhan
   - Atau gunakan `make menuconfig`

3. **Rebuild dengan Workflow**
   - Push ke branch Anda
   - Trigger workflow

4. **Test Hasil**
   - Download artifact
   - Flash di device A52
   - Test performa & stability

## 🚨 Penting!
- **Jangan ubah EXTRAVERSION** jika ingin custom name
- **Backup stock kernel** sebelum flash
- **Test di emulator/device lama** dulu
- **Compile log** tersimpan di workflow output

## 📞 Troubleshooting

| Masalah | Solusi |
|---------|--------|
| Build fail | Cek compile.log di workflow output |
| Image.gz tidak terbentuk | Cek defconfig kompatibilitas |
| Device tidak boot | Defconfig salah atau driver missing |
| Performance kurang | Cek GPU/CPU frequency settings |

---
Dibuat: 2026-10-10 | Target OS: LineageOS 23.0 (Android 16)
