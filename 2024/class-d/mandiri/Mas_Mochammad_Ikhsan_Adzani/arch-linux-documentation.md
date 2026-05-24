# Dokumentasi install arch manual
## jika sudah masuk kedalam install arch linux nya langsung ikutin langkah dibawah

# CHECKING PARTISI
## jika ingin melihat partisi beserta type nya
```
lsblk -o name,fstype,size 
```
## Jika ingin melihat partisinya saja
```
lsblk
```

# MEMBAGI PARTISI
```
cfdisk /dev/[partisi] (untuk membentuk layout yg mah di install)
```
