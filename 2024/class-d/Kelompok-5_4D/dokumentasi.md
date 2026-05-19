# TUTORIAL INSTALASI BASE ARCH LINUX
## 1. Download ISO Arch Linux
download iso arch linux menggunakan link berikut archlinux-2026.05.01-x86_64.iso  
## 2. Membuat Bootable USB

## 3. Boot ke flashdisk installer
colokkan flashdisk yang sudah berisi file bootable arch linux

restart laptop

tekan FN + F12


## 4. Sambungkan ke internet
gunakan ```iwctl```

```station wlan0 scan```

```station wlan0 get-networks```

```station wlan0 connect namawifi```
masukkan password wifi jika diminta

## 5. Cek struktur disk dan partisi
```lsblk```

untuk melihat nama disk, ukuran partisi, struktur storage

contoh disk ```/dev/nvme0n1```

## 6. Membuat Partisi
masuk ke cfdisk
```cfdisk /dev/nvme0n1```

## 7. Membagi Partisi
Pilih Free Space

Lalu bagi partisi sesuai pengkondisian penyimpanan yang sudah dibagi, kelompok kami memakai

1 GB untuk boot, type EFI system

4 GB untuk swap, type Linux swap

14 GB untuk root, type Linux filesystem

## 8. Membuat filesystem
```lsblk```

contoh hasilnya 

/dev/nvme0n1p5 untuk boot

/dev/nvme0n1p6 untuk swap

/dev/nvme0n1p7 untuk root

format root ke ext4

```mkfs.ext4 /dev/nvme0n1p7```

format partisi tambahan

```mkfs.ext4 /dev/nvme0n1p6```

aktifkan swap (swap digunakan sebagai cadangan ketika RAM penuh)

```mkswap /dev/nvme0n1p6```

```swapon /dev/nvme0n1p6```

format boot ```mkfs.fat -F 32 /dev/nvme0n1p5```

## 9. Mount Partisi

Partisi harus dipasang sebelum sistem diinstall

mount root ```mount /dev/nvme0n1p7 /mnt```

membuat folder boot ```mkdir -p /mnt/boot```

mount boot ```mount /dev/nvme0n1p5 /mnt/boot

## 10. Install base system
install package dasararch linux ```pacstrap -K /mnt base-devel linux linux-firmwarenvim amd-ucode (jika intel maka menjadi intel-ucode)```


