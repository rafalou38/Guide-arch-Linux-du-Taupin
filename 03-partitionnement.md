
# Partitionnement
C'est la partie la plus chiante et qu'il faut surtout pas rater.

La je présente l'option la plus simple: effacer un disque et installer linux dessus.
Tu peux aussi faire du dual boot mais plus compliqué.

## Trouve quel disque formater

```sh
$: lsblk
```

Tu cherche le chemin du disque
=> `/dev/sdX` si c'est un disque dur
=> `/dev/nvme0n1` si c'est un SSD

S'il y a des `p1`, `p2`, ... c'est les partitions du disque (là où sont les données) sinon c'est que le disque est vide.

exemple pour un système linux:
```
nvme0n1     259:0    0 931.5G  0 disk
├─nvme0n1p1 259:1    0     1G  0 part /boot/efi
├─nvme0n1p2 259:2    0    16G  0 part [SWAP]
└─nvme0n1p3 259:3    0 914.5G  0 part /
```

## Récupérer tes fichiers
Si c'est déjà fait ou que t'en a pas, saute cette étape

Trouve d'abord la partition qui a tes fichiers
Puis tu fais
```sh
$: mkdir /mnt/part
# remplace partition par ta partition, nvme0n1p3 dans l'exemple juste au dessus.
$: mount /dev/partition /mnt/part
```

Maintenant tout le contenu de la partition est affiché dans `/mnt/part`.
Branche un disque, monte le de la même façon puis transfère tes fichiers avec `cp -r`.

Pense à **un-mount** avant de continuer
```sh

# ⚠️ Oui c'est bien umount et pas unmount
$: umount /mnt/part
$: umount /mnt/usb # Si t'as monté une clé et que tu l'as mis dans USB

# Supprime aussi tes dossiers dans /mnt
# ⚠️ Vérifie qu'ils sont vide pour pas suprimer tes fichiers
$: rm -r /mnt/part
$: rm -r /mnt/usb
```


## Formater

Il faut d'abord choisir comment tu veux le formater:
1. Format du système de fichiers.
2. Quelles partitionnement appliquer au disque.

### Format
Pour les données: EXT4 est le plus simple

Btrfs si t'est un peu fou.
> Btrfs is a modern copy on write (COW) file system for Linux aimed at implementing advanced features while also focusing on fault tolerance, repair and easy administration.

En gros t'as des fonctionnalités en plus qui le rendent plus safe mais c'est un bordel à comprendre et un peu plus lent du coup.

### Partitions
#### Obligatoires

| Partition   | Format | Nom              | Taille                                 |
| ----------- | ------ | ---------------- | -------------------------------------- |
| `/`         | EXT4   | Racine           | Tout le reste, moins si `/home` a part |
| `/boot/EFI` | FAT32  | Fichiers de boot | 500 Mb                                 |

#### Optionnel

| Partition | Format | Nom                 |
| --------- | ------ | ------------------- |
| `/swap`   | swap   | Swap                |
| `/home`   | EXT4   | Dossier utilisateur |

##### /swap
Le **Swap** est une partition ou va être stoqué de la RAM dans deux cas:
1. Tu mets ton PC en hibernation
	1. Manuellement
	2. Si tu fermes le capot de ton PC portable
2. S'il n'y a plus de place dans la RAM

**TLDR:** Donc il faut en mettre si:
1. Tu as un PC portable
2. Tu as pas mal de stockage.
3. Tu as moins de 8Gb de RAM

Pour sa **taille**
1. Si tu as beaucoup de place libre, égale à la RAM
2. Sinon, tu peux réduire mais pas moins de 4Gb

Dis toi que pour l'hibernation, tout le contenu de la RAM va être copié dans le SWAP.
Si tu as 32 ou 16 G de RAM, elle sera compressée donc tu peux mettre un peu moins.


##### /home
Le **dossier utilisateur** permet de séparer les fichiers systeme des fichiers utilisateur.
Sert surtout si t'a un problème avec le système ou que tu veux changer de distro: la partition `/home` peut rester intacte.

Pour la **taille**,
Si vous avez un `/home`

pour `/`, prévoir **25Gb-30Gb**, plus si vous prévoyez d'installer **beaucoup** d'applications.
Pas besoin de plus de **80Gb** (c'est ce que j'ai)

Tout le reste peut partir dans `/home`


## Partitionnement
ça y est t'a choisi ce que t'allait faire, c'est le moment.

```sh
$: fdisk /dev/sdX
```

TODO

## Formatage
```sh
# Avec bien sur les bons ids de partition et de disque
$: mkfs.ext4 /dev/partition_root
$: mkfs.ext4 /dev/partition_home
$: mkfs.fat -F 32 /dev/partition_boot
```
___
[Étape suivante : Chroot](./04-chroot.md)