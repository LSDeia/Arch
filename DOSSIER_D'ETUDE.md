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

## Choix des outils

#### Gestionnaire bureau / besoin

i3 : gestionnaire de fenetre rapide, leger et utilisable entierement au clavier (sauf Navigateur Web).

xfce : gestionnaire de bureau rapide, leger, esthetique et pratique au quotidien.

gnome : gestionnaire de bureau tres beau, facile a prendre en main et pratique au quotidien.

ratpoison : gestionnaire de fenetre ultraleger et rapide, ne propose que le strict minimum. Rends les machines tres peu puissante utilisables.

#### Compiler un programme c

gcc : compilateur gnu performant et plein d'option.

tcc : compilateur minimaliste, moins d'option mais beaucoup plus rapide.

#### Regarder les pages de man de la libc :

man-db : facon traditionnelle de lire les pages de man

texinfo : programme GNU pour lire les pages de man

#### Lancer un make :

make  : programme gnu pour generer des executables

#### Editer du code source

vim : rapide, pas de gui et beaucoup de raccourci tres interresant.

nano : programme gnu, pas de gui non plus mais plus user-friendly que vim.

code : editeur de code de microsoft, belle gui et plein d'extensions tres pratique.

emacs programme gnu, avec une gui tres customisable.

#### Deboguer du code source

gdb : debugger gnu.

ghidra : outils de reverse engineering developpe par la NSA, contient aussi un debugger.

#### Naviguer sur le web

firefox : navigateur rapide, moins gourmands que chrome et tres customisable

lynx : navigateur textuel, adapte au synthetiseurs vocaux.

chromium : projet open-source de google qui sert de base a de nombreux navigateur

#### Editer une image matricielle

imagemagick : programme avec gui peu pratique mais tres interresant a uiliser dans le terminal.

gimp : programme gnu, gui complete requiert un peu d'experience

#### Editer une image vectorielle

inkscape : programme professionel d'edition vectorielle

#### (Dé)Compresser les archives `tar.gz`, `7z`, `rar`

p7zip : peut compresser et decompresser de nombreux format d'archive diffferent.

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

Editer la configuration de ssh : `# nano /etc/ssh/sshd_config` trouvez la ligne `#Port 22` et supprimez le diese, puis ctrl+X ctrl+S.

Creation des cles ssh : `$ ssh-keygen`, dans votre dossier ~/.ssh vous trouverez le fichier `id_rsa.pub` il faut copier son contenu dans le fichier ~/.ssh/authorized_keys de la machine a laquelle vous souhaiter vous connecter.

Connexion : `$ ssh USER@DOMAIN` ou USER est le nom d'un utilisateurs de la machine a laquelle vous voulez vous connecter et DOMAIN est l'adresse ip ou le nom de domaine de la machine a laquelle vous voulez vous connecter.

##### Permettre à une autre machine de se connecter sur la vôtre :

Editer la configuration de ssh : `$ sudo nano /etc/ssh/sshd_config` trouvez la ligne `#Port 22` et supprimez le diese, puis ctrl+X ctrl+S.

##### Installer un serveur web capable de lire vos pages perso (`userdir`) :

Creation du repertoire ~/public_html : `$ mkdir ~/public_html`

Changer les droits sur votre home recursivement : `$ chmod -R 755 ~/`

Ouvrir un navigateur est saisir l'url : `localhost/~USERNAME` ou use4rname seras par exemple : stagiaire.

## Sauvegardes

Faire une archive d'un répertoire (et de ses sous répertoires) : `$ tar cvf NOM_DE_L'ARCHIVE.tar CHEMIN_DU_REPERTOIRE_A_ARCHIVER`

*Copier l'archive sur une clé USB :* 

Montage de la cle usb : `$ sudo mount /dev/sdb /mnt`

Copiage de l'archive : `$ sudo cp CHEMIN_DE_L'ARCHIVE /mnt`

Demontage de la cle usb : `$ sudo umount /mnt`

Copier l'archive via `scp` (vous pouvez vous entraîner sur `localhost`) : `$ scp CHEMIN_DE_L'ARCHIVE USER@DOMAIN:CHEMIN_D'ARRIVE`

## Services (voir `systemd` ou autre selon OS)

Savoir démarrer/stopper un service : `$ sudo systemctl start NOM_DU_SERVICE` / `$ sudo systemctl stop NOM_DU_SERVICE`

Savoir en vérifier l'état : `$ sudo systemctl status NOM_DU_SERVICE`

Savoir afficher les messages d'erreur : `$ sudo journalctl -u NOM_DU_SERVICE | grep failed`

## Divers

Comment faire pour que le stagiaire n'ait pas accès aux données de votre compte ?

`$ chmod -R 711 ~/` 

