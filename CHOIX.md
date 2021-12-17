# Dossier d'etude

## Tableau des besoins et solutions

| Besoins                                                                             | Nombre d'outils différents | Logiciels proposés |
| ---------------------------------------------------------------------------------- | :-------------------------:| :-----------------:|
| Avoir des Gestionnaires de bureau/fenêtre (dont un qui vous plaise)                | 4                          | i3 xfce gnome ratpoison |
| Compiler un programme C                                                            | 2                          | gcc tcc            |
| Regarder les pages de man de la libC                                               | 2                          | man-db texinfo     |
| Lancer un `make`                                                                   | 1                          | make               |
| Éditer du code source                                                              | 4                          | vim nano code emacs |
| Déboguer du code                                                                   | 2                          | gdb ghidra         |
| Naviguer sur le Web                                                                | 2                          | firefox chromium lynx |
| Éditer une image matricielle (png...)                                              | 2                          | imagemagick gimp   |
| Éditer une image vectorielle (svg)                                                 | 1                          | Inkscape           |
| (Dé)Compresser les formats `targz` et `7z` et `rar`                                | 1                          | p7zip              |

## Gestionnaire de paquets

Installer un logiciel : `$ sudo pacman -S PACKAGE_NAME`

Désinstaller un logiciel : `$ sudo pacman -R PACKAGE_NAME`

Faire une recherche sur les paquets disponibles : `$ pacman -Ss PACKAGE_NAME`

Faire une mise à jour : `$ sudo pacman -Syu`

Lister les fichiers installés par un paquet : `$ pacman -Ql PACKAGE_NAME`

Rechercher quel paquet contient un fichier donné : `$ pacman -Ql | grep NOM_DU_FICHIER`

Idem si le paquet n'est pas installé : `$ pacman -Fy NOM_DU_FICHIER`


## Réseau

##### Pouvoir se connecter sur une autre machine avec `ssh` :

Creation des cles ssh : `$ ssh-keygen`, dans votre dossier ~/.ssh vous trouverez le fichier `id_rsa.pub` il faut copier son contenu dans le fichier ~/.ssh/authorized_keys de la machine a laquelle vous souhaiter vous connecter.

Connexion : `$ ssh USER@DOMAIN` ou USER est le nom d'un utilisateurs de la machine a laquelle vous voulez vous connecter et DOMAIN est l'adresse ip ou le nom de domaine de la machine a laquelle vous voulez vous connecter.

##### Permettre à une autre machine de se connecter sur la vôtre :

Editer la configuration de ssh : `$ sudo nano /etc/ssh/sshd_config` trouvez la ligne `#Port 22` et supprimez le diese, puis ctrl+X ctrl+S.

##### Installer un serveur web capable de lire vos pages perso (`userdir`) :

Creation du repertoire ~/public_html : `$ mkdir ~/public_html`

## Sauvegardes

Faire une archive d'un répertoire (et de ses sous répertoires) : `$ tar cvf NOM_DE_L'ARCHIVE.tar CHEMIN_DU_REPERTOIRE_A_ARCHIVER`

*Copier l'archive sur une clé USB :* 

Montage de la cle usb : `$ sudo mount /dev/sdb /mnt`

Copiage de l'archive : `$ sudo cp CHEMIN_DE_L'ARCHIVE /mnt`

Demontage de la cle usb : `$ sudo umount /mnt`

Copier l'archive via `scp` (vous pouvez vous entraîner sur `localhost`).

## Services (voir `systemd` ou autre selon OS)

Savoir démarrer/stopper un service : `$ sudo systemctl start NOM_DU_SERVICE` / `$ sudo systemctl stop NOM_DU_SERVICE`

Savoir en vérifier l'état : `$ sudo systemctl status NOM_DU_SERVICE`

Savoir afficher les messages d'erreur.

## Divers

Comment faire pour que le *stagiaire* n'ait pas accès aux données de votre compte ?

`$ chmod -R 711 ~/` 

## Bonus

* Écrire un service utilisateur qui se lance dès la connexion de l'utilisateur.
* Écrire un service qui se lance périodiquement.
* Installer un logiciel propriétaire en restreignant ses droits d'accès à votre environnement.

