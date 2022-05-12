# Arch install

## 1. Connect to network

```
iwctl
```

```
station wlan0 scan
station wlan0 show
station wlan0 connect "SSID"
```

<br/>

## 2. Create partitions

```
cfdisk /dev/sdX
```

> Create Boot partition `100 to 500 MB`

> Create swap partition `2 to 16 GB`

> Create Root partition `Rest of disk`

### Set file systems

```
mkfs.ext4 /dev/root_partition
```

```
mkswap /dev/swap_partition
```

```
mkfs.fat -F 32 /dev/efi_system_partition
```

<br/>

### 3. Mount partitions

```
mkdir -p /mnt/boot/efi

mount /dev/root_partition /mnt
mount /dev/efi_system_partition /mnt/boot/efi
```

```
swapon /dev/swap_partition
```

<br/>

### 4. Install default packages

```
pacstrap /mnt base linux linux-firmware grub efibootmgr iwd networkmanager net-tools dhcpcd vim git htop neofetch node icu
```

<br/>

### 5. Generate Fstab files

```
genfstab -U /mnt >> /mnt/etc/fstab
```

<br/>

### 6. Become root

```
arch-chroot /mnt
```

<br/>

### 7. Hostname

edit the host name in

```
vim /etc/hostname
```

<br/>

### 8. Root password

```
passwd
```

<br/>

### 9. Install GRUB efi as removable disk

```
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB --removable --recheck --no-nvram
```

<br/>

### 10. Safely exit

```
exit
umount -R /mnt
reboot
```

<br/>

# Post installation

### 1. Create User

```
useradd -m -G wheel -s /bin/bash YOUR_USER_NAME
```
