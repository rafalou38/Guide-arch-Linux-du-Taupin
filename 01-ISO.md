# L'ISO

## Téléchargement

Torrent (t'est un bon):
```
magnet:?xt=urn:btih:7920acc28f0ec4d75ac0e0edd82789092cf8ded1&dn=archlinux-2025.09.01-x86_64.iso
```

Téléchargement http (prendre le fichier x86_64.iso): 
```
https://archlinux.mirrors.ovh.net/archlinux/iso/latest/
```

Si t'est parano ou que t'as une co de merde [vérifie la signature](https://wiki.archlinux.org/title/Installation_guide#Verify_signature) de l'iso.

## Burn

Sur **Windows** utilise [balena etcher](https://etcher.balena.io/) ou [rufus](https://rufus.ie/)

Si t'est sur **Linux** trouve l'id de ta clé et vérifie qu'elle est pas monté
```sh
$: lsblk
```
Et puis
```sh
$: sudo cp ~/Downloads/arch.iso /dev/sd[id de la clé]
```
___
Enfin tu branches la clé dans le pc et redémare.

Pendant qu'il s'allume spam les touches `Echap`, `Supr`, `F11`, `F12` (ça dépend du pc, cherche ton modèle pour être sur ou vas y au talent)

Soit tu te retrouve dans le **BIOS** 
- Regarde si secure boot est activé, si c'est le cas désactive le
- Regarde si y a moyen de boot sur ta clé depuis ici
	- Changer l'ordre de boot
	- Booter sur la clé directement
Soit tu te retrouve dans le **menu de boot** avec
1. Windows boot manager, ou la distro que t'avais
2. UEFI Device Arch...
Tu choisi bien sur Arch et si ça marche pas faut désactiver secure boot dans le BIOS

[Suite](./02-Initialisation.md)
