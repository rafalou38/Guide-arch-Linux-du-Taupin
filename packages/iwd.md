Ci-dessous est présentée l'installation et la configuration basique d'IDW.

Installe d'abord iwd :
```sh
$: pacman -S iwd
```

Ensuite, active les services nécessaires :
```sh
$: systemctl enable iwd
$: systemctl enbable systemd-networkd
$: systemctl enable systemd-resolved
```

Il faut maintenant config iwd pour pouvoir se connecter à un réseau wifi, pour ethernet on le fera plus tard. On va donc créer le fichier de configuration pour iwd :
```sh
$: touch /etc/iwd/main.conf
$: vim /etc/iwd/main.conf
```
Ensuite, colle ça dans ce fichier : 
```sh
[General]
EnableNetworkConfiguration=true

[Network]
NameResolvingService=systemd
```
Enfin, restart iwd.
Pour te connecter à un WIFI :
```c
[iwd]# device list
// Remplace <name> en dessous par le nom du device (c'est souvent wlan0)
[iwd]# device <name> set-property Powered on
[iwd]# station <name> scan
[iwd]# station <name> get-networks
[iwd]# station <name> connect SSID
```
