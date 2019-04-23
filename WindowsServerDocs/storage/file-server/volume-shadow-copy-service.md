---
Title: Service VSS (page éventuellement en anglais)
ms.date: 01/30/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7b61a0494b8a63168b40bfaed42dedf0fff40c35
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887260"
---
# <a name="volume-shadow-copy-service"></a>Service VSS (page éventuellement en anglais)

S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

Sauvegarde et restauration des données critiques peuvent être très complexes en raison des problèmes suivants :

  - Les données doivent généralement être sauvegardées alors que les applications qui génèrent les données sont en cours d’exécution. Cela signifie que certains des fichiers de données peuvent être ouvertes ou ils peuvent être dans un état incohérent.  
      
  - Si le jeu de données est volumineux, il peut être difficile de sauvegarder l’intégralité de celui-ci en même temps.  
      

Effectuer correctement les opérations backup et restore nécessite étroite coordination entre les applications de sauvegarde, les applications line of business qui sont en cours de sauvegarde, et le matériel de gestion de stockage et les logiciels. Le VSS Volume Shadow Copy Service (), qui a été introduite dans Windows Server® 2003, facilite la conversation entre ces composants pour leur permettre de mieux collaborer. Lorsque tous les composants prend en charge VSS, vous pouvez les utiliser pour sauvegarder les données de votre application sans mettre les applications hors connexion.

VSS coordonne les actions qui sont requises pour créer des clichés (également appelé un instantané ou une copie de point-à-temps) des données qui doivent être sauvegardées. Le cliché instantané peut être utilisé en tant que-est, ou elle peut être utilisée dans des scénarios tels que les éléments suivants :

  - Vous souhaitez sauvegarder des données et du système état informations de l’application, y compris l’archivage des données vers un autre lecteur de disque dur, sur bande ou sur un autre support amovible.  
      
  - Vous êtes d’exploration de données.  
      
  - Vous effectuez des sauvegardes de disque à disque.  
      
  - Vous avez besoin d’une récupération rapide à partir de la perte de données en restaurant les données à l’unité logique d’origine ou à un numéro d’unité logique entièrement nouveau qui remplace un numéro d’unité logique d’origine qui a échoué.  
      

Fonctionnalités de Windows et les applications qui utilisent VSS sont les suivantes :

  - [Sauvegarde de Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) ()http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [Les clichés instantanés de dossiers partagés](http://go.microsoft.com/fwlink/?linkid=142874) ()http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=180892) ()http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [Restauration du système](http://go.microsoft.com/fwlink/?linkid=180893) ()http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>Fonctionne du Service de cliché instantané de Volume

Une solution VSS complète requiert que tous les composants de base suivantes :

**Le service VSS**   partie du système d’exploitation Windows qui garantit que les autres composants permettre communiquer entre eux correctement et fonctionnent ensemble.

**Demandeur VSS**   le logiciel qui demande la création de copies clichés instantanés (ou d’autres opérations de haut niveau telles que l’importation ou de les supprimer). En règle générale, il s’agit de l’application de sauvegarde. L’utilitaire sauvegarde Windows Server et l’application de System Center Data Protection Manager sont les demandeurs VSS. Les demandeurs non-Microsoft® VSS incluent presque tous les logiciels de sauvegarde qui s’exécute sur Windows.

**Enregistreur VSS**   le composant garantissant que nous avons un ensemble cohérent de données à sauvegarder. Cela est généralement fourni en tant que partie d’une application line of business, tels que SQL Server® ou Exchange Server. Les enregistreurs VSS pour les différents composants de Windows, par exemple le Registre, sont inclus dans le système d’exploitation Windows. Enregistreurs VSS de non Microsoft sont inclus avec de nombreuses applications pour Windows qui doivent garantir la cohérence des données lors de la sauvegarde des.

**Fournisseur VSS**   le composant qui crée et gère les clichés instantanés. Cela peut se produire dans le logiciel ou matériel. Le système d’exploitation Windows inclut un fournisseur VSS qui utilise la copie sur écriture. Si vous utilisez un réseau de zone de stockage (SAN), il est important d’installer le fournisseur de matériel VSS pour le réseau SAN, le cas échéant. Un fournisseur de matériel permet de décharger la tâche de création et la maintenance d’un cliché instantané du système d’exploitation hôte.

Le diagramme suivant illustre la façon dont le service VSS coordonnées avec demandeurs, les scripteurs et les fournisseurs pour créer un cliché instantané d’un volume.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figure 1**   diagramme Architectural de Volume Shadow Copy Service

### <a name="how-a-shadow-copy-is-created"></a>Création d’un cliché instantané

Cette section place les différents rôles du demandeur, le writer et le fournisseur dans le contexte en répertoriant les étapes qui doivent être prises pour créer un cliché instantané. Le diagramme suivant montre comment le Service de cliché instantané de Volume contrôle la coordination globale du demandeur, le writer et le fournisseur.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figure 2** processus de création de clichés instantanés

Pour créer un cliché instantané, le demandeur, le writer et le fournisseur d’effectuent les actions suivantes :

1.  Le demandeur demande le Service de cliché instantané de Volume pour énumérer les enregistreurs, rassembler les métadonnées de l’enregistreur et préparer pour la création de clichés instantanés.  
      
2.  Chaque enregistreur crée une description XML des magasins de données et les composants qui doivent être sauvegardées et lui fournit pour le Service de cliché instantané de Volume. L’enregistreur définit également une méthode de restauration, ce qui est utilisée pour tous les composants. Le Service VSS fournit la description de l’enregistreur au demandeur, qui sélectionne les composants qui seront sauvegardées.  
      
3.  Le Service de cliché instantané de Volume informe tous les enregistreurs de préparer leurs données pour effectuer un cliché instantané.  
      
4.  Chaque enregistreur prépare les données comme il convient, telles que la fin de toutes les transactions en cours, restauration des journaux de transactions et vider les caches. Lorsque les données sont prêtes à être une copie fantôme, l’enregistreur informe le Service de cliché instantané de Volume.  
      
5.  Le Service VSS dit aux writers de geler temporairement les demandes d’e/s écriture application (demandes d’e/s sont toujours possibles pour la lecture) pour les quelques secondes sont nécessaires pour créer le cliché instantané de volume ou volumes. Le blocage d’application n’est pas autorisé à prendre plus de 60 secondes. Le Service de cliché instantané de Volume vide les mémoires tampons de fichiers système et puis se fige le système de fichiers, ce qui garantit que les métadonnées de système de fichier sont enregistrée correctement et cliché instantané des données sont écrit dans un ordre cohérent.  
      
6.  Le Service VSS demande au fournisseur pour créer le cliché instantané. La période de la création de clichés instantanés copie dure pas plus de 10 secondes, pendant laquelle tous les écrire des requêtes d’e/s au système de fichiers restent figées.  
      
7.  Le Service de cliché instantané de Volume libère des requêtes d’e/s d’écriture fichiers système.  
      
8.  VSS dit aux writers de libérer les demandes d’e/s écriture application. À ce stade les applications sont gratuites reprendre l’écriture de données sur le disque est en cours de cliché instantané.  
      

> [!NOTE]
> La création de clichés instantanés peut être annulée si les enregistreurs sont conservées dans l’état de blocage pendant plus de 60 secondes ou si les fournisseurs de prennent plus de 10 secondes pour valider le cliché instantané. 
<br>

9.  Le demandeur peut recommencer le processus (allez à l’étape 1) ou pour avertir l’administrateur pour réessayer ultérieurement.  
      
10. Si le cliché instantané est créé avec succès, le Service de cliché instantané de Volume retourne les informations d’emplacement pour le cliché instantané au demandeur. Dans certains cas, le cliché instantané peut être temporairement rendue disponible en un volume en lecture-écriture afin que VSS et une ou plusieurs applications peuvent modifier le contenu de la copie de clichés instantanés avant que le cliché instantané est terminé. Une fois que VSS et les applications apporter leurs modifications, le cliché instantané est effectué en lecture seule. Cette phase est appelée récupération automatique, et il est utilisé pour annuler les transactions de système de fichiers ou une application sur le volume de clichés instantanés qui n’ont pas été terminées avant que le cliché instantané a été créé.  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>Comment le fournisseur crée un cliché instantané

Un fournisseur de clichés instantanés matériels ou logiciels utilise l’une des méthodes suivantes pour la création d’un cliché instantané :

**Copie complète**   cette méthode effectue une copie complète (appelée « copie complète » ou « clonage ») du volume d’origine à un moment donné dans le temps. Cette copie est en lecture seule.

**Copie sur écriture**   cette méthode ne copie pas le volume d’origine. Au lieu de cela, il fait une copie différentielle en copiant toutes les modifications (requêtes d’e/s écriture terminée) qui sont apportées au volume après un moment donné dans le temps.

**Redirection lors de l’écriture**   cette méthode ne copie pas le volume d’origine, et il ne rend pas toutes les modifications sur le volume d’origine après un moment donné dans le temps. Au lieu de cela, il fait une copie différentielle en redirigeant toutes les modifications vers un autre volume.

## <a name="complete-copy"></a>Copie complète

Une copie complète est généralement créée en effectuant un « miroir fractionné » comme suit :

1.  Le volume d’origine et le volume de clichés instantanés sont un ensemble de volume en miroir.  
      
2.  Le volume de clichés instantanés est séparé du volume d’origine. Cela interrompt la connexion de mise en miroir.  
      

Une fois la connexion de mise en miroir est interrompue, le volume d’origine et le volume de clichés instantanés sont indépendants. Le volume d’origine continue d’accepter toutes les modifications (demandes d’e/s d’écriture), lors de la copie de clichés instantanés volume reste une copie en lecture seule exacte des données d’origine au moment du saut.

### <a name="copy-on-write-method"></a>Méthode de copie sur écriture

Dans la méthode de copie sur écriture, lorsque se produit une modification du volume d’origine (mais avant l’écriture de la demande d’e/s terminée), chaque bloc à modifier est lu et ensuite écrite dans zone de stockage du volume (également appelé son « zone diff »). La zone de stockage de clichés instantanés peut être sur le même volume ou un autre volume. Cela conserve une copie du bloc de données sur le volume d’origine avant la modification s’il le remplace.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Temps</th>
<th>Source de données (état et données)</th>
<th>Cliché instantané (état et données)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Données d’origine : 1 2 3 4 5</p></td>
<td><p>Aucune copie : :</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Données modifiées dans le cache : 3 à 3 »</p></td>
<td><p>Clichés instantanés créés (différences uniquement) : 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Données d’origine remplacées : 1 2 3’ 4 5</p></td>
<td><p>Différences et des index stockés sur le cliché : 3</p></td>
</tr>
</tbody>
</table>

**Tableau 1**   la méthode de copie sur écriture de la création de clichés instantanés

La méthode de copie sur écriture est une méthode rapide pour la création d’un cliché instantané, puisqu’il copie uniquement les données qui sont modifiées. Les blocs copiés dans la zone diff peuvent être combinés avec les données modifiées sur le volume d’origine pour restaurer le volume à son état avant que les modifications ont été apportées. S’il existe de nombreuses modifications, la méthode de copie sur écriture peut devenir coûteuse.

### <a name="redirect-on-write-method"></a>Méthode de redirection lors de l’écriture

Dans la méthode de redirection lors de l’écriture, chaque fois que le volume d’origine reçoit une modification (demande d’e/s d’écriture), la modification n’est pas appliquée sur le volume d’origine. Au lieu de cela, la modification est écrite à la zone de stockage des clichés instantanés d’un autre volume.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Temps</th>
<th>Source de données (état et données)</th>
<th>Cliché instantané (état et données)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Données d’origine : 1 2 3 4 5</p></td>
<td><p>Aucune copie : :</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Données modifiées dans le cache : 3 à 3 »</p></td>
<td><p>Clichés instantanés créés (différences uniquement) : 3’</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Données d’origine est inchangé : 1 2 3 4 5</p></td>
<td><p>Différences et des index stockés sur le cliché : 3’</p></td>
</tr>
</tbody>
</table>

**Tableau 2**   la méthode de redirection lors de l’écriture de la création de clichés instantanés

Comme la méthode de copie sur écriture, la méthode de redirection lors de l’écriture est une méthode rapide pour la création d’un cliché instantané, puisqu’il copie uniquement les modifications apportées aux données. Les blocs copiés dans la zone diff peuvent être combinés avec les données non modifiées sur le volume d’origine pour créer une copie complète et à jour des données. S’il existe de nombreuses demandes d’e/s de lecture, la méthode de redirection lors de l’écriture peut devenir coûteuse.

## <a name="shadow-copy-providers"></a>Fournisseurs de clichés instantanés

Il existe deux types de fournisseurs de clichés instantanés : fournisseurs en fonction du matériel et basé sur logiciel. Il existe également un fournisseur de système, qui est un fournisseur de logiciel qui est intégré au système d’exploitation Windows.

### <a name="hardware-based-providers"></a>Fournisseurs de matériel

En fonction du matériel de clichés instantanés copie fournisseurs act en tant qu’interface entre le Service de cliché instantané de Volume et le niveau de matériel en travaillant conjointement avec un adaptateur de stockage de matériel ou un contrôleur. Le travail de création et la gestion de la copie de clichés instantanés est effectué par la baie de stockage.

Fournisseurs de matériel ont toujours le cliché instantané d’un LUN entière, mais le Volume Shadow Copy Service expose uniquement le cliché instantané de volume ou volumes qui ont été demandées.

Un fournisseur de clichés instantanés de matériels, utilise le Service VSS fonctionnalité qui définit le point dans le temps, autorise la synchronisation des données, gère le cliché instantané et fournit une interface commune aux applications de sauvegarde. Toutefois, le Service VSS ne spécifie pas le mécanisme par lequel le fournisseur de matériel génère et gère les clichés instantanés.

### <a name="software-based-providers"></a>Fournisseurs de logiciels

Fournisseurs de clichés instantanés basé sur logiciel généralement intercepter et traiter lisent et écrivent les demandes d’e/s dans une couche logicielle entre le système de fichiers et les logiciels de gestionnaire de volume.

Ces fournisseurs sont implémentées comme un composant DLL en mode utilisateur et le pilote de périphérique en mode noyau au moins un, en général, un pilote de filtre de stockage. Contrairement aux fournisseurs de matériel, fournisseurs de logiciels créent des clichés au niveau du logiciel, non au niveau du matériel.

Un fournisseur de cliché instantané de logiciel doit conserver une vue « point-à-temps » d’un volume en ayant un accès à un jeu de données qui peut être utilisé pour recréer l’état du volume avant l’heure de la création des clichés instantanés. Un exemple est la technique de copie sur écriture du fournisseur système. Cependant, le Service de cliché instantané de Volume ne place aucune restriction sur quelles technique utilisée par les fournisseurs de logiciels pour créer et gérer des clichés instantanés.

Un fournisseur de logiciel s’applique à un large éventail de plateformes de stockage à un fournisseur basé sur le matériel, et elle doit fonctionner tout aussi bien avec des disques de base ou des volumes logiques. (Un volume logique est un volume qui est créé en combinant l’espace libre à partir de deux ou plusieurs disques.) Contrairement aux clichés instantanés de matériels, fournisseurs de logiciels de consomment des ressources de système d’exploitation pour maintenir le cliché instantané.

Pour plus d’informations sur les disques de base, consultez [quelles sont les disques de base et les Volumes ?](http://go.microsoft.com/fwlink/?linkid=180894) (http://go.microsoft.com/fwlink/?LinkId=180894) sur TechNet.

### <a name="system-provider"></a>Fournisseur du système

Un fournisseur de clichés instantanés, le fournisseur du système, est fourni dans le système d’exploitation Windows. Même si un fournisseur par défaut est fourni dans Windows, les autres fournisseurs sont libres de fournir des implémentations optimisées pour leurs applications stockage de configurations matérielle et logicielle.

Pour mettre à jour la vue « point-à-temps » d’un volume qui est contenue dans un cliché instantané, le fournisseur du système utilise une technique de copie sur écriture. Les copies des blocs sur le volume qui ont été modifiées depuis le début de la création de clichés instantanés sont stockés dans une zone de stockage de clichés instantanés.

Le fournisseur du système peut exposer le volume de production, qui peut être écrit à et lire à partir de normalement. Lorsque le cliché instantané est nécessaire, il applique logiquement les différences dans les données sur le volume de production pour exposer le cliché instantané complet.

Pour le fournisseur du système, la zone de stockage de clichés instantanés doit être sur un volume NTFS. Le volume de clichés instantanés ne doive pas être un volume NTFS, mais au moins un volume monté sur le système doit être un volume NTFS.

Les fichiers des composants qui composent le fournisseur du système sont swprv.dll et volsnap.sys.

### <a name="in-box-vss-writers"></a>Emploi enregistreurs VSS

Le système d’exploitation Windows inclut un ensemble d’enregistreurs VSS qui sont responsables de l’énumération des données qui sont requis par diverses fonctionnalités de Windows.

Pour plus d’informations sur ces enregistreurs, consultez les sites Web Microsoft suivants :

  - [L’emploi enregistreurs VSS](http://go.microsoft.com/fwlink/?linkid=180895) ()http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [Nouveaux enregistreurs VSS de l’emploi pour Windows Server 2008 et Windows Vista SP1](http://go.microsoft.com/fwlink/?linkid=180896) ()http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [Nouveaux enregistreurs VSS de l’emploi pour Windows Server 2008 R2 et Windows 7](http://go.microsoft.com/fwlink/?linkid=180897) ()http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>Utilisation des clichés instantanés

Outre sauvegarder les informations d’état application système et de données, les clichés instantanés utilisable pour de nombreuses fins, notamment les suivantes :

  - Restauration des numéros d’unité logique (la resynchronisation du numéro d’unité logique et le remplacement de numéro d’unité logique)  
      
  - Restauration de fichiers spécifiques (clichés instantanés pour dossiers partagés)  
      
  - Exploration de données à l’aide des clichés instantanés transportables  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Restauration des numéros d’unité logique (la resynchronisation du numéro d’unité logique et le remplacement de numéro d’unité logique)

Dans Windows Server 2008 R2 et Windows 7, les demandeurs VSS peuvent utiliser une matériel fournisseur fonctionnalité de cliché instantané appelée la resynchronisation du numéro d’unité logique (ou « Resynchronisation LUN »). Il s’agit d’un modèle de récupération rapide qui permet à un administrateur d’application restaurer les données à partir d’un cliché instantané à l’unité logique d’origine ou à un nouveau LUN.

Le cliché instantané peut être un clone complet ou un cliché instantané différentielle. Dans les deux cas, à la fin de l’opération de resynchronisation, la numéro d’unité logique de destination aura le même contenu que le cliché instantané numéro d’unité logique. Pendant l’opération de resynchronisation, le tableau effectue une copie au niveau du bloc à partir du cliché vers la destination du numéro d’unité logique.


> [!NOTE]
> Le cliché instantané doit être une copie de clichés instantanés transportables de matériel. 
<br>


La plupart des tableaux permettent des opérations d’e/s de production reprendre le peu de temps après le début de l’opération de resynchronisation. Lorsque l’opération de resynchronisation est en cours d’exécution, lire les demandes sont redirigées vers le cliché instantané numéro d’unité logique, requêtes et d’écriture vers la destination du numéro d’unité logique. Ainsi, les tableaux à récupérer les jeux de données très volumineux et reprendre les opérations normales dans quelques secondes.

La resynchronisation de numéro d’unité logique est différente de l’échange de numéro d’unité logique. Un échange de numéro d’unité logique est un scénario de récupération rapide VSS a pris en charge depuis Windows Server 2003 SP1. Dans un échange de numéro d’unité logique, le cliché instantané est importé, puis converti en un volume en lecture-écriture. La conversion est une opération irréversible, et le volume et le numéro d’unité logique sous-jacent ne peut pas être contrôlée avec les API VSS après cela. La liste suivante décrit la manière dont la resynchronisation du numéro d’unité logique compare avec le remplacement de numéro d’unité logique :

  - Resynchronisation de numéro d’unité logique, le cliché instantané n’est pas modifié, afin qu’il peut être utilisé plusieurs fois. Dans le remplacement de numéro d’unité logique, le cliché instantané peut servir qu’une seule fois pour une récupération. Pour les administrateurs plus soucieux de la sécurité, il est important. Quand la resynchronisation du numéro d’unité logique est utilisée, le demandeur peut réessayer l’opération de restauration si une erreur survient lors de la première fois.  
      
  - À la fin d’un échange de numéro d’unité logique, le cliché instantané numéro d’unité logique est utilisé pour les demandes d’e/s de production. Pour cette raison, le cliché instantané numéro d’unité logique doit utiliser la même qualité de stockage que le numéro d’unité logique de production d’origine pour vous assurer que les performances ne sont pas affectées après l’opération de récupération. Si la resynchronisation du numéro d’unité logique est utilisée au lieu de cela, le fournisseur de matériel peut conserver le cliché instantané sur un stockage moins coûteux que le stockage de la qualité de production.  
      
  - Si la destination de numéro d’unité logique est inutilisable et doit être recréé, le remplacement de numéro d’unité logique peut être plus économique, car il ne nécessite pas une numéro d’unité logique de destination.  
      


> [!WARNING]
> Toutes les opérations répertoriées sont des opérations au niveau du numéro d’unité logique. Si vous tentez de récupérer un volume spécifique à l’aide de la resynchronisation du numéro d’unité logique, vous allez involontairement pour rétablir toutes les autres volumes qui partagent le numéro d’unité logique. 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Restauration de fichiers spécifiques (clichés instantanés pour dossiers partagés)

Clichés instantanés pour dossiers partagés utilise le Service VSS pour fournir des point-à-temps des copies de fichiers qui se trouvent sur une ressource réseau partagée, comme un serveur de fichiers. Avec les clichés instantanés pour dossiers partagés, les utilisateurs peuvent récupérer rapidement des fichiers supprimés ou modifiés qui sont stockés sur le réseau. Car ils peuvent le faire sans assistance de l’administrateur, les clichés instantanés pour dossiers partagés peuvent augmenter la productivité et réduire les coûts administratifs.

Pour plus d’informations sur les clichés instantanés pour dossiers partagés, consultez [clichés instantanés pour dossiers partagés](http://go.microsoft.com/fwlink/?linkid=180898) (http://go.microsoft.com/fwlink/?LinkId=180898) sur TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Exploration de données à l’aide des clichés instantanés transportables

Avec un fournisseur de matériel qui est conçu pour une utilisation avec le Service de cliché instantané de Volume, vous pouvez créer des clichés instantanés transportables qui peuvent être importés sur des serveurs dans le même sous-système (par exemple, un réseau SAN). Ces clichés instantanés peuvent être utilisés pour amorcer une production ou de tester l’installation avec des données en lecture seule pour l’exploration de données.

Avec le Service de cliché instantané de Volume et une baie de stockage avec un fournisseur de matériel qui est conçu pour une utilisation avec le Service VSS, il est possible de créer un cliché instantané du volume de données source sur un seul serveur et réimporter le cliché instantané sur un autre serveur  (ou sur le même serveur). Ce processus s’effectue en quelques minutes, quelle que soit la taille des données. Le processus de transport s’effectue via une série d’étapes qui utilisent un demandeur de copie de clichés instantanés (une application de gestion du stockage) qui prend en charge des clichés instantanés transportables.

## <a name="to-transport-a-shadow-copy"></a>Pour transporter un cliché instantané

1.  Créer une copie de clichés instantanés transportables de la source de données sur un serveur.

2.  Importer le cliché instantané vers un serveur qui est connecté au réseau SAN (vous pouvez importer vers un autre serveur ou le même serveur).

3.  Les données sont maintenant prêtes à être utilisées.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figure 3**   création de clichés instantanés et transport entre deux serveurs


> [!NOTE]
> Une copie de clichés instantanés transportables qui est créée sur Windows Server 2003 ne peut pas être importée sur un serveur qui exécute Windows Server 2008 ou Windows Server 2008 R2. Impossible d’importer une copie de clichés instantanés transportables qui a été créée sur Windows Server 2008 ou Windows Server 2008 R2 sur un serveur qui exécute Windows Server 2003. Toutefois, un cliché instantané qui est créé sur Windows Server 2008 peut être importé sur un serveur qui exécute Windows Server 2008 R2 et vice versa. 
<br>


Les clichés instantanés sont en lecture seule. Si vous souhaitez convertir un cliché instantané à un numéro d’unité logique de lecture/écriture, vous pouvez utiliser une application de gestion du stockage basé sur le Service de disque virtuel (y compris des demandeurs) en plus du Service de cliché instantané de Volume. À l’aide de cette application, vous pouvez supprimer le cliché instantané de la gestion du Service de cliché instantané de Volume et la convertir en un numéro d’unité logique de lecture/écriture.

Transport de Service de cliché instantané de volume est une solution avancée sur les ordinateurs exécutant Windows Server 2003 Enterprise Edition, Windows Server 2003 Datacenter Edition, Windows Server 2008 ou Windows Server 2008 R2. Il fonctionne uniquement s’il existe un fournisseur de matériel sur la baie de stockage. Transport de copie de clichés instantanés peut servir pour de nombreuses fins, y compris les sauvegardes sur bande, les données d’exploration de données et de test.

## <a name="frequently-asked-questions"></a>Forum Aux Questions

Ce forum aux questions sur les services Internet (VSS, Volume Shadow Copy Service) pour les administrateurs système. Pour plus d’informations sur les interfaces de programmation d’application VSS, consultez [Volume Shadow Copy Service](http://go.microsoft.com/fwlink/?linkid=180899) (http://go.microsoft.com/fwlink/?LinkId=180899) dans la bibliothèque de centre de développement Windows.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>Lorsque le Volume Shadow Copy Service a été introduit ? Sur quelles versions de système d’exploitation Windows est-il disponible ?

VSS a été introduite dans Windows XP. Il est disponible sur Windows XP, Windows Server 2003, Windows Vista®, Windows Server 2008, Windows 7 et Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>Quelle est la différence entre un cliché instantané et une sauvegarde ?

Dans le cas d’une sauvegarde de disque dur, le cliché instantané créé est également la sauvegarde. Les données peuvent être copiées désactiver le cliché instantané pour une restauration ou le cliché instantané peut être utilisé pour un scénario de récupération rapide, par exemple, la resynchronisation du numéro d’unité logique ou le remplacement de numéro d’unité logique.

Lorsque les données sont copiées à partir du cliché instantané sur bande ou autre support amovible, le contenu qui est stocké sur le support constitue la sauvegarde. Le cliché instantané lui-même peut être supprimé une fois que les données sont copiées à partir de celui-ci.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>Qu’est le plus grand volume de taille qui prend en charge de Volume Shadow Copy Service ?

Volume Shadow Copy Service prend en charge une taille de volume de jusqu'à 64 To.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>J’ai effectué une sauvegarde sur Windows Server 2008. Puis-je restaurer sur Windows Server 2008 R2 ?

Cela dépend du logiciel de sauvegarde que vous avez utilisé. La plupart des programmes de sauvegarde prend en charge ce scénario pour les données, mais pas pour les sauvegardes d’état du système.

Clichés instantanés sont créés sur une de ces versions de Windows peuvent être utilisés sur l’autre.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>J’ai effectué une sauvegarde sur Windows Server 2003. Puis-je restaurer sur Windows Server 2008 ?

Cela dépend du logiciel de sauvegarde que vous avez utilisé. Si vous créez un cliché instantané sur Windows Server 2003, vous ne pouvez pas l’utiliser sur Windows Server 2008. En outre, si vous créez un cliché instantané sur Windows Server 2008, vous ne pouvez pas la restaurer sur Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>Comment puis-je désactiver VSS ?

Il est possible de désactiver le Service de cliché instantané de Volume à l’aide de la Console de gestion Microsoft. Toutefois, vous ne devez pas le faire. La désactivation de VSS négativement affecte tout logiciel que vous utilisez et qui en dépend, telles que la restauration du système et de sauvegarde de Windows Server.

Pour plus d’informations, consultez les sites Web Microsoft TechNet suivants :

  - [Restauration du système](http://go.microsoft.com/fwlink/?linkid=157113) ()http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Sauvegarde de Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) ()http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>Puis-je exclure des fichiers à partir d’un cliché instantané à économiser de l’espace ?

VSS est conçu pour créer des clichés instantanés des volumes entiers. Les fichiers temporaires, tels que les fichiers de pagination, sont omis automatiquement à partir des clichés instantanés pour économiser de l’espace.

Pour exclure des fichiers spécifiques de clichés instantanés, utilisez la clé de Registre suivante : **FilesNotToSnapshot**.


> [!NOTE]
> Le <STRONG>FilesNotToSnapshot</STRONG> clé de Registre est destinée à être utilisée uniquement par les applications. Les utilisateurs qui tentent d’utiliser rencontreront des limitations, telles que les éléments suivants :
<br>
<UL>
<LI>Il ne peut pas supprimer des fichiers à partir d’un cliché instantané qui a été créé sur un serveur Windows à l’aide de la fonctionnalité Versions précédentes.<BR><BR>
<LI>Il ne peut pas supprimer les fichiers de clichés instantanés pour dossiers partagés.<BR><BR>
<LI>Il peut supprimer des fichiers à partir d’un cliché instantané qui a été créé à l’aide de la [Diskshadow](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/diskshadow) utilitaire, mais il ne peut pas supprimer des fichiers à partir d’un cliché instantané qui a été créé à l’aide de la [Vssadmin](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/vssadmin) utilitaire.<BR><BR>
<LI>Fichiers sont supprimés à partir d’un cliché instantané sur une mesure du possible. Cela signifie qu’ils ne sont pas garantis pour être supprimé.<BR><BR></LI></UL>


Pour plus d’informations, consultez [à l’exclusion de fichiers à partir de clichés instantanés](http://go.microsoft.com/fwlink/?linkid=180904) (http://go.microsoft.com/fwlink/?LinkId=180904) sur MSDN.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Mon programme de sauvegarde non-Microsoft a échoué avec une erreur VSS. Que puis-je faire ?

Consultez la section de prise en charge de produit du site Web de la société qui a créé le programme de sauvegarde. Il peut y avoir une mise à jour de produit que vous pouvez télécharger et installer pour résoudre le problème. Si ce n’est pas le cas, contactez le service de support produit de la société.

Administrateurs système peuvent utiliser les informations de dépannage VSS sur le site Web de la bibliothèque Microsoft TechNet suivant pour recueillir des informations de diagnostic sur les problèmes liés à VSS.

Pour plus d’informations, consultez [Volume Shadow Copy Service](http://go.microsoft.com/fwlink/?linkid=180905) (http://go.microsoft.com/fwlink/?LinkId=180905) sur TechNet.

### <a name="what-is-the-diff-area"></a>Qu’est la « zone diff » ?

La zone de stockage de clichés instantanés (ou « zone diff ») est l’emplacement où sont stockées les données pour le cliché instantané créé par le fournisseur de logiciel du système.

### <a name="where-is-the-diff-area-located"></a>Où se trouve la zone diff ?

La zone diff peut se trouver sur n’importe quel volume local. Toutefois, il doit se trouver sur un volume NTFS qui dispose de suffisamment d’espace pour stocker.

### <a name="how-is-the-diff-area-location-determined"></a>Quelle est l’emplacement de la zone diff déterminé ?

Les critères suivants sont évalués dans l’ordre, pour déterminer l’emplacement de la zone diff :

  - Si un volume possède déjà un cliché instantané existant, cet emplacement est utilisé.  
      
  - S’il existe une association manuelle préconfigurée entre le volume d’origine et l’emplacement de volume de clichés instantanés, cet emplacement est utilisé.  
      
  - Si les deux critères précédents ne fournissent pas un emplacement, le service de cliché instantané choisit une localisation basée sur l’espace libre disponible. Si plus d’un volume est en cours de clichés instantanés, le service VSS crée une liste des emplacements possibles instantané selon la taille d’espace libre, dans l’ordre décroissant. Le nombre d’emplacements fourni est égal au nombre de volumes en cours de clichés instantanés.  
      
  - Si le volume en cours de clichés instantanés est un des emplacements possibles, une association locale est créée. Sinon, une association avec le volume avec le plus grand espace disponible est créée.  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>VSS peut créer des clichés instantanés des volumes non NTFS ?

Oui. Toutefois, les clichés instantanés persistant est possible uniquement pour les volumes NTFS. En outre, au moins un volume monté sur le système doit être un volume NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>Qu’est le nombre maximal de clichés instantanés que je peux créer en même temps ?

Le nombre maximal de volumes de clichés instantanés dans un jeu de clichés instantanés unique est 64. Notez que cela n’est pas le même que le nombre de clichés instantanés.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>Ce qui est le nombre maximal de clichés instantanés logiciels créé par le fournisseur du système que je peux mettre à jour pour un volume ?

Le nombre maximal est de clichés instantanés logiciels pour chaque volume est 512. Toutefois, par défaut, vous pouvez uniquement gérer 64 clichés instantanés qui sont utilisés par la fonctionnalité clichés instantanés de dossiers partagés. Pour modifier la limite de la fonctionnalité clichés instantanés de dossiers partagés, utilisez la clé de Registre suivante : **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>Comment puis-je contrôler l’espace qui est utilisé pour l’espace de stockage de clichés instantanés ?

Type de la **vssadmin redimensionner shadowstorage** commande.

Pour plus d’informations, consultez [Vssadmin redimensionner shadowstorage](http://go.microsoft.com/fwlink/?linkid=180906) (http://go.microsoft.com/fwlink/?LinkId=180906) sur TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>Que se passe-t-il lorsque je manque d’espace ?

Clichés instantanés pour le volume sont supprimés, en commençant par le cliché le plus ancien.

## <a name="volume-shadow-copy-service-tools"></a>Outils du Service de cliché instantané de volume

Le système d’exploitation Windows fournit les outils suivants pour travailler avec VSS :

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907) (http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](http://go.microsoft.com/fwlink/?linkid=84008) (http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

Oui, DiskShadow est un demandeur VSS que vous pouvez utiliser pour gérer tous les instantanés de matériels et logiciels que vous pouvez avoir sur un système. DiskShadow inclut des commandes telles que les éléments suivants :

  - **Liste**: Répertorie les enregistreurs VSS, les fournisseurs VSS et les clichés instantanés  
      
  - **créer**: Crée une nouvelle copie de clichés instantanés  
      
  - **importer**: Importe une copie de clichés instantanés transportables  
      
  - **exposer**: Expose un cliché instantané persistant (comme une lettre de lecteur, par exemple)  
      
  - **rétablir**: Rétablit un volume vers un cliché instantané spécifié  
      

Cet outil est destiné aux professionnels de l’informatique, mais les développeurs peuvent également s’avérer utile lorsque vous testez un enregistreur VSS ou le fournisseur VSS.

DiskShadow est disponible uniquement sur les systèmes d’exploitation Windows Server. Il n’est pas disponible sur les systèmes d’exploitation clients Windows.

### <a name="vssadmin"></a>VssAdmin

VssAdmin sert à créer, supprimer et répertorier des informations sur les clichés instantanés. Il peut également être utilisé pour redimensionner la zone de stockage (« zone diff »).

VssAdmin inclut des commandes telles que les éléments suivants :

  - **créer des clichés instantanés**: Crée une nouvelle copie de clichés instantanés  
      
  - **supprimer les ombres**: Supprime les clichés instantanés  
      
  - **liste des fournisseurs**: Répertorie tous les fournisseurs VSS inscrits  
      
  - **liste des enregistreurs**: Répertorie tous les abonnés des enregistreurs VSS  
      
  - **redimensionner shadowstorage**: Modifie la taille maximale de la zone de stockage de clichés instantanés  
      

VssAdmin peut uniquement servir à administrer les clichés instantanés sont créés par le fournisseur de logiciels système.

VssAdmin est disponible sur le client de Windows et les versions de système d’exploitation Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Clés de Registre Volume Shadow Copy Service

Les clés de Registre suivantes sont disponibles pour une utilisation avec VSS :

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

Cette clé est utilisée pour spécifier quels utilisateurs ont accès aux clichés instantanés.

Pour plus d’informations, consultez les entrées suivantes sur le site Web MSDN :

  - [Considérations sur la sécurité pour les Writers](http://go.microsoft.com/fwlink/?linkid=157739) ()http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [Considérations de sécurité pour les demandeurs](http://go.microsoft.com/fwlink/?linkid=180908) ()http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

Cette clé spécifie le nombre maximal de copies clichés instantanés accessibles par les clients qui peuvent être stockées sur chaque volume de l’ordinateur. Clichés instantanés accessibles par les clients sont utilisés par les clichés instantanés pour dossiers partagés.

Pour plus d’informations, consultez l’entrée suivante sur le site Web MSDN :

**MaxShadowCopies** sous [pour la sauvegarde et restauration des clés de Registre](http://go.microsoft.com/fwlink/?linkid=180909) ()http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Cette clé spécifie la taille initiale minimale, en Mo, de la zone de stockage de clichés instantanés.

Pour plus d’informations, consultez l’entrée suivante sur le site Web MSDN :

**MinDiffAreaFileSize** sous [pour la sauvegarde et restauration des clés de Registre](http://go.microsoft.com/fwlink/?linkid=180910) ()http://go.microsoft.com/fwlink/?LinkId=180910)

`##`# « Versions de système d’exploitation prises en charge

Le tableau suivant répertorie les versions minimale du système d’exploitation pris en charge pour les fonctionnalités VSS.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Fonction VSS</th>
<th>Client minimal pris en charge</th>
<th>Serveur minimal pris en charge</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Resynchronisation de numéro d’unité logique</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p><strong>FilesNotToSnapshot</strong> clé de Registre</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="odd">
<td><p>Clichés instantanés transportables</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2003 avec SP1</p></td>
</tr>
<tr class="even">
<td><p>Clichés instantanés matériels</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Versions précédentes de Windows Server</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="even">
<td><p>Récupération rapide à l’aide de la permutation de numéro d’unité logique</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2003 avec SP1</p></td>
</tr>
<tr class="odd">
<td><p>Plusieurs importations de clichés instantanés matériels</p>
<div class="alert">
<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><img src="media/volume-shadow-copy-service/Dd560667.note(WS.10).gif" />Remarque</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Il s’agit de la capacité d’importer un cliché instantané plusieurs fois. Opération d’importation qu’une seule peut être effectuée à la fois.
<p></p></td>
</tr>
</tbody>
</table>
<p></p>
</div></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Clichés instantanés pour dossiers partagés</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Récupéré automatiquement clichés instantanés transportables</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sessions de sauvegarde simultanées (jusqu'à 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Session de restauration unique simultanée avec des sauvegardes</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 avec SP2</p></td>
</tr>
<tr class="even">
<td><p>Jusqu'à 8 sessions de restauration simultanées avec les sauvegardes</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Voir aussi

[Volume Shadow Copy Service dans le centre de développement Windows](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)