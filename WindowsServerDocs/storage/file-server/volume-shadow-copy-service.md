---
title: Service VSS (page éventuellement en anglais)
ms.date: 01/30/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3fc184f8f23e4325198e3a1a08f20109c2c577e8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867319"
---
# <a name="volume-shadow-copy-service"></a>Service VSS (page éventuellement en anglais)

S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

La sauvegarde et la restauration des données critiques de l’entreprise peuvent être très complexes en raison des problèmes suivants :

  - En règle générale, les données doivent être sauvegardées alors que les applications qui produisent les données sont toujours en cours d’exécution. Cela signifie que certains des fichiers de données sont peut-être ouverts ou qu’ils sont dans un état incohérent.  
      
  - Si le jeu de données est volumineux, il peut être difficile de le sauvegarder en même temps.  
      

L’exécution correcte des opérations de sauvegarde et de restauration nécessite une coordination étroite entre les applications de sauvegarde, les applications métier en cours de sauvegarde et le matériel et les logiciels de gestion du stockage. Le Service VSS (VSS), qui a été introduit dans Windows Server® 2003, facilite la conversation entre ces composants afin de leur permettre de mieux fonctionner ensemble. Lorsque tous les composants prennent en charge VSS, vous pouvez les utiliser pour sauvegarder vos données d’application sans mettre les applications hors connexion.

VSS coordonne les actions requises pour créer un cliché instantané cohérent (également connu sous le nom de capture instantanée ou une copie ponctuelle) des données à sauvegarder. Le cliché instantané peut être utilisé tel quel, ou il peut être utilisé dans des scénarios tels que les suivants :

  - Vous souhaitez sauvegarder les données d’application et les informations d’État du système, y compris l’archivage des données sur un autre disque dur, sur une bande ou sur un autre support amovible.  
      
  - Vous êtes l’exploration de données.  
      
  - Vous effectuez des sauvegardes de disque à disque.  
      
  - Vous avez besoin d’une récupération rapide après une perte de données en restaurant les données sur le numéro d’unité logique d’origine ou sur un numéro d’unité logique entièrement nouveau qui remplace un numéro d’unité logique d’origine qui a échoué.  
      

Les fonctionnalités Windows et les applications qui utilisent VSS sont les suivantes :

  - [Sauvegarde Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [Clichés instantanés de dossiers partagés](http://go.microsoft.com/fwlink/?linkid=142874) (http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [Data Protection Manager de System Center](http://go.microsoft.com/fwlink/?linkid=180892) (http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [Restauration du système](http://go.microsoft.com/fwlink/?linkid=180893) (http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>Fonctionnement de Service VSS

Une solution VSS complète requiert tous les éléments de base suivants :

   Composant du service VSS du système d’exploitation Windows qui garantit que les autres composants peuvent communiquer entre eux correctement et fonctionner ensemble.

**Demandeur VSS logiciel**qui demande la création réelle de clichés instantanés (ou d’autres opérations de haut niveau telles que l’importation ou la suppression).    En règle générale, il s’agit de l’application de sauvegarde. L’utilitaire Sauvegarde Windows Server et l’application System Center Data Protection Manager sont des demandeurs VSS. Les demandeurs VSS non-Microsoft® incluent presque tous les logiciels de sauvegarde qui s’exécutent sur Windows.

**Enregistreur VSS composant**qui garantit que nous disposons d’un jeu de données cohérent à sauvegarder.    Il est généralement fourni dans le cadre d’une application métier, par exemple SQL Server® ou Exchange Server. Les enregistreurs VSS des différents composants Windows, tels que le registre, sont fournis avec le système d’exploitation Windows. Les enregistreurs VSS non-Microsoft sont inclus avec de nombreuses applications pour Windows qui doivent garantir la cohérence des données lors de la sauvegarde.

**Fournisseur VSS composant**quicréeetgèrelesclichésinstantanés   . Cela peut se produire dans le logiciel ou dans le matériel. Le système d’exploitation Windows comprend un fournisseur VSS qui utilise la copie en écriture. Si vous utilisez un réseau de zone de stockage (SAN), il est important d’installer le fournisseur de matériel VSS pour le réseau SAN, si celui-ci est fourni. Un fournisseur de matériel décharge la tâche de création et de maintenance d’un cliché instantané du système d’exploitation hôte.

Le diagramme suivant illustre la façon dont le service VSS coordonne avec les demandeurs, les rédacteurs et les fournisseurs de créer un cliché instantané d’un volume.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figure 1**   diagramme architectural de service VSS

### <a name="how-a-shadow-copy-is-created"></a>Création d’un cliché instantané

Cette section met en contexte les différents rôles du demandeur, de l’enregistreur et du fournisseur en répertoriant les étapes à suivre pour créer un cliché instantané. Le diagramme suivant montre comment le Service VSS contrôle la coordination globale du demandeur, de l’enregistreur et du fournisseur.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figure 2** Processus de création de clichés instantanés

Pour créer un cliché instantané, le demandeur, le rédacteur et le fournisseur effectuent les actions suivantes :

1.  Le demandeur demande à l’Service VSS d’énumérer les writers, de collecter les métadonnées de l’enregistreur et de préparer la création de clichés instantanés.  
      
2.  Chaque enregistreur crée une description XML des composants et des banques de données qui doivent être sauvegardées et les fournit au Service VSS. Le writer définit également une méthode de restauration, qui est utilisée pour tous les composants. Le Service VSS fournit la description du writer au demandeur, qui sélectionne les composants qui seront sauvegardés.  
      
3.  L’Service VSS avertit tous les enregistreurs de préparer leurs données pour créer un cliché instantané.  
      
4.  Chaque enregistreur prépare les données comme il convient, par exemple en effectuant toutes les transactions ouvertes, en exécutant des journaux des transactions et en vidant des caches. Lorsque les données sont prêtes à être copiées par des clichés instantanés, l’enregistreur avertit l’Service VSS.  
      
5.  Le Service VSS indique aux enregistreurs de geler temporairement les demandes d’e/s d’écriture d’application (les demandes d’e/s de lecture sont toujours possibles) pendant les quelques secondes nécessaires à la création du cliché instantané du ou des volumes. Le blocage de l’application ne peut pas durer plus de 60 secondes. Le Service VSS vide les mémoires tampons du système de fichiers, puis fige le système de fichiers, ce qui garantit que les métadonnées du système de fichiers sont correctement enregistrées et que les données à copier par cliché instantané sont écrites dans un ordre cohérent.  
      
6.  Le Service VSS indique au fournisseur de créer le cliché instantané. La période de création de clichés instantanés ne dure pas plus de 10 secondes, pendant lesquelles toutes les demandes d’e/s d’écriture sur le système de fichiers restent figées.  
      
7.  Le Service VSS libère les demandes d’e/s d’écriture dans le système de fichiers.  
      
8.  VSS indique aux rédacteurs de libérer les demandes d’e/s d’écriture d’application. À ce stade, les applications sont libres de reprendre l’écriture des données sur le disque qui fait l’objet d’une copie fantôme.  
      

> [!NOTE]
> La création de clichés instantanés peut être abandonnée si les enregistreurs sont conservés dans l’État gelé pendant plus de 60 secondes ou si les fournisseurs prennent plus de 10 secondes pour valider le cliché instantané. 
<br>

9. Le demandeur peut réessayer le processus (revenir à l’étape 1) ou notifier l’administrateur qu’il doit réessayer ultérieurement.  
      
10. Si le cliché instantané est correctement créé, le Service VSS retourne les informations d’emplacement du cliché instantané au demandeur. Dans certains cas, le cliché instantané peut être rendu temporairement disponible en tant que volume en lecture-écriture afin que VSS et une ou plusieurs applications puissent modifier le contenu du cliché instantané avant la fin du cliché instantané. Une fois que VSS et les applications ont été modifiés, le cliché instantané est mis en lecture seule. Cette phase est appelée récupération automatique et permet d’annuler toutes les transactions de système de fichiers ou d’application sur le volume de clichés instantanés qui n’ont pas été terminées avant la création du cliché instantané.  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>Comment le fournisseur crée un cliché instantané

Un fournisseur de clichés instantanés matériel ou logiciel utilise l’une des méthodes suivantes pour créer un cliché instantané :

**Copie complète cette**méthode effectue une copie complète (appelée « copie complète » ou « clone ») du volume d’origine à un moment donné.    Cette copie est en lecture seule.

**Copie sur écriture**   cette méthode ne copie pas le volume d’origine. Au lieu de cela, il effectue une copie différentielle en copiant toutes les modifications (demandes d’e/s d’écriture terminées) effectuées sur le volume après un point donné dans le temps.

**Redirection-on-Write**   cette méthode ne copie pas le volume d’origine et n’apporte aucune modification au volume d’origine après un point donné dans le temps. Au lieu de cela, il effectue une copie différentielle en redirigeant toutes les modifications vers un volume différent.

## <a name="complete-copy"></a>Copie complète

Une copie complète est généralement créée en créant un « miroir scindé » comme suit :

1.  Le volume d’origine et le volume de clichés instantanés sont un ensemble de volumes mis en miroir.  
      
2.  Le volume de clichés instantanés est séparé du volume d’origine. Cela rompt la connexion miroir.  
      

Une fois la connexion miroir interrompue, le volume d’origine et le volume de clichés instantanés sont indépendants. Le volume d’origine continue d’accepter toutes les modifications (écriture des demandes d’e/s), tandis que le volume du cliché instantané reste une copie exacte en lecture seule des données d’origine au moment de l’arrêt.

### <a name="copy-on-write-method"></a>Méthode de copie sur écriture

Dans la méthode de copie sur écriture, lorsqu’une modification apportée au volume d’origine se produit (mais avant la fin de la requête d’e/s d’écriture), chaque bloc à modifier est lu, puis écrit dans la zone de stockage de clichés instantanés du volume (également appelée « zone diff »). La zone de stockage des clichés instantanés peut se trouver sur le même volume ou sur un volume différent. Cela préserve une copie du bloc de données sur le volume d’origine avant que la modification ne le remplace.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>Données sources (État et données)</th>
<th>Cliché instantané (État et données)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Données d’origine : 1 2 3 4 5</p></td>
<td><p>Aucune copie :</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Données modifiées dans le cache : 3 à 3 '</p></td>
<td><p>Cliché instantané créé (différences uniquement) : 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Données d’origine remplacées : 1 2 3 ' 4 5</p></td>
<td><p>Différences et index stockés sur le cliché instantané : 3</p></td>
</tr>
</tbody>
</table>

**Tableau 1**   méthode de copie sur écriture pour la création de clichés instantanés

La méthode de copie sur écriture est une méthode rapide pour créer un cliché instantané, car elle copie uniquement les données modifiées. Les blocs copiés dans la zone diff peuvent être combinés avec les données modifiées sur le volume d’origine pour restaurer le volume à son état avant l’exécution de l’une des modifications. Si de nombreuses modifications sont apportées, la méthode de copie sur écriture peut devenir coûteuse.

### <a name="redirect-on-write-method"></a>Méthode de redirection en écriture

Dans la méthode de redirection en écriture, chaque fois que le volume d’origine reçoit une modification (demande d’e/s d’écriture), la modification n’est pas appliquée au volume d’origine. Au lieu de cela, la modification est écrite dans la zone de stockage des clichés instantanés d’un autre volume.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>Données sources (État et données)</th>
<th>Cliché instantané (État et données)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Données d’origine : 1 2 3 4 5</p></td>
<td><p>Aucune copie :</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Données modifiées dans le cache : 3 à 3 '</p></td>
<td><p>Cliché instantané créé (différences uniquement) : 1,3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Données d’origine inchangées : 1 2 3 4 5</p></td>
<td><p>Différences et index stockés sur le cliché instantané : 1,3</p></td>
</tr>
</tbody>
</table>

**Tableau 2**   méthode de redirection en écriture pour la création de clichés instantanés

À l’instar de la méthode de copie sur écriture, la méthode de redirection en écriture est une méthode rapide pour créer un cliché instantané, car elle copie uniquement les modifications apportées aux données. Les blocs copiés dans la zone diff peuvent être combinés avec les données non modifiées sur le volume d’origine pour créer une copie complète et à jour des données. S’il existe de nombreuses demandes d’e/s en lecture, la méthode de redirection en écriture peut devenir coûteuse.

## <a name="shadow-copy-providers"></a>Fournisseurs de clichés instantanés

Il existe deux types de fournisseurs de clichés instantanés : les fournisseurs basés sur le matériel et les fournisseurs de logiciels. Il existe également un fournisseur système, qui est un fournisseur de logiciels intégré au système d’exploitation Windows.

### <a name="hardware-based-providers"></a>Fournisseurs basés sur le matériel

Les fournisseurs de clichés instantanés basés sur le matériel jouent le rôle d’interface entre les Service VSS et le niveau matériel en travaillant conjointement avec un contrôleur ou un adaptateur de stockage matériel. Le travail de création et de maintenance du cliché instantané est effectué par le groupe de stockage.

Les fournisseurs de matériel prennent toujours le cliché instantané d’un numéro d’unité logique (LUN) entier, mais le Service VSS expose uniquement le cliché instantané du ou des volumes qui ont été demandés.

Un fournisseur de clichés instantanés basé sur le matériel utilise les fonctionnalités de Service VSS qui définissent le point dans le temps, permet la synchronisation des données, gère le cliché instantané et fournit une interface commune avec les applications de sauvegarde. Toutefois, le Service VSS ne spécifie pas le mécanisme sous-jacent par lequel le fournisseur basé sur le matériel produit et gère les clichés instantanés.

### <a name="software-based-providers"></a>Fournisseurs basés sur des logiciels

Les fournisseurs de clichés instantanés basés sur des logiciels interceptent et traitent généralement les demandes d’e/s en lecture et en écriture dans une couche logicielle entre le système de fichiers et le logiciel du gestionnaire de volume.

Ces fournisseurs sont implémentés en tant que composant DLL en mode utilisateur et au moins un pilote de périphérique en mode noyau, généralement un pilote de filtre de stockage. Contrairement aux fournisseurs basés sur le matériel, les fournisseurs de logiciels créent des clichés instantanés au niveau du logiciel, et non pas au niveau du matériel.

Un fournisseur de clichés instantanés logiciel doit conserver une vue « ponctuelle » d’un volume en ayant accès à un jeu de données qui peut être utilisé pour recréer l’état du volume avant la création du cliché instantané. C’est le cas, par exemple, de la technique de copie sur écriture du fournisseur système. Toutefois, le Service VSS n’impose aucune restriction quant à la technique utilisée par les fournisseurs de logiciels pour créer et gérer des clichés instantanés.

Un fournisseur de logiciels s’applique à un plus grand nombre de plates-formes de stockage qu’un fournisseur basé sur le matériel. il doit également fonctionner avec des disques de base ou des volumes logiques. (Un volume logique est un volume créé par la combinaison de l’espace libre de deux disques ou plus.) Contrairement aux clichés instantanés matériels, les fournisseurs de logiciels consomment des ressources de système d’exploitation pour maintenir le cliché instantané.

Pour plus d’informations sur les disques de base, consultez [que sont les disques et les volumes de base ?](http://go.microsoft.com/fwlink/?linkid=180894) (http://go.microsoft.com/fwlink/?LinkId=180894) sur TechNet.

### <a name="system-provider"></a>Fournisseur système

Un fournisseur de clichés instantanés, le fournisseur système, est fourni dans le système d’exploitation Windows. Bien qu’un fournisseur par défaut soit fourni dans Windows, les autres fournisseurs sont libres de fournir des implémentations optimisées pour leur matériel de stockage et leurs applications logicielles.

Pour tenir à jour la vue « ponctuelle » d’un volume contenu dans un cliché instantané, le fournisseur système utilise une technique de copie sur écriture. Les copies des blocs sur le volume qui ont été modifiés depuis le début de la création du cliché instantané sont stockées dans une zone de stockage des clichés instantanés.

Le fournisseur système peut exposer le volume de production, qui peut être écrit et lu normalement. Lorsque le cliché instantané est nécessaire, il applique logiquement les différences aux données sur le volume de production pour exposer le cliché instantané complet.

Pour le fournisseur système, la zone de stockage des clichés instantanés doit se trouver sur un volume NTFS. Le volume à copier Shadow ne doit pas nécessairement être un volume NTFS, mais au moins un volume monté sur le système doit être un volume NTFS.

Les fichiers de composants qui composent le fournisseur système sont swprv. dll et Volsnap. sys.

### <a name="in-box-vss-writers"></a>Enregistreurs VSS intégrés

Le système d’exploitation Windows comprend un ensemble de Writers VSS chargés d’énumérer les données requises par les différentes fonctionnalités de Windows.

Pour plus d’informations sur ces Writers, consultez les sites Web Microsoft suivants :

  - [Enregistreurs VSS intégrés](http://go.microsoft.com/fwlink/?linkid=180895) (http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [Nouveaux enregistreurs VSS en boîte pour Windows Server 2008 et Windows Vista SP1](http://go.microsoft.com/fwlink/?linkid=180896) (http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [Nouveaux enregistreurs VSS en boîte pour Windows Server 2008 R2 et Windows 7](http://go.microsoft.com/fwlink/?linkid=180897) (http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>Utilisation des clichés instantanés

Outre la sauvegarde des données d’application et des informations sur l’état du système, les clichés instantanés peuvent être utilisés à plusieurs fins, notamment les suivantes :

  - Restauration des numéros d’unités logiques (resynchronisation des LUN et échange de LUN)  
      
  - Restauration de fichiers individuels (clichés instantanés pour dossiers partagés)  
      
  - Exploration de données à l’aide des clichés instantanés transportables  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Restauration des numéros d’unités logiques (resynchronisation des LUN et échange de LUN)

Dans Windows Server 2008 R2 et Windows 7, les demandeurs VSS peuvent utiliser une fonctionnalité du fournisseur de cliché instantané matériel appelée resynchronisation des LUN (ou « resynchronisation de LUN »). Il s’agit d’un schéma de récupération rapide qui permet à un administrateur d’application de restaurer des données à partir d’un cliché instantané vers le numéro d’unité logique d’origine ou vers un nouveau numéro d’unité logique.

Le cliché instantané peut être un clone complet ou un cliché instantané différentiel. Dans les deux cas, à la fin de l’opération de resynchronisation, le numéro d’unité logique de destination aura le même contenu que le numéro d’unité logique de cliché instantané. Pendant l’opération de resynchronisation, le tableau effectue une copie au niveau du bloc à partir du cliché instantané vers le numéro d’unité logique de destination.


> [!NOTE]
> Le cliché instantané doit être un cliché instantané matériel transférable. 
<br>


La plupart des tableaux permettent aux opérations d’e/s de production de reprendre peu après le début de l’opération de resynchronisation. Pendant que l’opération de resynchronisation est en cours, les demandes de lecture sont redirigées vers le numéro d’unité logique de cliché instantané et les demandes d’écriture sur le numéro d’unité logique de destination. Cela permet aux tableaux de récupérer des jeux de données très volumineux et de reprendre les opérations normales en quelques secondes.

La resynchronisation des numéros d’unités logiques diffère de l’échange de LUN. Un échange de LUN est un scénario de récupération rapide pris en charge par VSS depuis Windows Server 2003 SP1. Dans un échange de LUN, le cliché instantané est importé, puis converti en volume en lecture-écriture. La conversion est une opération irréversible, et le volume et le numéro d’unité logique sous-jacent ne peuvent pas être contrôlés avec les API VSS. La liste suivante décrit la comparaison de la resynchronisation des numéros d’unités logiques avec l’échange de LUN :

  - Dans la resynchronisation des LUN, le cliché instantané n’est pas modifié et peut donc être utilisé plusieurs fois. Dans le cas d’un échange d’unités logiques, le cliché instantané ne peut être utilisé qu’une seule fois pour une récupération. C’est important pour les administrateurs les plus attentifs à la sécurité. Lorsque la resynchronisation des LUN est utilisée, le demandeur peut réessayer l’intégralité de l’opération de restauration en cas de problème pour la première fois.  
      
  - À la fin de l’échange d’un numéro d’unité logique, le numéro d’unité logique du cliché instantané est utilisé pour les demandes d’e/s de production. Pour cette raison, le numéro d’unité logique du cliché instantané doit utiliser la même qualité de stockage que le numéro d’unité logique de production d’origine pour garantir que les performances ne sont pas affectées après l’opération de récupération. Si la resynchronisation des numéros d’unités logiques est utilisée à la place, le fournisseur de matériel peut conserver le cliché instantané sur le stockage qui est moins onéreux que le stockage de qualité production.  
      
  - Si le numéro d’unité logique de destination est inutilisable et doit être recréé, l’échange de LUN peut être plus économique, car il ne nécessite pas de LUN de destination.  
      


> [!WARNING]
> Toutes les opérations répertoriées sont des opérations de niveau LUN. Si vous tentez de récupérer un volume spécifique à l’aide de la resynchronisation des numéros d’unités logiques, vous allez involontairement restaurer tous les autres volumes qui partagent le numéro d’unité logique. 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Restauration de fichiers individuels (clichés instantanés pour dossiers partagés)

Clichés instantanés pour dossiers partagés utilise la Service VSS pour fournir des copies ponctuelles des fichiers situés sur une ressource réseau partagée, par exemple un serveur de fichiers. Avec clichés instantanés pour dossiers partagés, les utilisateurs peuvent récupérer rapidement des fichiers supprimés ou modifiés qui sont stockés sur le réseau. Dans la mesure où ils peuvent le faire sans l’aide de l’administrateur, clichés instantanés pour dossiers partagés peut augmenter la productivité et réduire les coûts d’administration.

Pour plus d’informations sur clichés instantanés pour dossiers partagés, consultez [clichés instantanés pour dossiers partagés](http://go.microsoft.com/fwlink/?linkid=180898) (http://go.microsoft.com/fwlink/?LinkId=180898) sur TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Exploration de données à l’aide des clichés instantanés transportables

Avec un fournisseur de matériel conçu pour être utilisé avec le Service VSS, vous pouvez créer des clichés instantanés transportables qui peuvent être importés sur des serveurs au sein du même sous-système (par exemple, un réseau SAN). Ces clichés instantanés peuvent être utilisés pour amorcer une installation de production ou de test avec des données en lecture seule pour l’exploration de données.

Avec la Service VSS et une baie de stockage avec un fournisseur de matériel conçu pour être utilisé avec le Service VSS, il est possible de créer un cliché instantané du volume de données source sur un serveur, puis d’importer le cliché instantané sur un autre serveur.  (ou de nouveau sur le même serveur). Ce processus s’effectue en quelques minutes, quelle que soit la taille des données. Le processus de transport est effectué via une série d’étapes qui utilisent un demandeur de cliché instantané (une application de gestion de stockage) qui prend en charge les clichés instantanés transportables.

## <a name="to-transport-a-shadow-copy"></a>Pour transporter un cliché instantané

1.  Créez un cliché instantané transportable des données sources sur un serveur.

2.  Importez le cliché instantané sur un serveur connecté au réseau SAN (vous pouvez importer sur un autre serveur ou sur le même serveur).

3.  Les données sont maintenant prêtes à être utilisées.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figure 3**   création de clichés instantanés et transport entre deux serveurs


> [!NOTE]
> Un cliché instantané transportable créé sur Windows Server 2003 ne peut pas être importé sur un serveur qui exécute Windows Server 2008 ou Windows Server 2008 R2. Impossible d’importer un cliché instantané transportable créé sur Windows Server 2008 ou Windows Server 2008 R2 sur un serveur qui exécute Windows Server 2003. Toutefois, un cliché instantané créé sur Windows Server 2008 peut être importé sur un serveur qui exécute Windows Server 2008 R2 et vice versa. 
<br>


Les clichés instantanés sont en lecture seule. Si vous souhaitez convertir un cliché instantané en numéro d’unité logique en lecture/écriture, vous pouvez utiliser une application de gestion du stockage basée sur le service de disque virtuel (y compris certains demandeurs) en plus de la Service VSS. À l’aide de cette application, vous pouvez supprimer le cliché instantané de la gestion des Service VSS et le convertir en LUN en lecture/écriture.

Service VSS transport est une solution avancée sur les ordinateurs exécutant Windows Server 2003 Enterprise Edition, Windows Server 2003 Datacenter Edition, Windows Server 2008 ou Windows Server 2008 R2. Il fonctionne uniquement s’il existe un fournisseur de matériel sur le groupe de stockage. Le transport de clichés instantanés peut être utilisé à plusieurs fins, y compris les sauvegardes sur bande, l’exploration de données et le test.

## <a name="frequently-asked-questions"></a>Questions fréquemment posées

Ce FAQ répond à des questions sur Service VSS (VSS) pour les administrateurs système. Pour plus d’informations sur les interfaces de programmation d’applications VSS, consultez [service VSS](http://go.microsoft.com/fwlink/?linkid=180899) (http://go.microsoft.com/fwlink/?LinkId=180899) dans la bibliothèque du centre de développement Windows.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>Quand la Service VSS a-t-elle été introduite ? Sur quelle version du système d’exploitation Windows est-il disponible ?

VSS a été introduit dans Windows XP. Elle est disponible sur Windows XP, Windows Server 2003, Windows Vista®, Windows Server 2008, Windows 7 et Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>Quelle est la différence entre un cliché instantané et une sauvegarde ?

Dans le cas d’une sauvegarde de disque dur, le cliché instantané créé est également la sauvegarde. Les données peuvent être copiées en dehors du cliché instantané pour une restauration ou le cliché instantané peut être utilisé pour un scénario de récupération rapide, par exemple, la resynchronisation des numéros d’unités logiques ou l’échange de LUN.

Lorsque les données sont copiées du cliché instantané sur une bande ou un autre support amovible, le contenu stocké sur le support constitue la sauvegarde. Le cliché instantané lui-même peut être supprimé une fois que les données ont été copiées à partir de celui-ci.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>Quel est le volume de plus grande taille pris en charge par Service VSS ?

Service VSS prend en charge une taille de volume allant jusqu’à 64 to.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>J’ai effectué une sauvegarde sur Windows Server 2008. Puis-je le restaurer sur Windows Server 2008 R2 ?

Cela dépend du logiciel de sauvegarde que vous avez utilisé. La plupart des programmes de sauvegarde prennent en charge ce scénario pour les données, mais pas pour les sauvegardes de l’état du système.

Les clichés instantanés créés sur l’une de ces versions de Windows peuvent être utilisés sur l’autre.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>J’ai effectué une sauvegarde sur Windows Server 2003. Puis-je le restaurer sur Windows Server 2008 ?

Cela dépend du logiciel de sauvegarde que vous avez utilisé. Si vous créez un cliché instantané sur Windows Server 2003, vous ne pouvez pas l’utiliser sur Windows Server 2008. En outre, si vous créez un cliché instantané sur Windows Server 2008, vous ne pouvez pas le restaurer sur Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>Comment puis-je désactiver VSS ?

Il est possible de désactiver le Service VSS à l’aide de la console MMC (Microsoft Management Console). Toutefois, vous ne devez pas le faire. La désactivation de VSS affecte les logiciels que vous utilisez, tels que la restauration du système et les Sauvegarde Windows Server.

Pour plus d’informations, consultez les sites Web Microsoft TechNet suivants :

  - [Restauration du système](http://go.microsoft.com/fwlink/?linkid=157113) (http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Sauvegarde Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>Puis-je exclure des fichiers d’un cliché instantané pour économiser de l’espace ?

VSS est conçu pour créer des clichés instantanés de volumes entiers. Les fichiers temporaires, tels que les fichiers d’échange, sont automatiquement omis des clichés instantanés pour économiser de l’espace.

Pour exclure des fichiers spécifiques des clichés instantanés, utilisez la clé de Registre suivante : **FilesNotToSnapshot**.


> [!NOTE]
> La clé de Registre <STRONG>FilesNotToSnapshot</STRONG> est destinée à être utilisée uniquement par les applications. Les utilisateurs qui essaient de l’utiliser rencontreront des limitations telles que les suivantes :
> <br>
> <UL>
> <LI>Il ne peut pas supprimer les fichiers d’un cliché instantané qui a été créé sur un serveur Windows à l’aide de la fonctionnalité versions précédentes.<BR><BR>
> <LI>Il ne peut pas supprimer les fichiers des clichés instantanés des dossiers partagés.<BR><BR>
> <LI>Il peut supprimer des fichiers d’un cliché instantané qui a été créé à l’aide de l’utilitaire <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">DiskShadow</a> , mais il ne peut pas supprimer les fichiers d’un cliché instantané qui a été créé à l’aide de l’utilitaire <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">vssadmin</a> .<BR><BR>
> <LI>Les fichiers sont supprimés d’un cliché instantané de manière optimale. Cela signifie qu’il n’est pas garanti qu’elles soient supprimées.<BR><BR></LI></UL>


Pour plus d’informations, consultez [exclusion de fichiers des clichés instantanés](http://go.microsoft.com/fwlink/?linkid=180904) (http://go.microsoft.com/fwlink/?LinkId=180904) sur MSDN).

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Mon programme de sauvegarde non-Microsoft a échoué avec une erreur VSS. Que puis-je faire ?

Consultez la section Support technique du site Web de la société qui a créé le programme de sauvegarde. Il peut y avoir une mise à jour de produit que vous pouvez télécharger et installer pour résoudre le problème. Si ce n’est pas le cas, contactez le service de support technique de l’entreprise.

Les administrateurs système peuvent utiliser les informations de dépannage VSS sur le site Web de la bibliothèque Microsoft TechNet suivante pour collecter des informations de diagnostic sur les problèmes liés à VSS.

Pour plus d’informations, consultez [service VSS](http://go.microsoft.com/fwlink/?linkid=180905) (http://go.microsoft.com/fwlink/?LinkId=180905) sur TechNet.

### <a name="what-is-the-diff-area"></a>Qu’est-ce que la « zone diff » ?

La zone de stockage des clichés instantanés (ou « zone diff ») est l’emplacement où sont stockées les données du cliché instantané créé par le fournisseur de logiciels système.

### <a name="where-is-the-diff-area-located"></a>Où se trouve la zone diff ?

La zone diff peut se trouver sur n’importe quel volume local. Toutefois, il doit se trouver sur un volume NTFS qui dispose de suffisamment d’espace pour le stocker.

### <a name="how-is-the-diff-area-location-determined"></a>Comment l’emplacement de la zone diff est-il déterminé ?

Les critères suivants sont évalués, dans cet ordre, pour déterminer l’emplacement de la zone diff :

  - Si un volume a déjà un cliché instantané existant, cet emplacement est utilisé.  
      
  - S’il existe une association manuelle préconfigurée entre le volume d’origine et l’emplacement du volume de clichés instantanés, cet emplacement est utilisé.  
      
  - Si les deux critères précédents ne fournissent pas d’emplacement, le service de cliché instantané choisit un emplacement en fonction de l’espace libre disponible. Si plusieurs volumes sont en cours de copie fantôme, le service de cliché instantané crée une liste d’emplacements d’instantanés possibles en fonction de la taille de l’espace libre, dans l’ordre décroissant. Le nombre d’emplacements fournis est égal au nombre de volumes en cours de clichés instantanés.  
      
  - Si le volume en cours de copie fantôme est l’un des emplacements possibles, une association locale est créée. Dans le cas contraire, une association avec le volume avec le plus d’espace disponible est créée.  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>VSS peut-il créer des clichés instantanés de volumes non-NTFS ?

Oui. Toutefois, les clichés instantanés persistants ne peuvent être créés que pour les volumes NTFS. En outre, au moins un volume monté sur le système doit être un volume NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>Quel est le nombre maximal de clichés instantanés que je peux créer en même temps ?

Le nombre maximal de volumes de clichés instantanés dans un seul jeu de clichés instantanés est de 64. Notez que ce n’est pas le même que le nombre de clichés instantanés.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>Quel est le nombre maximal de clichés instantanés de logiciels créés par le fournisseur système que je peux conserver pour un volume ?

Le nombre maximal de clichés instantanés logiciels pour chaque volume est de 512. Toutefois, par défaut, vous ne pouvez gérer que 64 clichés instantanés qui sont utilisés par la fonctionnalité clichés instantanés de dossiers partagés. Pour modifier la limite de la fonctionnalité clichés instantanés de dossiers partagés, utilisez la clé de Registre suivante : **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>Comment contrôler l’espace utilisé pour l’espace de stockage des clichés instantanés ?

Tapez la commande **vssadmin Resize ShadowStorage** .

Pour plus d’informations, consultez [vssadmin Resize ShadowStorage](http://go.microsoft.com/fwlink/?linkid=180906) (http://go.microsoft.com/fwlink/?LinkId=180906) sur TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>Que se passe-t-il lorsque je manque d’espace ?

Les clichés instantanés pour le volume sont supprimés, en commençant par le cliché instantané le plus ancien.

## <a name="volume-shadow-copy-service-tools"></a>Outils de Service VSS

Le système d’exploitation Windows fournit les outils suivants pour l’utilisation de VSS :

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907) (http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](http://go.microsoft.com/fwlink/?linkid=84008) (http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

DiskShadow est un demandeur VSS que vous pouvez utiliser pour gérer l’ensemble des captures instantanées matérielles et logicielles que vous pouvez avoir sur un système. DiskShadow comprend des commandes telles que les suivantes :

  - **liste**: Répertorie les enregistreurs VSS, les fournisseurs VSS et les clichés instantanés  
      
  - **créer**: Crée un nouveau cliché instantané  
      
  - **importation**: Importe un cliché instantané transportable  
      
  - **exposer**: Expose un cliché instantané persistant (sous la forme d’une lettre de lecteur, par exemple)  
      
  - **rétablir**: Ramène un volume à un cliché instantané spécifié  
      

Cet outil est destiné aux professionnels de l’informatique, mais les développeurs peuvent également le trouver utile lors du test d’un enregistreur VSS ou d’un fournisseur VSS.

DiskShadow est disponible uniquement sur les systèmes d’exploitation Windows Server. Elle n’est pas disponible sur les systèmes d’exploitation clients Windows.

### <a name="vssadmin"></a>VssAdmin

VssAdmin est utilisé pour créer, supprimer et répertorier des informations sur les clichés instantanés. Il peut également être utilisé pour redimensionner la zone de stockage des clichés instantanés (« zone diff »).

VssAdmin comprend les commandes suivantes :

  - **créer une ombre**: Crée un nouveau cliché instantané  
      
  - **Supprimer les ombres**: Supprime les clichés instantanés  
      
  - **répertorier les fournisseurs**: Répertorie tous les fournisseurs VSS inscrits  
      
  - **enregistreurs de liste**: Répertorie tous les enregistreurs VSS abonnés  
      
  - **Redimensionner ShadowStorage**: Modifie la taille maximale de la zone de stockage des clichés instantanés  
      

VssAdmin ne peut être utilisé que pour administrer des clichés instantanés créés par le fournisseur de logiciels système.

VssAdmin est disponible sur les versions de système d’exploitation client Windows et Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Service VSS les clés de Registre

Les clés de Registre suivantes peuvent être utilisées avec VSS :

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

Cette clé est utilisée pour spécifier les utilisateurs qui ont accès aux clichés instantanés.

Pour plus d’informations, consultez les rubriques suivantes sur le site Web MSDN :

  - [Considérations relatives à la sécurité pour les Writers](http://go.microsoft.com/fwlink/?linkid=157739) (http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [Considérations relatives à la sécurité pour les demandeurs](http://go.microsoft.com/fwlink/?linkid=180908) (http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

Cette clé spécifie le nombre maximal de clichés instantanés accessibles par le client qui peuvent être stockés sur chaque volume de l’ordinateur. Les clichés instantanés accessibles par le client sont utilisés par clichés instantanés pour dossiers partagés.

Pour plus d’informations, consultez l’entrée suivante sur le site Web MSDN :

**MaxShadowCopies** sous [clés de Registre pour la sauvegarde et la restauration](http://go.microsoft.com/fwlink/?linkid=180909) (http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Cette clé spécifie la taille initiale minimale, en Mo, de la zone de stockage des clichés instantanés.

Pour plus d’informations, consultez l’entrée suivante sur le site Web MSDN :

**MinDiffAreaFileSize** sous [clés de Registre pour la sauvegarde et la restauration](http://go.microsoft.com/fwlink/?linkid=180910) (http://go.microsoft.com/fwlink/?LinkId=180910)

`##`# 'Versions de système d’exploitation prises en charge

Le tableau suivant répertorie les versions de système d’exploitation minimales prises en charge pour les fonctionnalités VSS.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Fonctionnalité VSS</th>
<th>Client minimal pris en charge</th>
<th>Serveur minimal pris en charge</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Resynchronisation des LUN</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p>Clé de Registre <strong>FilesNotToSnapshot</strong></p></td>
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
<td><p>Récupération rapide à l’aide de l’échange de LUN</p></td>
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
<td>Il s’agit de la possibilité d’importer plusieurs fois un cliché instantané. Une seule opération d’importation peut être effectuée à la fois.
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
<td><p>clichés instantanés pour dossiers partagés</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Clichés instantanés transportables à récupération automatique</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sessions de sauvegarde simultanées (jusqu’à 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Session de restauration unique simultanée avec les sauvegardes</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 avec SP2</p></td>
</tr>
<tr class="even">
<td><p>Jusqu’à 8 sessions de restauration simultanées avec des sauvegardes</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Voir aussi

[Service VSS dans le centre de développement Windows](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)