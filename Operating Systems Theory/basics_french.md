# Base de système d'exploitation
## Chemin
Un système d'exploitation fonctionne depuis une base, depuis son démarrage:
- Hardware // Materiel
- Bootloader // Bootloader
- Kernel // Kernel ou noyau
- Syscall // Appels Système
- Userland // Environnement utilisateur

### Materiel
Il s'agit des composants d'un ordinateur. Pour utiliser le materiel, les programmes ont besoin d'appeler les syscall afin d'intéragir avec le kernel.
Par example, lorsque discord à besoin d'acceder à la webcam, il utilise un syscall.

---

## Types de systèmes d'exploitation
- Mono-process vs multi-process // Monolitiques, un seul processus ou Multi-process, plusieurs processus.
Nommé monolithique, un système d'exploitation mono-processus signifie qu'il ne peut exécuter qu'un processus à la fois. D'autre part, un système d'exploitation multi-processus peut exécuter plusieurs processus en même temps.

Aujourd'hui, le système d'exploitation monolithique "n'existe plus". Windows, MacOS, Linux, sont tous des systèmes d'exploitation multi-processus.

- Mono-users vs multi-users // Un utilisateur ou Plusieurs utilisateurs
- Families // Familles

---

## Langage machine
Appellé `assembleur`, il est utilisé afin de creer des istructions simples, qui seront envoyées au processeur,
comme des expressions mathématiques ou des integers (entiers) (ADD/SUB/DIV).

---

## Quelques failles connues liées au materiel
### Meltdown
Meltdown est une vulnérabilité matérielle affectant les microprocesseurs Intel x86, les processeurs IBM POWER et certains microprocesseurs basés sur ARM. Cela permet à un processus malhonnête de lire toute la mémoire, même s'il n'est pas autorisé à le faire.

Source: [Meltdown (vulnérabilité) on Wikipedia](https://fr.wikipedia.org/wiki/Meltdown_(vuln%C3%A9rabilit%C3%A9))
BITE
### Spectre
Spectre est une vulnérabilité qui affecte les microprocesseurs modernes effectuant une "prédiction de branche". Sur la plupart des processeurs l'exécution spéculative résultant d'une mauvaise prédiction de branche peut laisser des effets secondaires observables susceptibles de révéler des données privées aux attaquants. Par exemple, si la configuration des accès à la mémoire effectuée lors d’une telle exécution spéculative dépend de données privées, l’état résultant du cache de données constitue un canal latéral par lequel un attaquant peut extraire des informations sur les données privées en utilisant une attaque temporelle.

Source: [Spectre (vulnérabilité) on Wikipedia](https://fr.wikipedia.org/wiki/Spectre_(vuln%C3%A9rabilit%C3%A9))

### Out-of-Order execution
Les processus peuvent accéder à la mémoire avant même de l’utiliser, ce qui signifie qu'ils sont plus rapide mais su'ils accédait à une mémoire potentiellement inutile (Hyperthreading). Cela signifie qu'un exploit pourrait accéder à la mémoire.

---

## Interruption
Une interruption signifie qu'un programme crée un appel système "S'il vous plaît noyau, donnez-moi accès à la mémoire". Le noyau générera une interruption et enverra une requête à un bus de données. Le processeur s’arrêtera (réalisera une interruption). Il réalise ensuite l'action appropriée et redémarre après l'envoi de la réponse au noyau.

Il y a niveau d'urgence lorsqu'il y a plusieurs interruptions. Le clavier est moins important que le matériel réseau.
Comment le processeur sait-il réagir à une pression sur le clavier ? Au démarrage, le noyau chargera une table contenant toutes les adresses et le nombre d'interruptions, et saura comment réagir en conséquence.

---

## Mode Noyau ou Mode utilisateur
Qu'est-ce qui m'empêcherait de récupérer de la mémoire?
Tout processeur a 2 modes: noyau ou utilisateur. En mode noyau, le processeur peut faire ce qu'il veut.

Mode utilisateur: l'interruption matérielle est désactivée, il n'y à aucune interruption. Vous ne pouvez pas voir toute la mémoire en mode utilisateur.

Lors du démarrage de l'ordinateur, le bootloader démarre en mode noyau. Lorsque le noyau charge un programme, il change de mode en fonction de ses droits.

Lorsque vous êtes en mode utilisateur, vous ne pouvez pas revenir en mode noyau. Mais vous pouvez revenir au noyau dès que le processeur à besoin de revenir en mode noyau.

Comme une interruption matérielle n'est pas possible, il (le programme qui a besoin d'écrire de la mémoire, par exemple) fera une interruption logicielle qui demandera une interruption matérielle. Ceci est une interruption à deux niveaux pour atteindre une interruption matérielle en mode utilisateur

### Virtualisation
En virtualisation, il n'y a pas d'interruption matérielle puisqu'il "n'y a pas" de matériel. Ceux-ci seront simulés par un Hyperviseur.
L'hyperviseur de la machine hôte récupérera la demande et stockera la demande dans un fichier. Il fournira ensuite des réponses à la machine virtuelle.

---
BITE
## Boot // Démarrage
```
Materiel 

!
v

Processeur     <- ASM   <-  BIOS EPROM (Le travail du BIOS est de lire le MBR et de charger la mémoire)
                                                                                                   
!
v

RAM         Stock
            MBR Bootstrap
            Bootcode
            Kernel
            Init
                                                                                                                
```

### UEFI
The MBR is limited to 2.2 To.
GPT est complètement supporté (128 partitions de 9.4 Zo)
L'UEFI possède un shell avec lequel vous pouvez interagir.
L'UEFI autorise les clones de disques natifs (Stormtroopers)
L'UEFI peut créer des partitions dédiées. (NTFS FAT, taille> 40 Mo)
Pilotes (matériel, NTFS) + chargeur de démarrage

## API SYSCALLS
Un appel système (Syscall) permet d'obtenir un accès abstrait à un matériel. Nous n'avons pas besoin de savoir comment fonctionne un matériel pour y accéder et l'utiliser.

## Informations 
- Un processeur est bien plus rapide que tout autre composant matériel, même la mémoire flash.. 
- Le matériel réseau génère beaucoup d’interruptions (interruption à chaque requête sur un réseau).
- Une inondation de réseau générant de nombreuses interruptions peut être utilisée comme une attaque (DOS).
- Si vous essayez de faire une division par 0: cela générera une interruption et le système d'exploitation sera invité à interagir avec cette monstruosité.
- Il existe deux types d'interruption: les interruptions logicielles et les interruptions matérielles.

## Glossaire 
### MBR
Master Boot Record -> 512 first bytes of our Memory. A part of the MBR, the Bootstrap 416 first bytes whose purpose is to be loaded by the Bios to be executed by the process on Boot. 
me

### EPROM

### BIOS 

### ASM
Machine language used to make processor's call. (ACRONIM MEANING ??????)
 
### RAM

### FAT

### UEFI

Unified Extensible Firmware Interface is the successor of BIOS. It's a micro-software used in most part of modern computers


### GPT (ça pue)
BITE BITE 
### NTFS (montre les moi tes fesses)

### FAT(not me)