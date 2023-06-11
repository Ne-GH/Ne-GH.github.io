---
redirect_from: /_posts/2023-6-1-使用tar命令进行Arch备份及还原.md/
title: 使用tar命令进行Arch备份及还原
tags: Linux
---
# 备份

`` cd / ``  
  
``
tar -cvpzf /backup-`date +%Y-%m-%d`.tar.gz --exclude=/proc --exclude=/sys --exclude=/dev --exclude=/run --exclude=/mnt --exclude=/media --exclude=/tmp --exclude=/lost+found --exclude=/var/log --exclude=/var/cache/apt/archives --exclude=/usr/src/linux-headers* --exclude=/home/*/.gvfs --exclude=/home/*/.cache --exclude=/home/*/.local/share/Trash /
``   
  
``  --exclude=/dir `` 命令可以排除指定文件夹

# 还原
## 新建分区  
1、使用``fdisk -l`` 命令查看磁盘设备，并使用``fdisk /dev/sda`` ,这里假定系统在``/dev/sda``中  
2、进入fdisk中后使用``g``新建分区表（这里使用的是GPT+UEFI,如果使用MBR+Legacy BIOS应该使用``o``）  
3、使用``n``建立efi分区，盘号和起始位置使用默认，大小此处使用``+512M``,并使用再次使用``n``创建新的系统分区和其余分区    
4、``w``保存   
## 格式化
``mkfs.fat -F32 /dev/sda1`` (efi分区使用fat)  
``mkfs.ext4 /dev/sda2``

## 挂载
``mount /dev/sda2 /mnt``  
``mkdir -p /mnt/boot/efi``  
``mount /dev/sda1 /mnt/boot/efi``  

## 解压备份文件
``cp /U/backup_file_name.tar.gz /mnt``  (假定备份文件已经在/U中了，可以使用mount挂载u盘)  
``cd /mnt``
``tar -xvzpf /backup_file_name.tar.gz -C /mnt --numeric-owner`` 

# 重新建立引导
## 进入/mnt
``arch-chroot /mnt /usr/bin/bash``(直接使用``arch-chroot /mnt``好像会使用/usr/bin/zsh,但是并不确定，可能是我之前配置有问题)  
## 新建引导
UEFI  
``pacman -S grub efibootmgr``  
``grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=Arch``  
``grub-mkconfig -o /boot/grub/grub.cfg`` 
BIOS  
``pacman -Sy grub``  
``grub-install --target=i386-pc $DEVDISK``  
``grub-mkconfig -o /boot/grub/grub.cfg``  

## 事后还需要的命令
``genfstab -U -p /mnt > /mnt/etc/fstab``  
``mkinitcpio -p linux``
