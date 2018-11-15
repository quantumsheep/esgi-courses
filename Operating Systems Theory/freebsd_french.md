# FreeBSD
## Installation
Afin d'installer FreeBSD nous utilisaon l'iso "bootonly"(https://download.freebsd.org/ftp/releases/amd64/amd64/ISO-IMAGES/) en mode Live CD.

- Pour changer la disposition du clavier : `kbdcontrol -l <LANG>`, par example `kbdcontrol -l fr`

Nous avons actuellement un disque dans `/dev` qui s'appelle `da0`. La commande suivante utilise `da0`.

- Initialisation du disque avec `gpart create -s gpt da0`
- Définition du Bootloader avec une taille de 512ko `gpart bootcode -b /boot/pmbr d0a`
- Ecriture du Bootloader avec `gpart add -t freebsd-boot -s 512k da0`
- Ajout de la partition EFI d'une taille de 800ko à da0 `gpart add -t efi -s 800k da0`
- Ajout de la partition GPT à da0 `gpart add -p /boot/gptboot -i 1 da0`

Ensuite, nous allons séparer notre éspace disque restant:
```
gpart add -t freebsd-ufs -s 1g -l fb_root da0
gpart add -t freebsd-ufs -s 4g -l fb_usr da0
gpart add -t freebsd-ufs -s 2g -l fb_var da0
gpart add -t freebsd-ufs -s 2g -l fb_swap da0
```

Nous utilisons ces commandes pour creer des partitions, UFS (Unix File System) sont les disques qui seront utilisés pour stocker nos données. SWAP est utilisé pour créer un disque de fichiers temporaires pour l'échange de données.

Lors de la création de cette configuration, Nous avons un partitionnement comme celui-ci (depuis un disque de 20Go):
```
[ MBR   | Table part (GPT) | da0 1  | da0 2 | / (root) | /usb | /var | Free space ]
  40 Ko   512 Ko             800 Ko   1 Go    1 Go       4 Go   2 Go   11 Go
```

`gpart show -l` retourne la configuration suivante:
```
=>          40 41942960  da0   GPT      (20G)
            40     1024    1  (null)    (512K)
          1064     1600    2  (null)    (800K)
          2664  2097152    3  fb_root   (1.0G)
       2099816  8388608    4  fb_usr    (4.0G)
      10488424  4194304    5  fb_var    (2.0G)
      14682728  4194304    6  fb_swap   (2.0G)
      18877032 23065960       - free -  (11G)
```

Enfin nous initialisons les partitions en temps que système de fichier :
```
newfs gpt/fb_root
newfs -U gpt/fb_usr
newfs -U gpt/fb_var
```

## Informations
- Le Bootloader est capable de comprendre le noyau. Le noyau est stocké dans le fichier /dev/

## Commandes
- kdbmap -> CLI qui permet à l'utilisateur de changer la disposition de son clavier
- gpart -> Outil de partitionnement
  - `gpart show -l` Pour lister les partitions
  - `gpart add -t <TYPE> -s <SIZE> -l <NAME> <DISK>` pour ajouter une partition
  - `gpart delete -i <DISK>` pour supprimer une partition
- newfs -> Definition d'une partition de disque en système de fichier
  - `newfs -U <MOUNT_POINT>` Activation de l'écriture différé sur la partition du disque afin d'augmenter les performances de celui-ci