# Passage au nouveau système

Maintenant que le disque est partitionné, tu vas pouvoir passer à l'installation.

### Montage
Tu remplaceras `/dev/part` par les partitions respectives que tu as configuré juste au-dessus.

```sh
# Partition /
$: mount /dev/part /mnt

# Partition /boot/EFI
$: mkdir /mnt/boot
$: mkdir /mnt/boot/EFI
$: mount /dev/part /mnt/boot/EFI

# Si SWAP
$: swapon /dev/part

# Si /home

$: mkdir /mnt/home
$: mount /dev/part /mnt/home
```

### Installation

Ici, on va installer tous les packages de base, si tu t'y connais, tu peux en ajouter dès maintenant et choisir ton editeur, shell ect.

=> Choisis l'un: amd-ucode ou intel-ucode en fonction de ton CPU.
=> Tu auras besoin d'un éditeur de texte, **Vim** et **Nano** sont standards **msedit** plus moderne et intuitif, tu peux choisir de mettre ceux que tu souhaites.

```sh
$: pacstrap -K /mnt base linux linux-firmware \
	[amd-ucode/intel-ucode] fsck \
	nano vim msedit \
	man-db man-pages texinfo
```

### Configuration

#### Commune

```sh
# Enregistre les points de montage de façon permanente a partir de ceux du moment
$: genfstab -U /mnt >> /mnt/etc/fstab

# Modifie /mnt/etc/fstab si il y a un souci

# Rentrer dans le nouveau système
$: arch-chroot /mnt

# Configurer l'heure
$: ln -sf /usr/share/zoneinfo/Europe/Paris /etc/localtime
$: hwclock --systohc

# Configuration de la locale (langue du PC)
$: locale-gen

# modifie les fichiers suivant avec vim / nano / msedit

# trouve les options avec
$: localectl list-locales
# Puis modifie
# LANG=en_US.UTF-8 Si tu veux le PC en anglais
# LANG=fr_FR.UTF-8
$: vim /etc/locale.conf



# Pour le clavier
# KEYMAP=fr
$: vim /etc/vconsole.conf



# Choix du nom du PC:
# nomdupc
$: vim /etc/hostname
```

##### Installation du boot-loader
```sh 
$: pacman -S grub efibootmgr

$: grub-install --target=x86_64-efi --efi-directory=boot/EFI --bootloader-id=GRUB --modules="tpm" --disable-shim-lock

$: grub-mkconfig -o /boot/grub/grub.cfg
```


#### Config réseau
=> NetworkManager
=> IWD
#### Création de l'utilisateur
=> user/groups/passwd
=> Root
#### Choix du bureau
=> KDE plasma
=> Hyprland


