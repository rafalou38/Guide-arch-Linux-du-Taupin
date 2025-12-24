
# Partitionnement
C'est la partie la plus chiante et qu'il ne faut surtout pas rater.

Là je présente l'option la plus simple: effacer un disque et installer linux dessus.
Tu peux aussi faire du dual boot mais c'est plus compliqué.

## Trouve quel disque formater

```sh
$: lsblk
```

Tu cherches le chemin du disque
=> `/dev/sdX` si c'est un disque dur
=> `/dev/nvme0n1` si c'est un SSD

S'il y a des `p1`, `p2`, ... c'est les partitions du disque (là où sont les données) sinon c'est que le disque est vide.

Exemple pour un système linux:
```
nvme0n1     259:0    0 931.5G  0 disk
├─nvme0n1p1 259:1    0     1G  0 part /boot/efi
├─nvme0n1p2 259:2    0    16G  0 part [SWAP]
└─nvme0n1p3 259:3    0 914.5G  0 part /
```

## Récupère tes fichiers
Si c'est déjà fait ou que tu n'en as pas, saute cette étape.

Sinon, trouve d'abord la partition où sont tes fichiers
Puis fais
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
$: umount /mnt/usb # Si tu as monté une clé et que tu l'as mis dans USB.

# Supprime aussi tes dossiers dans /mnt
# ⚠️ Vérifie qu'ils sont vides pour pas supprimer tes fichiers.
$: rm -r /mnt/part
$: rm -r /mnt/usb
```


## Formater

Il faut d'abord choisir comment tu veux le formater:
1. Format du système de fichiers.
2. Quelles partitionnement appliquer au disque.

### Format
Pour les données: EXT4 est le plus simple.

Btrfs si tu es un peu fou.
> Btrfs is a modern copy on write (COW) file system for Linux aimed at implementing advanced features while also focusing on fault tolerance, repair and easy administration.

En gros tu as des fonctionnalités en plus qui le rendent plus safe mais c'est un bordel à comprendre et un peu plus lent du coup.

### Partitions
#### Obligatoires

| Partition   | Format | Nom              | Taille                                 |
| ----------- | ------ | ---------------- | -------------------------------------- |
| `/`         | EXT4   | Racine           | Tout le reste, moins si `/home` à part |
| `/boot/EFI` | FAT32  | Fichiers de boot | 500 Mb                                 |

#### Optionnel

| Partition | Format | Nom                 |
| --------- | ------ | ------------------- |
| `/swap`   | swap   | Swap                |
| `/home`   | EXT4   | Dossier utilisateur |

##### /swap
Le **Swap** est une partition où va être stocké de la RAM dans deux cas:
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

Dis toi que pour l'hibernation, tout le contenu de la RAM va être copié dans la SWAP.
Si tu as 32 ou 16 G de RAM, elle sera compressée donc tu peux mettre un peu moins.


##### /home
Le **dossier utilisateur** permet de séparer les fichiers système des fichiers utilisateur. Cette partition sert surtout si tu as un problème avec le système ou que tu veux changer de distro: la partition `/home` peut rester intacte.

Pour la **taille**,
Si vous avez un `/home`

pour `/`, prévoir **25Gb-30Gb**, plus si vous prévoyez d'installer **beaucoup** d'applications.
Pas besoin de plus de **80Gb** (c'est ce que j'ai).

Tout le reste peut partir dans `/home`.


## Partitionnement
Ça y est tu as choisi ce que tu allais faire, c'est le moment.

```sh
$: fdisk /dev/sdX
```
Choix du type de table de partiton: GPT ou MBR
**GPT** est plus moderne et doit être choisi dans la plupart des cas.
Tapper `g`.
`o` pour MBR.

Pour créer un partition, tappe `n`
S'il demande le type de partiton: primary `p` pour toutes.
Laisse le numéro par défaut.

Pour chaque partition il demande la position du début de la partition et de la fin. Laisser le début par défaut et pour la fin utiliser +4G pour une partition de 4 Go (+taille{M,G,T,P}).

### Types de partitions
Utilise `t` pour changner le type de chaque partition, affiche les avec `L`.
Cherche le type qui te semble le plus adapté':
`uefi` pour ESP (partiton boot)
`home` pour la partition home
`swap` pour la swap
et `linux` pour les autres.

Si système EFI (standard sur les PC récents) la partition boot doit être du type `EFI system`

Sinon `BIOS boot` pour les systèmes BIOS avec GPT.

Enfin, affiche les changements avec `p`, annule avec `q` ou enregistre avec `w`.


## Formatage
```sh
# Avec bien sûr les bons ids de partition et de disque
$: mkfs.ext4 /dev/partition_root
$: mkfs.ext4 /dev/partition_home
$: mkfs.fat -F 32 /dev/partition_boot
# La partition swap n'a pas besoin d'etre formatée
```
___
[Étape suivante : Chroot](./04-chroot.md).
