# ArchLinux

Wiki installation ArchLinux

<h4> TODO <h4>

- [ ] Faire la mise en forme

- [ ] Taille totale de l'installation -> taille recommande

- [ ] Chasse au fautes d'ortographes

- [ ] Tester le wiki dans une VM ou UEFI


<h2> Configuration du clavier <h2>

Votre clavier sera par default en US, pour afficher tout les clavier possible executez :`# ls /usr/share/kbd/keymaps/**/*.map.gz `

Pour modifier la configuration du clavier : `# loadkeys $NOM_DU_LAYOUT`

<h2> Verifier le mode de boot <h2>

Ce tutoriel est pour le boot mode UEFi, pour verifier si vous etes bien en UEFI : `# ls /sys/firmware/efi/efivars`

Si le dossier est affiche sans erreur alors vous etes bien en UEFI.

<h2> Connection a internet <h2>

Pour etre sur que votre interface reseau est activee : `# ip link`

Pour la connection sans fil veuillez utiliser : `# iwctl`

Si tout vas bien vous devriez voir apparaitre `[iwd]#` a l'ecran.

Pour connaitre le nom de votre appareil sans fil : `[iwd]# device list`

Maintenant il faut scanner les reseaux disponibles : `[iwd]# station $DEVICE scan`

Les afficher a l'ecran : `[iwd]# station $DEVICE get-networks`

Pour vous connectez a votre reseau sans fil : `[iwd]# station device connect $NOM_DU_RESEAU`

Si un mot de passe est necessaire il vous sera demande de le taper.

Pour sortir taper `[iwd]# exit`

Pour verifier votre connection executer la commande : `# ping 8.8.8.8`

Aucune erreur ? Bravo vous etes desormais connecte a l'internet !

<h2> Mise a jour de l'horloge <h2>

Pour s'assurer que l'horloge interne est a jour : `# timedatectl set-ntp true`

<h2> Partitionner les disques <h2>

Cette partie s'applique au espace de stockage completement vide.

Pour afficher chaque disques et ses partitionns : `# fdisk -l`

Pour commencer la partition du disque executez : `# fdisk /dev/$LE_DISQUE_A_PARTITIONNER`

!!! Attention choisissez bien votre disque, et suivez bien les instructions sous peine de voir votre disque totalement efface !!!

<h4> Partition EFI <h4>

Nous allons commencer par creer la partition EFI pour cela tapez `n` puis entrer.

il s'affiche `Numero de la partition` ... tapez entrer.

il s'affiche `Premier secteur`... tapez entrer.

il s'affiche `Dernier secteur`... tapez `+550M`

Ensuite tapez `t`, entrer, puis L. Vous devriez observer une liste, trouvez `EFI filesystem` et tappez `q` entrer, puis son numero.

<h4> Partition Racine <h4>

Maintenant nous allons creer la partition Racine, celle ou tout vos fichier se retrouverons pour cela tapez `n` puis entrer.

il s'affiche `Numero de la partition` ... tapez entrer.

il s'affiche `Premier secteur`... tapez entrer.

il s'affiche `Dernier secteur`... , La taille minimum requise et de ??? tapez donc une valeur superieure ou egale, `+10G`

<h4> Formatter les partitions <h4>

Formattage partition EFI : `# mkfs.fat -F 32 /dev/$VOTRE_PARTITION_EFI`

Formattage partition Racine : `# mkfs.ext4 /dev/$VOTRE_PARTITION_RACINE`

<h2> Monter la partition racine <h2>

Pour continuer il vous faut monter votre partition racine : `# mount /dev/$VOTRE_PARTITION_RACINE /mnt`

<h2> Installation des paquet esssentiel <h2>

L'installation des paquet essentiel ce fait via pacstrap : `# pacstrap /mnt base linux linux-firmware`

Nous installerons le reste des paquets dont vous avez besoin plus tard.

<h2> Configuration de votre sysrteme <h2>

Generer un fichier "fstab"  : `# genfstab -U /mnt >> ?mnt/etc/fstbab`

Root dans votre nouveau systeme : `# arch-chroot /mnt`

Changer votre fuseau horaire : `# ln -sf /usr/share/zoneinfo/$REGION/$VILLE /etc/localtime`

Si vous etes en france la commade sera : `# ln -sf /usr/share/zoneinfo/Europe/Paris /etc/localtime`

Generer le fichier /etc/adjtime : `hwclock --systohc`

Editer le fichier /etc/locale.gen : `nano /etc/locale.gen`

Trouvez la ligne fr_FR.UTF-8 UTF-8, supprimer le `#` en debut de ligne puis faites ctrl+S et ctrl+X pour sauvegarder et sauver les modifications.

Puis executer : `# locale-gen` pour generer les changements.

Creer le fichier /etc/locale.conf : `# nano /etc/locale.conf`

Puis ecrire : `LANG=fr_FR.UTF-8` et faites ctrl+S et ctrl+X comme precedemment.

Creer le fichier /etc/vconsole.conf : `# nano /etc/vconsole.conf`

Puis ecrire : `KEYMAP=$NOM_DU_LAYOUT` ou "$NOM_DU_LAYOUT" et le layout choisis au debut de l'installation et faites ctrl+S et ctrl+X comme precedemment.

Creer le fichier /etc/hostname : `# nano /etc/hostname`

Puis ecrire le nom que votre machine portera sur cet OS, vous pouvez mettre ce que vous voulez alors lachez-vous !

<h2> Configurer les utilisateurs <h2>

Changer le mot de passe root, ATTENTION AVEC CE MOT DE PASSE VOUS AVEZ LA MAIN SUR VOTRE SYSTEM ENTIER, NE LE PARTAGER PAS ET CHOISISSEZ UN MDP FORT.

Recommandation mots de passe : https://www.economie.gouv.fr/particuliers/creer-mot-passe-securise

Commande : `# passwd`

<h4> Creation de votre utilisateur <h4>

Pour creer votre utilisateur : `# useradd -mG wheel,video,audio,optical,storage $VOTRE_NOM_D'UTILISATEUR`

Changer votre mots de passe aussi avec : `# passwd $VOTRE_NOM_D'UTILISATEUR`

<h4> Ajout au groupe wheel <h4>

Il faut editer le fichier sudoers : `# EDITOR=nano visudo`

Parcourez le fichier jusqu'a trouver la ligne : `# %wheel ALL=(ALL) ALL` et supprimez le `#` en debut de ligne puis faites ctrl+S et ctrl+X.

<h2> Installation des paquets <h2>

Mise a jour des paquets : `# pacman -Syu`

Installation des paquets indispensable : `# pacman -S sudo grub networkmanager `

Installation de i3 : `# pacman -S i3-gaps i3blocks i3lock i3status`

Installation de lightdm : `# pacman -S lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings`

Lire les pages de man : `# pacman -S man-db texinfo`

Compiler un programme C : `# pacman -S gcc UN_AUTRE!!!` !!!!!!!!!!!!!!!!!!!!!!!

Lancer un make : `# pacman -S make`




















