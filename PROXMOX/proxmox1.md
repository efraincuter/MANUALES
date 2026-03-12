# Proxmox 9.1.1
### Adicionar un nuevo disco en un servidor
#### 1.- Ver con fdisk -l y lsblk el disco PERC que es sda/sda1
```
root@v1:~# fdisk -l
Disk /dev/sda: 14.55 TiB, 16001974403072 bytes, 31253856256 sectors
Disk model: PERC H730P Adp  
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 85C345C3-A562-4967-9981-8C0A04320527

root@v1:~# lsblk 
NAME               MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda                  8:0    0 14.6T  0 disk 
└─sda1               8:1    0 14.6T  0 part 
sdb                  8:16   1 29.3G  0 disk 
├─sdb1               8:17   1 29.3G  0 part 
└─sdb2               8:18   1   32M  0 part 
sdc                  8:32   0 14.9G  0 disk 
├─sdc1               8:33   0 1007K  0 part 
├─sdc2               8:34   0  512M  0 part /boot/efi
└─sdc3               8:35   0 14.4G  0 part 
  ├─pve-swap       252:0    0    1G  0 lvm  [SWAP]
  ├─pve-root       252:1    0  6.7G  0 lvm  /
  ├─pve-data_tmeta 252:2    0    1G  0 lvm  
  │ └─pve-data     252:4    0  4.7G  0 lvm  
  └─pve-data_tdata 252:3    0  4.7G  0 lvm  
    └─pve-data     252:4    0  4.7G  0 lvm  
```
#### 2.- Limpiar el disco
```
En la interfaz web ingresar a el nombre del VM y luego Disk, seleccionar el disco y limpiarlo, y luego revisar

root@v1:/etc/apt/sources.list.d# lsblk 
NAME               MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda                  8:0    0 14.6T  0 disk 
sdb                  8:16   1 29.3G  0 disk 
├─sdb1               8:17   1 29.3G  0 part 
└─sdb2               8:18   1   32M  0 part 
sdc                  8:32   0 14.9G  0 disk 
├─sdc1               8:33   0 1007K  0 part 
├─sdc2               8:34   0  512M  0 part /boot/efi
└─sdc3               8:35   0 14.4G  0 part 
  ├─pve-swap       252:0    0    1G  0 lvm  [SWAP]
  ├─pve-root       252:1    0  6.7G  0 lvm  /
  ├─pve-data_tmeta 252:2    0    1G  0 lvm  
  │ └─pve-data     252:4    0  4.7G  0 lvm  
  └─pve-data_tdata 252:3    0  4.7G  0 lvm  
    └─pve-data     252:4    0  4.7G  0 lvm  

root@v1:/etc/apt/sources.list.d# fdisk -l
Disk /dev/sda: 14.55 TiB, 16001974403072 bytes, 31253856256 sectors
Disk model: PERC H730P Adp  
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
```
