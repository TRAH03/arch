just in case you need to read the friendly manual [Arch Wiki](https://wiki.archlinux.org/title/Installation_guide)

Check if your in UEFI or BIOS mode
```
cat /sys/firmware/efi/fw_platform_size
```
if it returns 64 your in UEFI and good to go, if it returns 32 or another error check the Wiki

Check your Ethernet
```
ip link
```

Check internet access
```
ping google.com
```

Update clock
```
timedatectl
```

Partition disk
```
fdisk -l  #lists disks and their partitions
```

Select the drive
```
fdisk /dev/drive_your_formatting
```

example partition layout for a 512M EFI partition and using the rest of the drive for the OS. You can also add a swap partition here now or after install if you have less than 16GB of RAM, it should be the same size as your RAM.
```
g        # Create new GPT table
n        # New partition (EFI)
+512M
t        # Change type
1        # EFI System

n        # New partition (root)
<enter>  # Default start
<enter>  # Use rest of disk

w        # Write changes
```
the w makes it permanent

format the partitions
```
mkfs.fat -F32 /dev/nvme0n1p1   # EFI
mkfs.ext4 /dev/nvme0n1p2       # Root
```

If you created a swap partition initialize it
```
mkswap /dev/_swap_partition_
```
mount the partitions
```
mount /dev/nvme0n1p2 /mnt
mkdir /mnt/boot
mount /dev/nvme0n1p1 /mnt/boot
```

#### Pacstrap Install Packages.
This is the OS, plus some stuff that you need.

```
pacstrap -K /mnt base linux linux-firmware linux-headers networkmanager nano vim dosfstools exfatprogs ntfs-3g grub os-prober tzdata efibootmgr sudo 
```
this includes the grub bootloader instead of systemdboot that the archWiki will tell you to use. You’ll need to edit the grub config and uncomment the line enabling the OS prober at the very bottom using nano

Generate fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```

Chroot into the new OS
```
arch-chroot /mnt
```

Set the time
```
ln -sf /usr/share/zoneinfo/_Region_/_City_ /etc/localtime
```
```
hwclock --systohc
```
```
locale-gen
```

Edit /etc/locale.gen and uncomment your locale en_US.UTF-8 UTF-8
```
nano /etc/locale.gen
```

then run
```
locale-gen
```
```
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

set your computer hostname
```
echo "myhostname" > /etc/hostname
```

Run this to ensure you have internet each time you boot
```
systemctl enable NetworkManager
```

Install your bootloader
```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

edit `/etc/default/grub` with nano
uncomment this line
```
GRUB_DISABLE_OS_PROBER=false
```
generate the grub config
```
os-prober   # Should list your Windows installation
```
```
grub-mkconfig -o /boot/grub/grub.cfg
```

create your user account
```
useradd -m -G wheel -s /bin/bash myusername
```
```
passwd myusername
```

Enable sudo by editing the sudoers file
```
EDITOR=nano visudo
```
uncomment this line:
```
%wheel ALL=(ALL:ALL) ALL
```


You can set a password for the root user if you want, if not it’ll disable root (recommended)
```
passwd
```



You can now reboot into the OS and login as your user.

### NVIDIA Driver Packages
The lib32 packages need multilib enabled in `/etc/pacman.conf` then run pacman -Sy (optional, but better game compatibility) 

```
sudo pacman -S nvidia-dkms nvidia-utils nvidia-settings lib32-nvidia-utils libva libva-nvidia-driver
```

### Plasma Desktop and Display Manager
If you choose a different desktop or window manager it’ll have different app lists
```
sudo pacman -S plasma-meta dolphin konsole kate gwenview okular sddm ark
```
you can also add a web browser in here like Firefox or Vivaldi (chrome requires the AUR or flatpak)

```
sudo systemctl enable sddm
```
Then reboot

### Post Install Packages
these can also be included in the pacstrap command but aren’t necessary for first boot
```
sudo pacman -S base-devel unzip zip p7zip tar htop man-db man-pages usbutils pciutils ethtool fastfetch git curl wget pipewire pipewire-alsa pipewire-pulse pipeware-jack wireplumber
```

## Flatpak
Most proprietary apps will be on flathub since its distro agnostic and flatpaks are more sandboxed with a permission system just like phone apps. These are stored in your home folder so if you decide to keep home on a separate partition they will remain after an OS reinstall or distro hop.

```
sudo pacman -S flatpak
```
```
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
for apps like Spotify, Discord, Chrome, VLC, Bitwarden, Obsidian, prism launcher, flatseal, localsend, plex, gear lever, etc
You can browse these apps with the discover store or install with the terminal. Updates are managed by the store GUI or terminal.
## AUR
you can find just about anything here but all the packages are unofficially maintained by the community so grab a flatpak or pacman package if available
```
sudo pacman -S --needed base-devel git    #if you didn't grab them before
```
```
git clone https://aur.archlinux.org/yay.git
```
```
cd yay
```
```
makepkg -si
```
For apps like Chrome, Heroic Games Launcher (Epic/GOG/Amazon), and VS Code
you can find AUR packages on the website [AUR](https://aur.archlinux.org)

you can update all system packages and AUR packages with `yay -Syu`

Sometimes Bluetooth doesn't work right after install if so check the service
```
systemctl status bluetooth
```

if its disabled enable it so it starts on boot
```
sudo systemctl enable bluetooth
```
```
sudo systemctl start bluetooth
```

## Gaming Packages
```
sudo pacman -S steam lutris mangohud gamemode wine winetricks vulkan-tools gvfs lib32-gamemode lib32-gnutls lib32-vkd3d vkd3d
```

## Optional Packages
```
Sudo pacman -S zsh 
```
