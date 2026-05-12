# Panduan Instalasi Arch Linux (Bahasa Indonesia)

Dokumen ini adalah panduan untuk menginstal Arch Linux menggunakan live system dari media instalasi resmi.

Arch Linux dikenal sebagai distribusi Linux minimalis dan fleksibel. Sistem ini memberi pengguna kontrol penuh terhadap paket dan konfigurasi yang dipasang.

---

# Persiapan Sebelum Instalasi

## 1. Download ISO Arch Linux

Unduh file ISO resmi dari:

https://archlinux.org/download/

### Penjelasan
File ISO adalah image sistem operasi yang digunakan untuk membuat bootable USB.

---

# 2. Verifikasi Signature ISO

Checksums resmi:

https://archlinux.org/download/#checksums

Verifikasi file ISO:

```bash
pacman-key -v archlinux-version-x86_64.iso.sig
```

### Penjelasan
Verifikasi memastikan file ISO tidak rusak atau dimodifikasi pihak lain.

---

# 3. Boot ke Live Environment

Setelah membuat bootable USB:

1. Masuk BIOS/UEFI
2. Boot dari USB
3. Pilih:
   - `Arch Linux install medium`

### Parameter HiDPI (opsional)

```bash
fbcon=font:TER16x32
```

### Penjelasan
Digunakan untuk memperbesar font terminal pada layar resolusi tinggi.

Dokumentasi parameter boot:

https://gitlab.archlinux.org/archlinux/mkinitcpio/mkinitcpio-archiso/blob/master/docs/README.bootparams

Daftar paket bawaan live ISO:

https://geo.mirror.pkgbuild.com/iso/latest/arch/pkglist.x86_64.txt

---

# 4. Mengatur Keyboard Layout

Lihat daftar layout keyboard:

```bash
localectl list-keymaps
```

Contoh layout Jerman:

```bash
loadkeys de-latin1
```

### Penjelasan
Secara default Arch menggunakan keyboard US.

Untuk Indonesia biasanya tidak perlu diubah.

---

# 5. Memeriksa Mode Boot

```bash
cat /sys/firmware/efi/fw_platform_size
```

### Hasil

- `64` → UEFI 64-bit
- `32` → UEFI 32-bit
- Error → BIOS/Legacy mode

### Penjelasan
Penting untuk menentukan jenis bootloader dan partisi yang digunakan.

---

# 6. Menghubungkan ke Internet

Cek interface jaringan:

```bash
ip link
```

Tes koneksi:

```bash
ping ping.archlinux.org
```

### Penjelasan
Internet diperlukan karena Arch mengunduh paket langsung dari mirror online saat instalasi.

Untuk WiFi gunakan:

```bash
iwctl
```

---

# 7. Sinkronisasi Waktu

```bash
timedatectl
```

### Penjelasan
Jam sistem harus benar agar verifikasi SSL dan package signature tidak gagal.

---

# Partisi Disk

Lihat disk:

```bash
fdisk -l
```

Partisi disk:

```bash
fdisk /dev/the_disk_to_be_partitioned
```

Contoh:

```bash
fdisk /dev/nvme0n1
```

---

# Struktur Partisi yang Disarankan

## UEFI

| Mount Point | Fungsi |
|---|---|
| `/boot` | EFI Partition |
| `swap` | Virtual memory |
| `/` | Root system |

## BIOS Legacy

| Mount Point | Fungsi |
|---|---|
| `swap` | Virtual memory |
| `/` | Root system |

---

# Format Partisi

```bash
mkfs.ext4 /dev/root_partition
```

### Penjelasan
Membuat filesystem ext4 pada partisi root.

---

## Membuat Swap

```bash
mkswap /dev/swap_partition
```

Aktifkan swap:

```bash
swapon /dev/swap_partition
```

### Penjelasan
Swap digunakan sebagai memori cadangan ketika RAM penuh.

---

## Format EFI Partition

```bash
mkfs.fat -F 32 /dev/efi_system_partition
```

### Penjelasan
EFI partition harus menggunakan FAT32.

---

# Mount Filesystem

Mount root:

```bash
mount /dev/root_partition /mnt
```

Mount EFI:

```bash
mount --mkdir /dev/efi_system_partition /mnt/boot
```

### Penjelasan
Partisi harus dipasang (mount) sebelum sistem diinstal.

---

# Instalasi Sistem Dasar

```bash
pacstrap -K /mnt base linux linux-firmware
```

### Penjelasan

- `base` → paket inti sistem
- `linux` → kernel Linux
- `linux-firmware` → firmware hardware

---

# Membuat fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### Penjelasan
fstab menentukan partisi mana yang otomatis dimount saat boot.

---

# Masuk ke Sistem Baru (Chroot)

```bash
arch-chroot /mnt
```

### Penjelasan
Mengubah root shell ke sistem Arch yang baru dipasang.

---

# Mengatur Timezone

```bash
ln -sf /usr/share/zoneinfo/Area/Location /etc/localtime
```

Contoh Indonesia:

```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```

Sinkronkan hardware clock:

```bash
hwclock --systohc
```

---

# Localization

Generate locale:

```bash
locale-gen
```

Isi `/etc/locale.conf`:

```ini
LANG=en_US.UTF-8
```

### Penjelasan
Mengatur bahasa dan format regional sistem.

---

# Hostname

Isi file:

```text
/etc/hostname
```

Contoh:

```text
myarchpc
```

### Penjelasan
Hostname adalah nama komputer di jaringan.

---

# Generate Initramfs

```bash
mkinitcpio -P
```

### Penjelasan
Membuat image boot awal Linux.

---

# Password Root

```bash
passwd
```

### Penjelasan
Mengatur password administrator/root.

---

# Install Bootloader

Pilihan umum:

- GRUB
- systemd-boot
- rEFInd

Dokumentasi:

https://wiki.archlinux.org/title/Arch_boot_process

---

# Reboot

Keluar dari chroot:

```bash
exit
```

Unmount:

```bash
umount -R /mnt
```

Reboot:

```bash
reboot
```

Lepas USB installer setelah restart.

---

# Resource Resmi

- https://archlinux.org/
- https://archlinux.org/download/
- https://wiki.archlinux.org/
- https://bbs.archlinux.org/
