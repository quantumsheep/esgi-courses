Dans un disque dur classique, il y a plusieurs bras écrivant sur les diskues, le bras reste fixe et fait tourner le disque pour sélectionner la lecture sur laquelle il va lire.

Un disque fait 200 tours seconde, 7400 tours minutes pour les plus rapides.
Il y a des mécanismes anti choc pour pallier à la friction et l'inertie générée par ces tours.a
Chaque disque est dispatché en plusieurs secteurs (3).
On lis donc tous les plateaux en même temps.

un disque dur contient :
    -un plateau bisurface
    -1 tete de lecture par surface
    -des secteurs (512 octets / 4K)
    -un cylindre (alignant les têtes)

La vitesse du disque dur dépend du seek-time, le temps que va mettre le bras pour se positionner sur la piste qu'il a besoin de consulter. Ce mouvement est entre 10 et 50ms, ce qui correspond à une éternité pour le processeur.
Le deuxième temps est lié à la latence : Il va falloir attendre que le bras passe le secteur de lecture voulu. Il s'agit de la latence. (entre 60 et 200 tps)
 go 
Le transfert time compte aussi mais n'est pas très important so fuck it.

Il y a plusieurs type de stockage :
Constant Linear Velocity (ce n'est pas adapté au disque dur, uniquement pour les CD/DVD/BlueRay car cela augmente le "seek time"):
La densité de donnée sur le disque est la même partout mais pas la vitesse.

Pour les disques durs, la technologie utilisé est la "constant angulare velocity" :
La vitesse est constante, la densitée varie (plus importante au centre qu'a l'exterieur). Avec cette variation de densitée, le flux de données est plus constante.


Glossaire : 
DMA 
Direct memory Access : 

Un file system sert à 
- Organiser les fichiers et les programmes 
- Génère et gère les fichiers et les dossiers (qui sont des fichiers)
- Gérer les sockets et les pseudos devices

Il sert d'interface "abstraites"
 Utilisateur
 pROGRAMMES

 Il gère les régles d'accès aux fichiers
 Il gère aussi la taille du fichiers, son nom et ses propriétaires la date de modification et son type.

FAT : F ile Allocation Table


                            