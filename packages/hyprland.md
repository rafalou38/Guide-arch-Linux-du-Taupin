Ci-dessous est présentée l'installation d'Hyrpland et une configuration basique.

Tout d'abord, installe Hyprland :
```sh
$ : pacman -S hyprland
```

Ensuite, crée le dossier de configuration puis copie la configuration par défaut dans ce dossier :
```sh
$ : mkdir -p ~/.config/hypr
$ : cp /usr/share/hypr/hyprland.lua ~/.config/hypr/
```

Tu peux maintenant modifier ce fichier a ta guise pour avoir une installation fonctionnelle. Voici quelques conseils :
- dans la catégorie input, change la variable `kb_layout` en fr si tu utilises un clavier azerty.
- dans les définitions des keybinds pour changer de bureaux, les chiffres ne sont pas valides pour les claviers français, il faut créer une liste avec les noms français (`eacute` etc) et lui faire appeler la liste au lieu de prendre les nombres de 1 à 9.

Plus généralement, toutes les options se trouvent [ici](https://wiki.hypr.land/Configuring). 
