Ci-dessous est présentée l'installation d'Hyrpland et une configuration basique.

Tout d'abord, installe Hyprland :
```sh
$ : pacman -S hyprland
```

Ensuite, crée le dossier de configuration puis copie la configuration par défaut dans ce dossier :
```sh
$ : mkdir -p ~/.config/hypr
$ : cp /usr/share/hypr/hyprland.conf ~/.config/hypr/
```

Tu peux maintenant modifier ce fichier a ta guise pour avoir une installation fonctionnelle. Voici quelques conseils :
- dans la catégorie input, change la variable `kb_layout` en fr si tu utilises un clavier azerty.
- dans les définitions des keybinds pour changer de bureaux, les chiffres ne sont pas valides pour les claviers français, il faut remplacer 1 par `ampersand`, 2 par `eacute`...

Plus généralement, toutes les options sont trouvables [ici](https://wiki.hypr.land/Configuring). 
