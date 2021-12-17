# Notice Installation ArchLinux

![haha](arch-icon.png)

❗ **INFORMATION IMPORTANTE** : Chaque suite de caracteres en MAJUSCULE est une variable que vous devez modifier vous meme.

❗ **Exemple** : ` # echo MON_PRENOM` devient ` # echo Remi`

## 1. Configuration du clavier ⌨

Votre clavier sera par default en US, pour afficher tout les clavier possible executez :`# ls /usr/share/kbd/keymaps/**/*.map.gz `

Pour modifier la configuration du clavier : `# loadkeys NOM_DU_LAYOUT`

## 2. Verifier le mode de boot ✅

Ce tutoriel est pour le boot mode UEFi, pour verifier si vous etes bien en UEFI : `# ls /sys/firmware/efi/efivars`

Si le dossier est affiche sans erreur alors vous etes bien en UEFI.

## 3. Connection a internet 📶

Pour etre sur que votre interface reseau est activee : `# ip link`

Pour la connection sans fil veuillez utiliser : `# iwctl`

Si tout vas bien vous devriez voir apparaitre `[iwd]#` a l'ecran.

Pour connaitre le nom de votre appareil sans fil : `[iwd]# device list`

Maintenant il faut scanner les reseaux disponibles : `[iwd]# station DEVICE scan`

Les afficher a l'ecran : `[iwd]# station DEVICE get-networks`

Pour vous connectez a votre reseau sans fil : `[iwd]# station device connect NOM_DU_RESEAU`

Si un mot de passe est necessaire il vous sera demande de le taper.

Pour sortir taper `[iwd]# exit`

Pour verifier votre connection executer la commande : `# ping 8.8.8.8`

Aucune erreur ? Bravo vous etes desormais connecte a l'internet !

## 4. Mise a jour de l'horloge 🕑

Pour s'assurer que l'horloge interne est a jour : `# timedatectl set-ntp true`

## 5. Partitionner les disques 💾

Cette partie s'applique au espace de stockage completement vide.

Pour afficher chaque disques et ses partitionns : `# fdisk -l`

Pour commencer la partition du disque executez : `# fdisk /dev/LE_DISQUE_A_PARTITIONNER`

!!! Attention choisissez bien votre disque, et suivez bien les instructions sous peine de voir votre disque totalement efface !!!

#### 5.1 Partition Racine

Maintenant nous allons creer la partition Racine, celle ou tout vos fichier se retrouverons pour cela tapez `n` puis entrer.

il s'affiche `Numero de la partition` ... tapez entrer.

il s'affiche `Premier secteur`... tapez entrer.

il s'affiche `Dernier secteur`... , La taille minimum requise et de ??? tapez donc une valeur superieure ou egale, `+10G`

Formattage partition Racine : `# mkfs.ext4 /dev/VOTRE_PARTITION_RACINE`

## 6. Monter la partition racine 🔼

Pour continuer il vous faut monter votre partition racine : `# mount /dev/VOTRE_PARTITION_RACINE /mnt`

## 7. Installation des paquets esssentiels 📥

L'installation des paquet essentiel ce fait via pacstrap : `# pacstrap /mnt base linux linux-firmware`

Nous installerons le reste des paquets dont vous avez besoin plus tard.

## 8. Configuration de votre systeme

Generer un fichier "fstab"  : `# genfstab -U /mnt >> /mnt/etc/fstbab`

Root dans votre nouveau systeme : `# arch-chroot /mnt`

Changer votre fuseau horaire : `# ln -sf /usr/share/zoneinfo/REGION/VILLE /etc/localtime`

    Si vous etes en france la commade sera : `# ln -sf /usr/share/zoneinfo/Europe/Paris /etc/localtime`

Generer le fichier /etc/adjtime : `hwclock --systohc`

Editer le fichier /etc/locale.gen : `nano /etc/locale.gen`

Trouvez la ligne fr_FR.UTF-8 UTF-8, supprimer le `#` en debut de ligne puis faites ctrl+S et ctrl+X pour sauvegarder et sauver les modifications.

Puis executer : `# locale-gen` pour generer les changements.

Creer le fichier /etc/locale.conf : `# nano /etc/locale.conf`

Puis ecrire : `LANG=fr_FR.UTF-8` et faites ctrl+S et ctrl+X comme precedemment.

Creer le fichier /etc/vconsole.conf : `# nano /etc/vconsole.conf`

Puis ecrire : `KEYMAP=NOM_DU_LAYOUT` ou "$NOM_DU_LAYOUT" et le layout choisis au debut de l'installation et faites ctrl+S et ctrl+X comme precedemment.

Creer le fichier /etc/hostname : `# nano /etc/hostname`

Puis ecrire le nom que votre machine portera sur cet OS, vous pouvez mettre ce que vous voulez alors lachez-vous !

## 9. Configurer les utilisateurs 👤

❗ **CHANGER LE MOT DE PASSE ROOT : ATTENTION AVEC CE MOT DE PASSE VOUS AVEZ LA MAIN SUR VOTRE SYSTEM ENTIER, NE LE PARTAGEZ PAS ET CHOISISSEZ UN MDP FORT.** ❗

Recommandation mots de passe : https://www.economie.gouv.fr/particuliers/creer-mot-passe-securise

Commande : `# passwd`

#### 9.1 Creation de votre utilisateur

Pour creer votre utilisateur : `# useradd -mG wheel,video,audio,optical,storage VOTRE_NOM_D'UTILISATEUR`

Changer votre mot de passe aussi avec : `# passwd VOTRE_NOM_D'UTILISATEUR`

#### 9.2 Creer l'utilisateur stagiaire

Pour creer le stagiaire : `# useradd -mG Stagiaire`

Changer le mot de passe aussi avec : `# passwd Stagiaire`

#### 9.3 Configuration du groupe wheel

Installation de sudo : ` # pacman -S sudo`

Il faut editer le fichier sudoers : `# EDITOR=nano visudo` Ici EDITOR n'est pas une variable

Parcourez le fichier jusqu'a trouver la ligne : `# %wheel ALL=(ALL) ALL` et supprimez le `#` en debut de ligne puis faites ctrl+S et ctrl+X.

## 10. Installation du grub

Installation des paquets requis : ` # pacman -S grub efibootmgr dosfstools os-prober mtools`

Creation du repertoire /boot/EFI : ` # mkdir /boot/EFI`

Monter la partition EFI dans /boot/EFI : ` # mount /dev/LA_PARTITION_EFI /boot/EFI`

Installer le grub : ` # grub-install --target=x86_64-efi --bootloader-id=archlinux_grub --recheck`

Creer le fichier de configurration du grub : `# grub-mkconfig -o /boot/grub/grub.cfg`

## 11. Installation des paquets 📥

Mise a jour des paquets : `# pacman -Syu`

Installation des paquets indispensable : `# pacman -S networkmanager man-pages`

Installer ssh : `# pacman -S openssh`

Installer apache : ` # pacman -S apache`

Installation de lightdm : `# pacman -S lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings`

Gestionnaire de Bureau / Fenetre : `# pacman -S i3-gaps i3blocks i3lock i3status xfce4 gnome ratpoison`

Compiler un programme C : `# pacman -S gcc tcc`

Lire les pages de man : `# pacman -S man-db man_pages texinfo`

Lancer un make : `# pacman -S make`

Editer du code source : `# pacman -S emacs vim nano code`

Debogguer du code : ` # pacman -S gdb ghidra`

Naviguer sur le web : `# pacman -S firefox chromium lynx`

INFORMATION : lynx est un navigateur base sur le textuel, il s'utilise dans le terminal.

Editer une image matricielle / vectorielle  : ` # pacman -S imagemagick inkscape gimp`

(De)compresser des fichier : ` # pacman -S p7zip`

## Lancement au demarrage

Lancer ssh au demarrage : ` # systemctl enable sshd`

Lancer apache au demarrage : ` # systemctl enable apache`

Lancer lightdm au demarrage : ` # systemctl enable lightdm`

Lancer networkmanager au demarrage : ` # systemctl enable NetworkManager`
























