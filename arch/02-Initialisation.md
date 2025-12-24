# Pré-configuration
Ça y est tu es sur Arch, dans un environnement temporaire dit "live".
Tu est **root** donc pas besoin de `sudo`.
Avant de continuer il faut configurer le clavier, l'internet et l'heure.

## Clavier
Si tu as un clavier qwerty c'est bon, sinon tape la commande :
```sh
$: loadkeys fr
```


## Connexion internet
Plusieurs options s'offrent à toi:
1. Tu branches un cable Ethernet
2. Tu connectes ton tel et tu fais un partage de connexion par USB

Sinon, **bienvenue en enfer**:
<details>

<summary>(connection via WIFI)</summary>

### WIFI (iwctl)
Tu dois utiliser l'outil `iwctl`
```sh
$: iwctl
```
#### Option manuelle
```c
[iwd]# device list
// Remplace <name> par le nom du device (c'est souvent wlan0)
[iwd]# device <name> set-property Powered on
[iwd]# station <name> scan
[iwd]# station <name> get-networks
[iwd]# station <name> connect SSID
```
#### Pin WPS
```c
[iwd]# wsc list
[iwd]# wsc device push-button
```
</details>




### Vérifie si ça marche
```
ping ping.archlinux.org
```

<details>

<summary>ça ne marche pas</summary>

#### Si ça ne marche pas
```
ip link
```
regarde si ta carte réseau est UP 
si ce n'est pas le cas
```
$: rfkill
ID TYPE      DEVICE      SOFT      HARD
 0 bluetooth hci0   unblocked unblocked
 1 wlan      phy0   unblocked unblocked
 
$: rfkill unblock wlan
```

Puis 
```
ip a
```
Pour voir si tu as une IP (sous la forme `192.168.1.X` si tu es sur un réseau normal et un truc chelou sinon).

Si tu en as une et pas de connexion c'est le réseau le problème, sinon recommence.

</details>


## Date et heure

Pense à configurer l'heure, remplace tz par ta timezone. Par exemple avec Europe/Paris si tu es en France.
```sh
$: timedatectl set-timezone tz
```




[Suite: Partitionnement](./03-partitionnement.md).
