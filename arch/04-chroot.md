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
$: mkswap /dev/part
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
	[amd-ucode/intel-ucode] \
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
Il est recommandé d'utiliser GRUB en tant que boot load, c'est en tout cas l'installation qui est présentée ici.
```sh 
$: pacman -S grub efibootmgr

$: grub-install --target=x86_64-efi --efi-directory=boot/EFI --bootloader-id=GRUB 

$: grub-mkconfig -o /boot/grub/grub.cfg
```

#### Configuration réseau
Pour la configuration réseau il y a plusieurs options : 

- NetworkManager avec un GUI pour se connecter aux réseaux -> guide [ici](https://github.com/rafalou38/Guide-arch-Linux-du-Taupin/blob/main/packages/networkManager.md)
- IWD avec son interface en ligne de commande `iwctl`, plus compliqué à utiliser -> guide [ici](https://github.com/rafalou38/Guide-arch-Linux-du-Taupin/blob/main/packages/iwd.md)
- En complément d'IWD, il est conseillé d'utiliser un des packages systemd pour la connexion ethernet, le guide est disponible [ici](https://github.com/rafalou38/Guide-arch-Linux-du-Taupin/blob/main/packages/systemd-network.md)

#### Création de l'utilisateur
Utilise la commande ```passwd``` pour créer le mot de passe de l'utilisateur root. 

Crées toi ensuite un utilisateur via cette commande :
```sh
$: useradd -m nom-de-l-utilisateur
```
#### Choix du bureau
Il est possible de complètement customiser son bureau sur Arch, je présente ici deux options parmis les plus populaires mais il existe aussi d'autres packages tels que [xfce](https://wiki.archlinux.org/title/Xfce) ou encore [xmonad](https://wiki.archlinux.org/title/Xmonad). 

Je conseille personnellement KDE Plasma, simple à utiliser, un guide d'installation est disponible [ici](https://github.com/rafalou38/Guide-arch-Linux-du-Taupin/blob/main/packages/kde-plasma.md). Si tu préfères un Desktop basé sur le tiling, je conseille Hyprland, pour lequel un guide est disponible [ici](https://github.com/rafalou38/Guide-arch-Linux-du-Taupin/blob/main/packages/hyprland.md).
