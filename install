DISK(){
    ONE="1"
    TWO="2"
    THREE="3"
    DEVSTR="/dev/"
    EFIDISKSIZE="+128M"
    
    lsblk
    echo -en "Please input your disk number (such as : sda)"
    read DISK
    DEVDISK="$DEVSTR$DISK"
    echo $DISK
    echo $DEVDISK
    echo -en "o\nn\n\n\n\n$EFIDISKSIZE\nn\n\n\n\n\nw\n" | sudo fdisk $DEVDISK

    yes | mkfs.fat -F 32 "$DEVDISK$ONE"
    yes | mkfs.ext4 "$DEVDISK$TWO"

    mkdir -p /mnt/boot/efi
    mount "$DEVDISK$TWO" /mnt
    mount "$DEVDISK$ONE" /mnt/boot/efi
}
ChrootFront(){
    DISK

    yes | pacman -S archlinux-keyring
    echo -en "\n\n\n\n\n\n\n\n\n\n\n" | pacstrap -i /mnt base base-devel linux linux-firmware

    genfstab -U -p /mnt > /mnt/etc/fstab
    
    arch-chroot /mnt

    echo "Chroot done...."

}
ChrootBehind(){

}
STARTINSTALL(){

    if[ $# -lt 3 ] ; then
        ChrootBehind
    else
        ChrootBehind
    fi

}


NOSTART(){
    echo "Bye"
}


Main(){
    echo -en "I always install arch,but i haven't more time,so write this bash.\nThis bash will think your install system desk is /dev/sdb2,if you shuld modify this,please modify this bash file.If you read all tips ,Please confirm to continue.[Y/N]"
    read ConfirmStart
    case "$ConfirmStart" in
        Y|y|YES|Yes|yes)
            STARTINSTALL ;;
        *)
            NOSTART ;;
    esac
}
Main
