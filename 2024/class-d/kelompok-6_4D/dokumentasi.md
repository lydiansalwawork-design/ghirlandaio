## setelah masuk Arch install dari BIOS 

## masukan internet / koneksi internet
__iwctl__ <br>
untuk masuk ke iwd 
__device list__  <br> 
untuk melihat list device
 __station [nama_device] scan__ <br>
 untuk scan 
__station [nama device] get-networks__ <br>
untuk mengambil dari scan
__station [nama_device] connect__ <br>
untuk koneksi ke wifi 
__station [nama_device} show__ <br> 
untuk melihat status 
exit  <br>

## lihat partisi 
 __fdisk -l__ <br>
 untuk melihat partisi 
__lsblk__ <br>
fungsi yang sama tetapi lebih minim dan fungsi melihat mana yang di block 

## membuat partisi 

__cfdisk__ <br>
untuk mengubah partisi secara keseluruhan <br>
cfdisk ke partisi contoh __/dev/sda__ or __/dev/nvme0n1__ <br>

cfdisk /dev/nvmeon1 <br>
cfdisk /dev/sda <br>

contoh tabel untuk bentukan partisinya 
| sda/nvme0n1 | format type | mount point | G |
|:------------|:------------|:------------|:---|
|sda1/nvme0n1p1    | mkfs.fat    | /mnt/boot   | 1G |
|sda2/nvme0n1p2    | mkswap      | swapon      | 4G |
|sda3/nvme0n1p3    | mkfs.ext4   | /mnt        | 50G |

## membuat format 

 __mkfs.fat -F 32__
 untuk membuat format efi boot loader <br>
contoh  /dev/sda1 or /dev/nvme0n1p1 <br>
mkfs.fat -F 32 /dev/nvme0n1p1 <br>
mkfs.fat -F 32 /dev/sda1 <br> 

pilih sesuai type kalian apakah nvme atau sda 
__mkkswap__
untuk membuat format swap <br>
contoh /dev/sda2 or /dev/nvme0n1p2 <br> 
mkswap /dev/nvme0n1p2 <br>
mkswap /dev/sda2 <br> 

pilih sesuai type kalian apakah nvme atau sda 
__mkfs.ext4__
 untuk membuat format root 
 contoh sda/3 or /dev/nvme0n1p3 <br>
 mkfs.ext4 /sda3 <br>
 mkfs.ext4 /nvme0n1p4 <br>  
ikuti sesuai type kalian apakah sda atau nvme0n1

## me mounting point 
### mounting partisi bentuk nvmeon1
mount /dev/nvme0n1p3 /mnt <br>
untuk me mount partisi root alias /mnt 
mount --mkdir /dev/nvme0n1p1 /mnt/boot <br>
untuk me mount partisi ke boot nantinya 
swapon /dev/nvme0n1p2  <br>
untuk mengaktifkan fungsi swap 

setelah format dan mounting berhasil ketik 

## pacstrap
pacstrap -K /mnt intel-ucode base base-devel linux linux-firmware git neovim iwd
pacstrap -K /mnt amd-ucode base base-devel linux linux-firmware git neovim iwd

pakai intel untuk intel, pakai amd untuk amd , ini di tentukan prosessor kalian, ini service untuk bug dan semacamnya 

## fstab
fstab -U /mnt >> /mnt/etc/fstab
fungsinya untuk menyiapkkan file yang sudah di mount tadi

## arch-crhoot /mnt 
masuk ke root dengan mengetik ini 
arch-chroot /mnt 

menyesuaikan waktu 
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc 

ini untuk menyesuaikan waktu dan jam agar tersinkronniasasi 






