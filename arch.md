## 1 | Virtual Machine Setup (UTM)

- Other
    - Boot Device: CD/DVD Image
    - Boot ISO Image: archlinux-2024.10.01-x86_64.iso
- Hardware
    - Architecture: x86_64
    - System: Standard PC (Q35 + ICH9, 2009) (alias of pc-q35-7.2) (q35)
    - Memory: 4096 MiB
    - CPU Cores: Default
- Storage
    - Drive Size: 64 GiB
- Shared Directory
    - None

## 2 |  Initial Configuration

Verify boot mode (expected “64”)
`cat /sys/firmware/efi/fw_platform_size`

Verify internet connection

`ping bentonraymer.com`

Verify system clock

`timedatectl`

### 2.1 | Partition Disk

`fdisk -l`

`fdisk /dev/sda`

`g` sets partition table to type GPT

`n` / `1` / `2048` / `+500M` creates 500MiB partition

`t` / `1` changes partition to EFI

`n` / `[enter]` / `[enter]` / `[enter]` assigns remaining space to partition 2

`w` write changes

### 2.2 | Format & Mount

Format EFI partition as Fat32:

`mkfs.fat -F 32 /dev/sda1`

Format Root partition as EXT4:

`mkfs.ext4 /dev/sda2`

Mount both partitions:

`mount --mkdir /dev/sda1 /mnt/boot`

`mount /dev/sda2 /mnt`

## 3 | Installation

Add packages:

`pacstrap -K /mnt base linux linux-firmware networkmanager nano man-db man-pages texinfo bash curl grep less sudo tar which`

Generate fstab:

`genfstab -L /mnt >> /mnt/etc/fstab`

Change root:

`arch-chroot /mnt` 

Set timezone:

`ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime`

`hwclock --systohc`

Set locale:

`nano /etc/locale.gen`

*Uncomment* `en_US.UTF-8 UTF-8`

`locale-gen`

`echo 'LANG=en_US.UTF-8' > /etc/locale.conf`

Set hostname:

`echo 'benton-arch1' > /etc/hostname`

Change password:

`passwd`

## 4 | Boot Manager

Install boot manager:

`pacman -Sy grub efibootmgr`

Grub instalation:

`grub-install --target=x86_64-efi --efi-directory=boot --bootloader-id=GRUB`

Make grub config:

`grub-mkconfig -o /boot/grub/grub.cfg`

Exit chroot:

`exit`

`reboot`

## 5 | Inside Arch

Set up & check network:

`systemctl enable NetworkManager`

`systemctl start NetworkManager`

`ping bentonraymer.com`

Install Display Server:

`pacman -S xorg lightdm lightdm-gtk-greeter`

Install Cinnamon:

`pacman -S cinnamon cinnamon-translations nemo-fileroller nemo-image-converter nemo-preview xed xreader gnome-terminal metacity gnome-shell`

Enable lightdm:

`systemctl enable lightdm`

## 6 | Custom Changes

### 6.1 | Users

Create Users:

`useradd -m benton`

`useradd -m justin`

`useradd -m codi`

Set Passwords:

`passwd benton`

`passwd justin`

`passwd codi`

Force Password Reset:

`passwd -e justin`

`passwd -e codi`

Install Vi:

`pacman -S vi`

Make visudo use Nano:

`export EDITOR=nano`

Use visudo:

`visudo`

Uncomment this: `%sudo ALL=(ALL) ALL`

Sudo Group:

`groupadd sudo`

`usermod -aG sudo benton`

`usermod -aG sudo justin`

`usermod -aG sudo codi`

### 6.2 | Install Packages

`pacman -S zsh`

`pacman -S openssh`

`pacman -S firefox`

### 6.3 | Other Customization

Enable colors by editing `/etc/pacman.conf` and `.bashrc`

Add Aliases:

`alias ls='ls --color=auto'`

`alias restart=reboot`
