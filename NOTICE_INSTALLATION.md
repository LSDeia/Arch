# Notice Installation ArchLinux

![haha](arch-icon.png)

❗ **INFORMATION IMPORTANTE** : Chaque suite de caractères en MAJUSCULE est une variable que vous devez modifier vous même.

❗ **Exemple** : ` # echo MON_PRENOM` devient ` # echo Rémi`

## 1. Configuration du clavier ⌨

Votre clavier sera par default en uk, pour afficher tout les clavier possible exécutez :`# ls /usr/share/kbd/keymaps/**/*.map.gz `

Pour modifier la configuration du clavier : `# loadkeys NOM_DU_LAYOUT`

    Clavier francais : `# loadkeys fr`

## 2. Vérifier le mode de boot ✅

Ce tutoriel est pour le boot mode UEFi, pour vérifier si vous êtes bien en UEFI : `# ls /sys/firmware/efi/efivars`

Si le dossier est affiché sans erreur alors vous êtes bien en UEFI.

## 3. Connexion à internet 📶

Pour être sur que votre interface réseau est activée : `# ip link`

Pour la connection sans fil veuillez utiliser : `# iwctl`

Si tout vas bien vous devriez voir apparaitre `[iwd]#` à l'écran.

Pour connaitre le nom de votre appareil sans fil : `[iwd]# device list`

Maintenant il faut scanner les réseaux disponibles : `[iwd]# station DEVICE scan`

Les afficher à l'écran : `[iwd]# station DEVICE get-networks`

Pour vous connectez à votre réseau sans fil : `[iwd]# station device connect NOM_DU_RESEAU`

Si un mot de passe est necessaire il vous sera demandé de le taper.

Pour sortir tapez `[iwd]# exit`

Pour vérifier votre connexion exécuter la commande : `# ping 8.8.8.8`

Aucune erreur ? Bravo vous êtes desormais connecté à l'internet !

## 4. Mise a jour de l'horloge 🕑

Pour s'assurer que l'horloge interne est à jour : `# timedatectl set-ntp true`

## 5. Partitionner les disques 💾

Cette partie s'applique au espace de stockage complètement vide.

Pour afficher chaque disques et ses partitions : `# fdisk -l`

Pour commencer la partition du disque executez : `# fdisk /dev/LE_DISQUE_A_PARTITIONNER`

!!! Attention choisissez bien votre disque, et suivez bien les instructions sous peine de voir votre disque totalement effacé !!!

#### 5.1 Partition Racine

Maintenant nous allons créer la partition Racine, celle ou tout vos fichier se retrouverons pour cela tapez `n` puis entrer.

il s'affiche `Numero de la partition` ... tapez entrer.

il s'affiche `Premier secteur`... tapez entrer.

il s'affiche `Dernier secteur`... , La taille minimum requise est de 10G tapez donc une valeur bien superieure, `+30G`

Effectuer les modifications : tapez `w` puis entrer.

Formattage partition Racine : `# mkfs.ext4 /dev/VOTRE_PARTITION_RACINE`

## 6. Monter la partition racine 🔼

Pour continuer il vous faut monter votre partition racine : `# mount /dev/VOTRE_PARTITION_RACINE /mnt`

## 7. Installation des paquets esssentiels 📥

L'installation des paquet essentiel ce fait via pacstrap : `# pacstrap /mnt nano base linux linux-firmware`

Nous installerons le reste des paquets dont vous avez besoin plus tard.

## 8. Configuration de votre systeme

Générer un fichier "fstab"  : `# genfstab -U /mnt >> /mnt/etc/fstbab`

Root dans votre nouveau systeme : `# arch-chroot /mnt`

Changer votre fuseau horaire : `# ln -sf /usr/share/zoneinfo/REGION/VILLE /etc/localtime`

    Si vous êtes en france la commade sera : `# ln -sf /usr/share/zoneinfo/Europe/Paris /etc/localtime`

Générer le fichier /etc/adjtime : `hwclock --systohc`

Editer le fichier /etc/locale.gen : `nano /etc/locale.gen`

Trouvez la ligne fr_FR.UTF-8 UTF-8, supprimer le `#` en debut de ligne puis faites ctrl+S et ctrl+X pour sauvegarder et sauver les modifications.

Puis exécuter : `# locale-gen` pour générer les changements.

Créez le fichier /etc/locale.conf : `# nano /etc/locale.conf`

Puis écrire : `LANG=fr_FR.UTF-8` et faites ctrl+S et ctrl+X comme precedemment.

Créer le fichier /etc/vconsole.conf : `# nano /etc/vconsole.conf`

Puis écrire : `KEYMAP=NOM_DU_LAYOUT` ou "NOM_DU_LAYOUT" et le layout choisis au debut de l'installation et faites ctrl+S et ctrl+X comme precedemment.

Créer le fichier /etc/hostname : `# nano /etc/hostname`

Puis écrire le nom que votre machine portera sur cet OS, vous pouvez mettre ce que vous voulez alors lachez-vous !

Créer le fichier /etc/hosts : `# nano /etc/hosts`, descendre de 3 lignes et ecrire :

```
127.0.0.1        localhost
::1              localhost
127.0.1.1        NOM_DE_VOTRE_MACHINE
```

## 9. Configurer les utilisateurs 👤

❗ **CHANGEZ LE MOT DE PASSE ROOT : ATTENTION AVEC CE MOT DE PASSE VOUS AVEZ LA MAIN SUR VOTRE SYSTEM ENTIER, NE LE PARTAGEZ PAS ET CHOISISSEZ UN MDP FORT.** ❗

Recommandation mots de passe : https://www.economie.gouv.fr/particuliers/creer-mot-passe-securise

Commande : `# passwd`

#### 9.1 Creation de votre utilisateur

Pour créer votre utilisateur : `# useradd -mG wheel,video,audio,optical,storage VOTRE_NOM_D'UTILISATEUR`

Changez votre mot de passe aussi avec : `# passwd VOTRE_NOM_D'UTILISATEUR`

#### 9.2 Creer l'utilisateur stagiaire

Pour créer le stagiaire : `# useradd -m Stagiaire`

Changez le mot de passe aussi avec : `# passwd Stagiaire`

#### 9.3 Configuration du groupe wheel

Installation de sudo : ` # pacman -S sudo`

Il faut éditer le fichier de configuration : `# EDITOR=nano visudo` Ici EDITOR n'est pas une variable

Parcourez le fichier jusqu'à trouver la ligne : `# %wheel ALL=(ALL) ALL` et supprimez le `#` en debut de ligne puis faites ctrl+S et ctrl+X.

## 10. Installation du grub

Installation des paquets requis : ` # pacman -S grub efibootmgr dosfstools os-prober mtools`

Création du repertoire /boot/EFI : ` # mkdir /boot/EFI`

Montez la partition EFI dans /boot/EFI : ` # mount /dev/LA_PARTITION_EFI /boot/EFI`

Installez le grub : ` # grub-install --target=x86_64-efi --bootloader-id=archlinux_grub --recheck`

Créer le fichier de configurration du grub : `# grub-mkconfig -o /boot/grub/grub.cfg`

## 11. Installation des paquets 📥

Mise à jour des paquets : `# pacman -Syu`

Installation des paquets indispensable : `# pacman -S networkmanager man-pages`

Installer ssh : `# pacman -S openssh`

Installer apache : ` # pacman -S apache`

Installation de lightdm : `# pacman -S lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings`

Gestionnaire de Bureau / Fenetre : `# pacman -S i3-gaps i3blocks i3lock i3status xfce4 gnome ratpoison`

Compiler un programme C : `# pacman -S gcc tcc`

Lire les pages de man : `# pacman -S man-db texinfo`

Lancer un make : `# pacman -S make`

Editer du code source : `# pacman -S emacs vim code`

Debogguer du code : ` # pacman -S gdb ghidra`

Naviguer sur le web : `# pacman -S firefox chromium lynx`

INFORMATION : lynx est un navigateur basé sur le textuel, il s'utilise dans le terminal.

Editer une image matricielle / vectorielle  : ` # pacman -S imagemagick inkscape gimp`

(Dé)compresser des fichier : ` # pacman -S p7zip`

## Lancement au demarrage

Lancer ssh au demarrage : ` # systemctl enable sshd`

Lancer lightdm au demarrage : ` # systemctl enable lightdm`

Lancer networkmanager au demarrage : ` # systemctl enable NetworkManager`

Lancer le server web au demarrage : `# systemctl enable httpd`

## Fin de l'installation

Sortir : `# exit`

Reboot : `# reboot`

Vous pouvez enlever votre iso quand le pc s'eteint.

Votre installation d'Archlinux est terminée ! Bravo !
























