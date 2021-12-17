
| Besoin                                                                             | Nombre d'outils différents demandés | Logiciels proposés |
| ---------------------------------------------------------------------------------- | :----------------------------------:| -------------------|
| Avoir des Gestionnaires de bureau/fenêtre (dont un qui vous plaise)                | 4                                   |                    |
| Avoir deux utilisateurs: vous et "stagiaire"                                       |                                     |                    |
| Compiler un programme C                                                            | 2                                   |                    |
| Regarder les pages de man de la libC                                               | 2                                   |                    |
| Lancer un `make`                                                                   |                                     |                    |
| Éditer du code source                                                              | 4                                   |                    |
| Déboguer du code                                                                   | 2                                   |                    |
| Naviguer sur le Web                                                                | 2                                   |                    |
| Naviguer sur le Web avec la version de firefox dans backports                      | 1                                   |                    |
| Éditer une image matricielle (png...)                                              | 2                                   |                    |
| Éditer une image vectorielle (svg)                                                 | 1                                   |                    |
| (Dé)Compresser les formats `targz` et `7z` et `rar`                                | 1                                   |                    |

## Points indispensables à maîtriser

Cette liste n'est pas exhaustive.

### Gestionnaire de paquets

* Installer un logiciel : `$ sudo pacman -S PACKAGE_NAME`
* Désinstaller un logiciel : `$ sudo pacman -R PACKAGE_NAME`
* Faire une recherche sur les paquets disponibles : `$ pacman -Ss PACKAGE_NAME`
* Faire une mise à jour : `$ sudo pacman -Syu`
* Lister les fichiers installés par un paquet : `$ pacman -Ql PACKAGE_NAME`
* Rechercher quel paquet contient un fichier donné : `$ pacman -Ql | grep NOM_DU_FICHIER`
  * Idem si le paquet n'est pas installé : `$ pacman -Fy NOM_DU_FICHIER`

### Réseau

* Pouvoir se connecter sur une autre machine avec `ssh` : ssh USER@DOMAIN
* Permettre à une autre machine de se connecter sur la vôtre.
* Installer un serveur web capable de lire vos pages perso (`userdir`).

### Sauvegardes

* Faire une archive d'un répertoire (et de ses sous répertoires) : `$ tar cvf NOM_DE_L'ARCHIVE.tar CHEMIN_DU_REPERTOIRE_A_ARCHIVER`
* Copier l'archive sur une clé USB : `$ sudo mount /dev/sdb /mnt`

`$ sudo cp CHEMIN_DE_L'ARCHIVE /mnt`

`$ sudo umount /mnt`
* Copier l'archive via `scp` (vous pouvez vous entraîner sur `localhost`).

### Services (voir `systemd` ou autre selon OS)

* Savoir démarrer/stopper un service : `$ sudo systemctl start NOM_DU_SERVICE`

`$ sudo systemctl stop NOM_DU_SERVICE`
* Savoir en vérifier l'état : `$ pacman sudo systemctl status NOM_DU_SERVICE`

* Savoir afficher les messages d'erreur.

### Divers

* Comment faire pour que le *stagiaire* n'ait pas accès aux données de votre compte ?

`$ chmod -R 711 ~/` 

### Bonus

* Écrire un service utilisateur qui se lance dès la connexion de l'utilisateur.
* Écrire un service qui se lance périodiquement.
* Installer un logiciel propriétaire en restreignant ses droits d'accès à votre environnement.

