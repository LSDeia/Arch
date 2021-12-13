# ArchLinux

Wiki installation ArchLinux

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

Pour sortir taper `[iwd]# exit`.

Pour verifier votre connection executer la commande : `# ping 8.8.8.8`

Aucune erreur ? Bravo vous etes desormais connecte a l'internet !









