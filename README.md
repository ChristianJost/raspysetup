Based on https://github.com/phortx/Raspberry-Pi-Setup-Guide


## Keyboard, Language and Timezone

```
echo KEYMAP=de_CH-latin1 > /etc/vconsole.conf
echo LANG=de_CH.UTF-8 > /etc/locale.conf  
rm /etc/localtime  
ln -s /usr/share/zoneinfo/Europe/Zurich /etc/localtime  
sed -i "s/en_US.UTF-8/#en_US.UTF-8/" /etc/locale.conf  
locale-gen de_CH.UTF-8
```
## Setup swapfile
```
fallocate -l 1024M /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
echo 'vm.swappiness=1' > /etc/sysctl.d/99-sysctl.conf
```
## Then add the following line to /etc/fstab:
```
swapfile none swap defaults 0 0
```

## Set hardware clock to UTC and set timezone
```
timedatectl set-local-rtc 0
nano /etc/timezone
Set to "Europe/Zurich"
```
# Update system and enable NTP
## Tweak pacman
```
sed -i 's/#Color/Color/' /etc/pacman.conf # Add color to pacman
```
## System update
```
pacman -Sy pacman
pacman-key --init
pacman -S archlinux-keyring
pacman-key --populate archlinux
pacman -Syu --ignore filesystem
pacman -S filesystem --force
reboot
```

## NTP
```
pacman -S ntp fake-hwclock
systemctl enable fake-hwclock.service
systemctl enable ntpd.service
systemctl start ntpd.service
```

# Advanced setup
## Set a secure root passwd
```
passwd
```
## Set hostname
```
hostnamectl set-hostname gorizont-11L
```
## sudo & user
```
pacman -S sudo vim
visudo
#Search for following line and uncomment it:
%wheel ALL=(ALL) ALL
```
## Add a new user (replace yourUserName with your username!)
```
useradd -d /home/<username> -m -G wheel -s /bin/bash <username>
```
## Set a password for your new user:
```
passwd <username>
```
Log out and log in with our newly created user

## After that, delete the old alarm user:
```
sudo userdel alarm
```

## Additional software
```
sudo pacman -S --needed htop openssh autofs alsa-utils alsa-firmware alsa-lib alsa-plugins git wget diffutils wpa_supplicant wireless_tools iw crda lshw neofetch multitail
```

## Wi-Fi
```
sudo wifi-menu -o
netctl start <wlanprofilename>
netctl enable <wlanprofilename>
```
