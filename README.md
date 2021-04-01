# ProjetInfra
Voici le setup global fonctionnel de tout le projet, même ceux que je n'ai pas pu lancer (comme le freeze de kodi et les modifs qui s'avéraient inutiles). 
Mais cette docs permet d'arriver exactement au point ou j'en étais le jour du rendu.
## Partie 1 : Kodi le maudit
Setup de la VM avec un ISO Unbuntu 20.04 64-bits, installation standard 10Go de stockage minimum.

Installation de Kodi
```
sudo apt-get install software-properties-common
```
```
sudo add-apt-repository ppa:team-xbmc/ppa
```
```
sudo apt-get update
```
```
sudo apt-get install kodi
```
A ce moment il était impossible de lancer kodi malgré les pseudo configs trouvées sur le net, donc je suis passé sur un serveur Apache.

## Partie 2 : Apache
Installation d'apache, php, mysql, php myAdmin.
Update avant tout 
```
sudo apt-get update
```
Installation Apache
```
sudo apt-get install apache
```
Configuration du Pare-feu
```
sudo ufw allow 'Apache'
```
Vérification
```
sudo ufw status
```
Nous avons donc ça en retour:
```
Status: active
```
Vérification que le serveur tourne
```
sudo systemctl status apache2

apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor prese>
     Active: active (running)

```
(edit) A partir de là je n'ai plus accès à ma VM après la Maj windows, donc ce n'est plus du copier coller de la vm, mais les commandes éssentielles que j'ai enregistré.

Installer Php
```
sudo apt-get install php
```
MySql
```
sudo apt-get install Mysql
```
PhpMyadmin
```
sudo apt-get install phpmyadmin
```
J'ai sauté toute la partie inutile de configuration de /etc/init.d/interfaces qui s'avérait inutile.
## Partie 3 : Configurer l'ip Statique
Vérifier que l'on ai cette réponse
```
cat /etc/cloud/cloud.cfg.d/subiquity-disable-cloudinit-networking.cfg
network: {config: disabled}
```
Modifier le fichier config
```
sudo vi /etc/netplan/00-installer-config.yaml
```
Et le remplir comme cela
```
network:
  ethernets:
    enp0s3:
      addresses: [192.168.1.3/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [4.2.2.2, 8.8.8.8]
  version: 2
```

Rendre les changement éffectifs
```
sudo netplan apply
```
Pour vérifier l'ip
```
ip add show
ip route show
```
Assigner l'ip à Apache
```
Listen 192.168.1.1:80
```
Sauf que Listen n'est pas installé, donc on lance
```
sudo apt-get install listen
```
Malheureusement l'installation bloque à 0% je n'ai donc pas pu tester











