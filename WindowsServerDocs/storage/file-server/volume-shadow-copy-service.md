---
title: Service VSS (page éventuellement en anglais)
ms.date: 01/30/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 1ab941e25da7171349bb24762940af3bf886c165
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "77675360"
---
# <a name="volume-shadow-copy-service"></a>Service VSS (page éventuellement en anglais)

S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

La sauvegarde et la restauration des données métier critiques peuvent s’avérer très complexes en raison des problèmes suivants :

  - Les données ont généralement besoin d’être sauvegardées pendant que les applications qui les produisent sont encore en cours d’exécution. Cela signifie que certains des fichiers de données peuvent être ouverts ou que leur état peut être incohérent.

  - Si le jeu de données est volumineux, il peut s’avérer difficile de le sauvegarder dans sa totalité en une seule fois.


L’exécution correcte des opérations de sauvegarde et de restauration nécessite une coordination étroite entre les applications de sauvegarde, les applications métier qui font l’objet d’une sauvegarde, ainsi que le matériel et les logiciels de gestion du stockage. Le service VSS (Volume Shadow Copy Service, service de cliché instantané de volume), introduit dans Windows Server® 2003, facilite la conversation entre ces composants afin de leur permettre de mieux fonctionner ensemble. Quand tous les composants prennent en charge VSS, vous pouvez les utiliser pour sauvegarder vos données d’application sans mettre les applications hors connexion.

VSS coordonne les actions nécessaires pour créer un cliché instantané cohérent (également appelé capture instantanée ou une copie à un instant donné) des données à sauvegarder. Le cliché instantané peut être utilisé en l’état ou dans des scénarios comme les suivants :

  - Vous souhaitez sauvegarder des données d’application et des informations sur l’état du système, notamment archiver des données sur un autre disque dur, une bande ou tout autre support amovible.

  - Vous effectuez une exploration de données.

  - Vous effectuez des sauvegardes de disque à disque.

  - Vous avez besoin d’une récupération rapide suite à une perte de données en restaurant les données sur le numéro d’unité logique d’origine ou sur un nouveau numéro d’unité logique si celui d’origine est défaillant.


Les fonctionnalités et applications Windows qui utilisent VSS sont les suivantes :

  - [Sauvegarde Windows Server](https://go.microsoft.com/fwlink/?linkid=180891) (https://go.microsoft.com/fwlink/?LinkId=180891)

  - [Clichés instantanés de dossiers partagés](https://go.microsoft.com/fwlink/?linkid=142874) (https://go.microsoft.com/fwlink/?LinkId=142874)

  - [System Center Data Protection Manager](https://go.microsoft.com/fwlink/?linkid=180892) (https://go.microsoft.com/fwlink/?LinkId=180892)

  - [Restauration du système](https://go.microsoft.com/fwlink/?linkid=180893) (https://go.microsoft.com/fwlink/?LinkId=180893)


## <a name="how-volume-shadow-copy-service-works"></a>Fonctionnement du service VSS

Une solution VSS complète nécessite tous les composants de base suivants :

**Service VSS**   Partie du système d’exploitation Windows qui garantit que les autres composants peuvent communiquer entre eux correctement et fonctionner ensemble.

**Demandeur VSS**   Logiciel qui demande la création réelle des clichés instantanés (ou d’autres opérations générales comme leur importation ou suppression). Habituellement, il s’agit de l’application de sauvegarde. L’utilitaire Sauvegarde Windows Server et l’application System Center Data Protection Manager sont des demandeurs VSS. Les demandeurs VSS non-Microsoft® incluent presque tous les logiciels de sauvegarde qui s’exécutent sur Windows.

**Enregistreur VSS**   Composant qui garantit la cohérence du jeu de données à sauvegarder. Il est généralement fourni dans le cadre d’une application métier, par exemple SQL Server® ou Exchange Server. Les enregistreurs VSS des différents composants Windows, comme le Registre, sont fournis avec le système d’exploitation Windows. Les enregistreurs VSS non-Microsoft sont inclus dans de nombreuses applications pour Windows qui ont besoin de garantir la cohérence des données pendant la sauvegarde.

**Fournisseur VSS**   Composant qui crée et gère les clichés instantanés. Ce composant peut se produire dans le logiciel ou dans le matériel. Le système d’exploitation Windows inclut un fournisseur VSS qui utilise la copie sur écriture. Si vous utilisez un réseau de zone de stockage (SAN), il est important d’installer le fournisseur de matériel VSS du SAN, si celui-ci est fourni. Un fournisseur de matériel décharge le système d’exploitation hôte de la tâche de création et de maintenance d’un cliché instantané.

Le diagramme suivant illustre la façon dont le service VSS fonctionne avec les demandeurs, les enregistreurs et les fournisseurs pour créer un cliché instantané d’un volume.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figure 1**   Diagramme architectural du service VSS

### <a name="how-a-shadow-copy-is-created"></a>Comment un cliché instantané est créé

Cette section met en contexte les différents rôles du demandeur, de l’enregistreur et du fournisseur en listant les étapes nécessaires pour créer un cliché instantané. Le diagramme suivant montre comment le service VSS supervise la coordination globale entre le demandeur, l’enregistreur et le fournisseur.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figure 2** Processus de création de clichés instantanés

Pour créer un cliché instantané, le demandeur, l’enregistreur et le fournisseur effectuent les actions suivantes :

1.  Le demandeur demande au service VSS d’énumérer les enregistreurs, de collecter les métadonnées des enregistreurs et de préparer la création du cliché instantané.

2.  Chaque enregistreur crée une description XML des composants et des banques de données qui ont besoin d’être sauvegardés, puis la fournit au service VSS. L’enregistreur définit aussi une méthode de restauration, utilisée pour tous les composants. Le service VSS fournit la description de l’enregistreur au demandeur, qui sélectionne les composants à sauvegarder.

3.  Le service VSS notifie tous les enregistreurs pour qu’ils préparent leurs données à la création d’un cliché instantané.

4.  Chaque enregistreur prépare les données convenablement, notamment en terminant toutes les transactions ouvertes, en s’occupant des journaux des transactions et en vidant les caches. Lorsque les données sont prêtes à faire l’objet d’un cliché instantané, l’enregistreur notifie le service VSS.

5.  Le service VSS indique aux enregistreurs de geler temporairement les demandes d’E/S d’écriture d’application (les demandes d’E/S de lecture sont toujours possibles) pendant les quelques secondes nécessaires à la création du cliché instantané du ou des volumes. Le gel de l’application n’est pas autorisé à durer plus de 60 secondes. Le service VSS vide les mémoires tampons du système de fichiers, puis gèle ce dernier, pour garantir un enregistrement correct de ses métadonnées et l’écriture dans un ordre cohérent des données incluses dans le cliché instantané.

6.  Le service VSS indique au fournisseur de créer le cliché instantané. La création du cliché instantané ne dure pas plus de 10 secondes, pendant lesquelles toutes les demandes d’E/S d’écriture sur le système de fichiers restent gelées.

7.  Le service VSS libère les demandes d’E/S d’écriture dans le système de fichiers.

8.  VSS indique aux enregistreurs de dégeler les demandes d’E/S d’écriture d’application. À ce stade, les applications sont libres de reprendre l’écriture de données sur le disque concerné par le cliché instantané.


> [!NOTE]
> La création d’un cliché instantané peut être abandonnée si les enregistreurs sont maintenus dans un état de gel pendant plus de 60 secondes ou si les fournisseurs mettent plus de 10 secondes à valider le cliché instantané.
<br>

9. Le demandeur peut retenter le processus (revenir à l’étape 1) ou notifier l’administrateur quant à une nouvelle tentative ultérieure.

10. Si le cliché instantané est correctement créé, le service VSS retourne les informations d’emplacement du cliché instantané au demandeur. Dans certains cas, le cliché instantané est temporairement disponible en tant que volume en lecture-écriture pour permettre à VSS et d’autres applications d’en modifier le contenu avant qu’il ne soit terminé. Une fois que VSS et les applications ont apporté leurs modifications, le cliché instantané est mis en lecture seule. Cette phase est appelée récupération automatique. Elle permet d’annuler sur le volume de cliché instantané toutes les transactions de système de fichiers ou d’application qui ne se sont pas terminées avant la fin de la création du cliché instantané.


### <a name="how-the-provider-creates-a-shadow-copy"></a>Comment le fournisseur crée un cliché instantané

Un fournisseur de cliché instantané matériel ou logiciel utilise l’une des méthodes suivantes pour créer un cliché instantané :

**Copie complète**   Cette méthode permet d’obtenir une copie complète (appelée « copie entière » ou « clone ») du volume d’origine à un moment donné. Cette copie est en lecture seule.

**Copie sur écriture**   Cette méthode ne copie pas le volume d’origine. Elle permet plutôt d’obtenir une copie différentielle en copiant toutes les modifications (demandes d’E/S d’écriture terminées) apportées sur le volume après un moment donné.

**Redirection sur écriture**   Cette méthode ne copie pas le volume d’origine et n’apporte aucune modification au volume d’origine après un moment donné. Elle permet plutôt d’effectuer une copie différentielle en redirigeant toutes les modifications vers un volume différent.

## <a name="complete-copy"></a>Copie complète

Une copie complète est généralement créée à l’aide d’un « miroir divisé » comme suit :

1. Le volume d’origine et le volume de cliché instantané forment un ensemble de volumes en miroir.

2. Le volume de cliché instantané est séparé du volume d’origine. La connexion miroir est rompue.


Une fois la connexion miroir rompue, le volume d’origine et le volume de cliché instantané sont indépendants. Le volume d’origine continue à accepter toutes les modifications (demandes d’E/S d’écriture), tandis que le volume de cliché instantané reste une copie exacte en lecture seule des données d’origine au moment de la rupture.

### <a name="copy-on-write-method"></a>Méthode de copie sur écriture

Dans la méthode de copie sur écriture, quand une modification est apportée au volume d’origine (mais avant la fin de la demande d’E/S d’écriture), chaque bloc à modifier est lu, puis écrit dans la zone de stockage de cliché instantané du volume (également appelée « zone différentielle »). La zone de stockage de cliché instantané peut se trouver sur le même volume ou sur un volume différent. Ainsi, une copie du bloc de données est conservée sur le volume d’origine avant que la modification ne le remplace.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Heure</th>
<th>Données sources (état et données)</th>
<th>Cliché instantané (état et données)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Données d’origine : 1 2 3 4 5</p></td>
<td><p>Aucune copie</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Données modifiées dans le cache : 3 à 3’</p></td>
<td><p>Cliché instantané créé (différences uniquement) : 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Données d’origine remplacées : 1 2 3’ 4 5</p></td>
<td><p>Différences et index stockés sur le cliché instantané : 3</p></td>
</tr>
</tbody>
</table>

**Tableau 1**   Création de clichés instantanés avec la méthode de copie sur écriture

La méthode de copie sur écriture est une méthode rapide pour créer un cliché instantané, car elle copie uniquement les données qui ont changé. Les blocs copiés dans la zone différentielle peuvent être combinés avec les données modifiées sur le volume d’origine pour rétablir le volume à son état précédant l’apport des modifications. Si de nombreuses modifications sont apportées, la méthode de copie sur écriture peut s’avérer coûteuse.

### <a name="redirect-on-write-method"></a>Méthode de redirection sur écriture

Dans la méthode de redirection sur écriture, dès que le volume d’origine reçoit une modification (demande d’E/S d’écriture), cette modification n’est pas appliquée au volume d’origine. Elle est plutôt écrite dans la zone de stockage de cliché instantané d’un autre volume.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Heure</th>
<th>Données sources (état et données)</th>
<th>Cliché instantané (état et données)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Données d’origine : 1 2 3 4 5</p></td>
<td><p>Aucune copie</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Données modifiées dans le cache : 3 à 3’</p></td>
<td><p>Cliché instantané créé (différences uniquement) : 3’</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Données d’origine inchangées : 1 2 3 4 5</p></td>
<td><p>Différences et index stockés sur le cliché instantané : 3’</p></td>
</tr>
</tbody>
</table>

**Tableau 2**   Création de clichés instantanés avec la méthode de redirection sur écriture

À l’instar de la méthode de copie sur écriture, la méthode de redirection sur écriture est une méthode rapide pour créer un cliché instantané, car elle copie uniquement les modifications apportées aux données. Les blocs copiés dans la zone différentielle peuvent être combinés avec les données inchangées sur le volume d’origine pour créer une copie complète et à jour des données. Si le nombre de demandes d’E/S de lecture est élevé, la méthode de redirection sur écriture peut s’avérer coûteuse.

## <a name="shadow-copy-providers"></a>Fournisseurs de clichés instantanés

Il existe deux types de fournisseurs de clichés instantanés : les fournisseurs matériels et les fournisseurs logiciels. Il existe également un fournisseur système, qui correspond à un fournisseur logiciel intégré au système d’exploitation Windows.

### <a name="hardware-based-providers"></a>Fournisseurs matériels

Les fournisseurs de clichés instantanés matériels jouent le rôle d’interface entre le service VSS et le niveau matériel en travaillant conjointement avec un adaptateur ou contrôleur de stockage matériel. Le travail de création et de maintenance du cliché instantané est effectué par la baie de stockage.

Les fournisseurs matériels prennent toujours le cliché instantané d’un numéro d’unité logique entier, tandis que le service VSS expose uniquement le cliché instantané du ou des volumes demandés.

Un fournisseur matériel de clichés instantanés utilise les fonctionnalités du service VSS qui définissent le point dans le temps, autorisent la synchronisation des données, gèrent le cliché instantané et fournissent une interface commune avec des applications de sauvegarde. Toutefois, le service VSS ne spécifie pas le mécanisme sous-jacent de production et de maintenance des clichés instantanés par le fournisseur matériel.

### <a name="software-based-providers"></a>Fournisseurs logiciels

Les fournisseurs logiciels de clichés instantanés interceptent et traitent généralement les demandes d’E/S de lecture et d’écriture dans une couche logicielle située entre le système de fichiers et le logiciel du gestionnaire de volumes.

Ces fournisseurs sont implémentés en tant que composant DLL en mode utilisateur et au moins un pilote de périphérique en mode noyau, généralement un pilote de filtre de stockage. Contrairement aux fournisseurs matériels, les fournisseurs logiciels créent des clichés instantanés au niveau du logiciel, et non au niveau du matériel.

Un fournisseur logiciel de clichés instantanés doit conserver une vue « à un moment donné » d’un volume en ayant accès à un jeu de données utilisable pour recréer l’état du volume avant la création du cliché instantané. C’est le cas, par exemple, de la technique de copie sur écriture du fournisseur système. Toutefois, le service VSS n’impose aucune restriction quant à la technique utilisée par les fournisseurs logiciels pour créer et gérer les clichés instantanés.

Un fournisseur logiciel s’applique à plus de plateformes de stockage qu’un fournisseur matériel. Il peut fonctionner aussi bien avec des disques de base que des volumes logiques. (Un volume logique est un volume créé en combinant l’espace libre de deux disques ou plus.) Contrairement aux clichés instantanés matériels, les fournisseurs logiciels consomment des ressources du système d’exploitation pour tenir à jour le cliché instantané.

Pour plus d’informations sur les disques de base, consultez [Que sont les disques et les volumes de base ?](https://go.microsoft.com/fwlink/?linkid=180894) (https://go.microsoft.com/fwlink/?LinkId=180894) sur le site TechNet.

### <a name="system-provider"></a>Fournisseur système

Un seul fournisseur de clichés instantanés, le fournisseur système, est inclus dans le système d’exploitation Windows. Bien qu’un fournisseur par défaut soit fourni dans Windows, d’autres fournisseurs sont libres de fournir des implémentations optimisées pour leur matériel de stockage et leurs applications logicielles.

Pour conserver la vue « à un moment donné » d’un volume contenu dans un cliché instantané, le fournisseur système utilise une technique de copie sur écriture. Les copies des blocs qui ont été modifiés sur le volume depuis le début de la création du cliché instantané sont stockées dans une zone de stockage de cliché instantané.

Le fournisseur système peut exposer le volume de production, qui reste normalement inscriptible et lisible. Lorsque le cliché instantané est nécessaire, il applique logiquement les différences aux données situées sur le volume de production pour exposer le cliché instantané complet.

Pour le fournisseur système, la zone de stockage de cliché instantané doit se trouver sur un volume NTFS. Le volume qui fait l’objet du cliché instantané n’a pas besoin d’être un volume NTFS, mais au moins un volume monté sur le système doit en être un.

Les fichiers de composants qui composent le fournisseur système sont swprv.dll et volsnap.sys.

### <a name="in-box-vss-writers"></a>Enregistreurs VSS intégrés

Le système d’exploitation Windows comprend un ensemble d’enregistreurs VSS chargés d’énumérer les données nécessaires aux différentes fonctionnalités Windows.

Pour plus d’informations sur ces enregistreurs, consultez les pages web Microsoft Docs suivantes :

- [Enregistreurs VSS intégrés](https://docs.microsoft.com/windows/win32/vss/in-box-vss-writers) (https://docs.microsoft.com/windows/win32/vss/in-box-vss-writers)


## <a name="how-shadow-copies-are-used"></a>Comment les clichés instantanés sont utilisés

Outre la sauvegarde des données d’application et des informations sur l’état du système, les clichés instantanés servent à plusieurs choses, notamment les suivantes :

  - Restauration des numéros d’unités logiques (resynchronisation et échange de numéro d’unité logique)

  - Restauration de fichiers individuels (clichés instantanés pour dossiers partagés)

  - Exploration de données à l’aide de clichés instantanés transportables


### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Restauration des numéros d’unités logiques (resynchronisation et échange de numéro d’unité logique)

Dans Windows Server 2008 R2 et Windows 7, les demandeurs VSS peuvent utiliser une fonctionnalité de fournisseur matériel de clichés instantanés appelée resynchronisation de numéro d’unité logique. Il s’agit d’un schéma de récupération rapide qui permet à un administrateur d’application de restaurer des données à partir d’un cliché instantané vers le numéro d’unité logique d’origine ou vers un nouveau numéro d’unité logique.

Le cliché instantané peut être un clone complet ou un cliché instantané différentiel. Dans les deux cas, à la fin de l’opération de resynchronisation, le numéro d’unité logique de destination présente le même contenu que le numéro d’unité logique du cliché instantané. Pendant l’opération de resynchronisation, la baie effectue une copie au niveau du bloc du cliché instantané vers le numéro d’unité logique de destination.


> [!NOTE]
> Le cliché instantané doit être un cliché instantané matériel transférable.
<br>


La plupart des baies permettent aux opérations d’E/S de production de reprendre peu après le début de l’opération de resynchronisation. Pendant que l’opération de resynchronisation est en cours, les demandes de lecture sont redirigées vers le numéro d’unité logique du cliché instantané et les demandes d’écriture vers le numéro d’unité logique de destination. Cela permet aux baies de récupérer des jeux de données très volumineux et de reprendre les opérations normales en quelques secondes.

La resynchronisation de numéro d’unité logique diffère de l’échange de numéro d’unité logique. Un échange correspond à un scénario de récupération rapide pris en charge par VSS depuis Windows Server 2003 SP1. Dans un échange, le cliché instantané est importé, puis converti en volume en lecture-écriture. La conversion est une opération irréversible. Le volume et le numéro d’unité logique sous-jacent ne peuvent pas être contrôlés avec les API VSS par la suite. La liste suivante compare la resynchronisation et l’échange de numéro d’unité logique :

  - Dans la resynchronisation, le cliché instantané n’est pas modifié et peut donc être utilisé plusieurs fois. Dans le cas de l’échange, le cliché instantané est utilisable une seule fois pour une récupération. Cette différence est importante pour les administrateurs les plus attentifs à la sécurité. Quand la resynchronisation est utilisée, le demandeur peut retenter toute l’opération de restauration en cas de problème lors de la première fois.

  - À la fin d’un échange, le numéro d’unité logique du cliché instantané est utilisé pour les demandes d’E/S de production. C’est pourquoi le numéro d’unité logique du cliché instantané doit utiliser la même qualité de stockage que le numéro d’unité logique de production d’origine pour veiller à ce que les performances ne soient pas impactées après l’opération de récupération. Si la resynchronisation de numéro d’unité logique est utilisée à la place, le fournisseur matériel peut conserver le cliché instantané sur un stockage moins onéreux que celui de qualité production.

  - Si le numéro d’unité logique de destination est inutilisable et a besoin d’être recréé, l’échange de numéro d’unité logique peut être plus économique, car il ne nécessite pas de numéro d’unité logique de destination.


> [!WARNING]
> Toutes les opérations listées sont des opérations au niveau du numéro d’unité logique. Si vous tentez de récupérer un volume spécifique par une resynchronisation de numéro d’unité logique, vous allez involontairement restaurer tous les autres volumes qui partagent le numéro d’unité logique.
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Restauration de fichiers individuels (clichés instantanés pour dossiers partagés)

Les clichés instantanés pour dossiers partagés utilisent le service VSS pour fournir des copies à un moment donné de fichiers situés sur une ressource réseau partagée, comme un serveur de fichiers. Avec les clichés instantanés pour dossiers partagés, les utilisateurs peuvent récupérer rapidement des fichiers supprimés ou modifiés, stockés sur le réseau. Étant donné qu’ils peuvent le faire sans avoir besoin de l’aide de l’administrateur, les clichés instantanés pour dossiers partagés peuvent augmenter la productivité et réduire les coûts d’administration.

Pour plus d’informations sur les clichés instantanés pour dossiers partagés, consultez [Clichés instantanés pour dossiers partagés](https://go.microsoft.com/fwlink/?linkid=180898) (https://go.microsoft.com/fwlink/?LinkId=180898) sur le site TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Exploration de données à l’aide de clichés instantanés transportables

Avec un fournisseur matériel conçu pour être utilisé avec le service VSS, vous pouvez créer des clichés instantanés transportables que vous pouvez importer sur des serveurs au sein du même sous-système (par exemple, un réseau de zone de stockage). Ces clichés instantanés peuvent être utilisés pour amorcer une installation de production ou de test avec des données en lecture seule à des fins d’exploration de données.

Avec le service VSS et une baie de stockage dotée d’un fournisseur matériel conçu pour être utilisé avec le service VSS, il est possible de créer un cliché instantané du volume de données sources sur un seul serveur, puis d’importer ce cliché instantané sur un autre serveur (ou de le réimporter sur le même serveur). Ce processus s’effectue en quelques minutes, quelle que soit la taille des données. Le processus de transport s’effectue par la biais d’une série d’étapes qui utilisent un demandeur de cliché instantané (une application de gestion de stockage) qui prend en charge les clichés instantanés transportables.

## <a name="to-transport-a-shadow-copy"></a>Pour transporter un cliché instantané

1.  Créez un cliché instantané transportable des données sources sur un serveur.

2.  Importez le cliché instantané sur un serveur connecté au réseau de zone de stockage (vous pouvez l’importer sur un autre serveur ou sur le même serveur).

3.  Les données sont maintenant prêtes à être utilisées.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figure 3**   Création d’un cliché instantané et transport entre deux serveurs


> [!NOTE]
> Un cliché instantané transportable créé sur Windows Server 2003 ne peut pas être importé sur un serveur qui exécute Windows Server 2008 ou Windows Server 2008 R2. Un cliché instantané transportable créé sur Windows Server 2008 ou Windows Server 2008 R2 ne peut pas être importé sur un serveur qui exécute Windows Server 2003. Toutefois, un cliché instantané créé sur Windows Server 2008 peut être importé sur un serveur qui exécute Windows Server 2008 R2 et vice versa.
<br>


Les clichés instantanés sont en lecture seule. Si vous souhaitez convertir un cliché instantané en numéro d’unité logique en lecture/écriture, vous pouvez utiliser une application de gestion de stockage VDS (Virtual Disk Service) (qui inclut quelques demandeurs) en plus du service VSS. En effet, cette application vous permet de supprimer le cliché instantané de la gestion du service VSS et de le convertir en numéro d’unité logique en lecture/écriture.

Le transport du service VSS est une solution avancée sur les ordinateurs exécutant Windows Server 2003 Enterprise Edition, Windows Server 2003 Datacenter Edition, Windows Server 2008 ou Windows Server 2008 R2. Il fonctionne uniquement s’il existe un fournisseur matériel sur la baie de stockage. Le transport de clichés instantanés peut être utilisé dans plusieurs buts, notamment à des fins de sauvegarde sur bande, d’exploration de données et de test.

## <a name="frequently-asked-questions"></a>Forum Aux Questions

Ces questions fréquentes (FAQ) portent sur le service VSS pour les administrateurs système. Pour plus d’informations sur les interfaces de programmation d’applications VSS, consultez [Service VSS](https://go.microsoft.com/fwlink/?linkid=180899) (https://go.microsoft.com/fwlink/?LinkId=180899) dans la bibliothèque du centre de développement Windows.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>Quand le service VSS a-t-il été introduit ? Sur quelles versions du système d’exploitation Windows est-il disponible ?

VSS a été introduit dans Windows XP. Il est disponible sur Windows XP, Windows Server 2003, Windows Vista®, Windows Server 2008, Windows 7 et Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>Quelle est la différence entre un cliché instantané et une sauvegarde ?

Dans le cas d’une sauvegarde de disque dur, le cliché instantané créé est également la sauvegarde. Les données peuvent être copiées hors du cliché instantané à des fins de restauration ou le cliché instantané peut servir à un scénario de récupération rapide (par exemple, resynchronisation ou échange de numéro d’unité logique).

Quand les données sont copiées du cliché instantané sur une bande ou tout autre support amovible, le contenu stocké sur le support constitue la sauvegarde. Le cliché instantané lui-même peut être supprimé une fois que les données qu’il contient ont été copiées.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>Quelle est la taille de volume la plus grande prise en charge par le service VSS ?

Le service VSS prend en charge une taille de volume allant jusqu’à 64 To.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>J’ai effectué une sauvegarde sur Windows Server 2008. Puis-je la restaurer sur Windows Server 2008 R2 ?

Cela dépend du logiciel de sauvegarde que vous avez utilisé. La plupart des programmes de sauvegarde prennent en charge ce scénario pour les données, mais pas pour les sauvegardes de l’état du système.

Les clichés instantanés créés sur l’une de ces deux versions de Windows peuvent être utilisés sur l’autre.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>J’ai effectué une sauvegarde sur Windows Server 2003. Puis-je la restaurer sur Windows Server 2008 ?

Cela dépend du logiciel de sauvegarde que vous avez utilisé. Si vous créez un cliché instantané sur Windows Server 2003, vous ne pouvez pas l’utiliser sur Windows Server 2008. De plus, si vous créez un cliché instantané sur Windows Server 2008, vous ne pouvez pas le restaurer sur Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>Comment puis-je désactiver le service VSS ?

Il est possible de désactiver le service VSS à l’aide de la console MMC (Microsoft Management Console). Toutefois, ce n’est pas souhaitable. Désactiver le service VSS affecte tous les logiciels que vous utilisez qui en dépendent, notamment Restauration du système et Sauvegarde Windows Server.

Pour plus d’informations, consultez les sites web Microsoft TechNet suivants :

- [Restauration du système](https://go.microsoft.com/fwlink/?linkid=157113) (https://go.microsoft.com/fwlink/?LinkID=157113)

- [Sauvegarde Windows Server](https://go.microsoft.com/fwlink/?linkid=180891) (https://go.microsoft.com/fwlink/?LinkID=180891)


### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>Puis-je exclure des fichiers d’un cliché instantané pour économiser de l’espace ?

Le service VSS est conçu pour créer des clichés instantanés de volumes entiers. Les fichiers temporaires, comme les fichiers d’échange, sont automatiquement omis des clichés instantanés pour économiser de l’espace.

Pour exclure des fichiers spécifiques des clichés instantanés, utilisez la clé de Registre suivante : **FilesNotToSnapshot**.


> [!NOTE]
> La clé de Registre <STRONG>FilesNotToSnapshot</STRONG> est destinée à être utilisée uniquement par les applications. Les utilisateurs qui essaient de l’utiliser rencontrent les limitations suivantes :
> <br>
> <UL>
> <LI>Elle ne permet pas de supprimer des fichiers d’un cliché instantané créé sur un serveur Windows Server à l’aide de la fonctionnalité Versions précédentes.<BR><BR>
> <LI>Elle ne permet pas de supprimer des fichiers des clichés instantanés pour dossiers partagés.<BR><BR>
> <LI>Elle permet de supprimer des fichiers d’un cliché instantané créé à l’aide de l’utilitaire <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">Diskshadow</a>, mais pas de supprimer des fichiers d’un cliché instantané créé à l’aide de l’utilitaire <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">Vssadmin</a>.<BR><BR>
> <LI>Les fichiers sont supprimés d’un cliché instantané autant que faire se peut. Cela signifie que leur suppression n’est pas garantie.<BR><BR></LI></UL>


Pour plus d’informations, consultez [Exclusion de fichiers dans les clichés instantanés](https://go.microsoft.com/fwlink/?linkid=180904) (https://go.microsoft.com/fwlink/?LinkId=180904) sur MSDN.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Mon programme de sauvegarde non-Microsoft a échoué avec une erreur VSS. Que puis-je faire ?

Consultez la section du support technique sur le site web de l’entreprise qui a créé le programme de sauvegarde. Une mise à jour du produit est peut-être disponible que vous pouvez télécharger et installer pour résoudre le problème. Dans le cas contraire, contactez le service du support technique de l’entreprise.

Les administrateurs système peuvent utiliser les informations de résolution des problèmes liés au service VSS sur le site web de la bibliothèque Microsoft TechNet suivante pour collecter des informations de diagnostic sur ces problèmes.

Pour plus d’informations, consultez [Service VSS](https://go.microsoft.com/fwlink/?linkid=180905) (https://go.microsoft.com/fwlink/?LinkId=180905) sur TechNet.

### <a name="what-is-the-diff-area"></a>Qu’est-ce que la « zone différentielle » ?

La zone de stockage de cliché instantané (ou « zone différentielle ») est l’emplacement où sont stockées les données du cliché instantané créé par le fournisseur logiciel système.

### <a name="where-is-the-diff-area-located"></a>Où se trouve la zone différentielle ?

La zone différentielle peut se trouver sur n’importe quel volume local. Toutefois, elle doit se situer sur un volume NTFS qui dispose de suffisamment d’espace pour la stocker.

### <a name="how-is-the-diff-area-location-determined"></a>Comment l’emplacement de la zone différentielle est-il déterminé ?

Les critères suivants sont évalués, dans cet ordre, pour déterminer l’emplacement de la zone différentielle :

  - Si un volume a déjà un cliché instantané existant, alors cet emplacement est utilisé.

  - S’il existe une association manuelle préconfigurée entre l’emplacement du volume d’origine et celui du volume de cliché instantané, alors cet emplacement est utilisé.

  - Si les deux critères précédents ne fournissent pas d’emplacement, le service VSS choisit un emplacement en fonction de l’espace libre disponible. Si plusieurs volumes font l’objet d’un cliché instantané, le service VSS crée la liste des emplacements possibles en fonction de la taille de l’espace libre, dans l’ordre décroissant. Le nombre d’emplacements fournis équivaut au nombre de volumes faisant l’objet d’un cliché instantané.

  - Si le volume faisant l’objet d’un cliché instantané correspond à l’un des emplacements possibles, alors une association locale est créée. Sinon, une association avec le volume présentant le plus d’espace disponible est créée.


### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>Le service VSS peut-il créer des clichés instantanés de volumes non-NTFS ?

Oui. Toutefois, des clichés instantanés persistants peuvent être créés uniquement pour des volumes NTFS. De plus, au moins un volume monté sur le système doit être un volume NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>Quel est le nombre maximal de clichés instantanés que je peux créer en même temps ?

Le nombre maximal de volumes copiés dans un seul jeu de clichés instantanés s’élève à 64. Notez que ce nombre n’est pas le même que le nombre de clichés instantanés.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>Quel est le nombre maximal de clichés instantanés logiciels créés par le fournisseur système que je peux conserver pour un volume ?

Le nombre maximal de clichés instantanés logiciels pour chaque volume s’élève à 512. Toutefois, par défaut, vous ne pouvez conserver que 64 clichés instantanés utilisés par la fonctionnalité Clichés instantanés de dossiers partagés. Pour modifier la limite de la fonctionnalité Clichés instantanés de dossiers partagés, utilisez la clé de Registre suivante : **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>Comment puis-je contrôler l’espace utilisé pour l’espace de stockage des clichés instantanés ?

Tapez la commande **vssadmin resize shadowstorage**.

Pour plus d’informations, consultez [vssadmin resize shadowstorage](https://go.microsoft.com/fwlink/?linkid=180906) (https://go.microsoft.com/fwlink/?LinkId=180906) sur TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>Que se passe-t-il en cas de manque d’espace ?

Les clichés instantanés du volume sont supprimés, en commençant par le plus ancien.

## <a name="volume-shadow-copy-service-tools"></a>Outils du service VSS

Le système d’exploitation Windows fournit les outils suivants pour utiliser le VSS :

  - [DiskShadow](https://go.microsoft.com/fwlink/?linkid=180907) (https://go.microsoft.com/fwlink/?LinkId=180907)

  - [VssAdmin](https://go.microsoft.com/fwlink/?linkid=84008) (https://go.microsoft.com/fwlink/?LinkId=84008)


### <a name="diskshadow"></a>DiskShadow

DiskShadow est un demandeur VSS que vous pouvez utiliser pour gérer tous les clichés instantanés matériels et logiciels que vous pouvez avoir sur un système. DiskShadow comprend des commandes comme les suivantes :

  - **list** : Liste les enregistreurs VSS, les fournisseurs VSS et les clichés instantanés.

  - **create** : Crée un cliché instantané.

  - **import** : Importe un cliché instantané transportable.

  - **expose** : Expose un cliché instantané persistant (sous la forme d’une lettre de lecteur, par exemple).

  - **revert** : Rétablit un volume à un cliché instantané spécifié.


Cet outil est destiné aux professionnels de l’informatique, mais les développeurs peuvent également le trouver utile pour tester un enregistreur VSS ou un fournisseur VSS.

DiskShadow est disponible uniquement sur les systèmes d’exploitation Windows Server. Il n’est pas disponible sur les systèmes d’exploitation clients Windows.

### <a name="vssadmin"></a>VssAdmin

VssAdmin est utilisé pour créer, supprimer et lister des informations sur les clichés instantanés. Il permet aussi de redimensionner la zone de stockage de cliché instantané (« zone différentielle »).

VssAdmin inclut des commandes comme les suivantes :

  - **create shadow** : Crée un cliché instantané.

  - **delete shadows** : Supprime des clichés instantanés.

  - **list providers** : Liste tous les fournisseurs VSS inscrits.

  - **list writers** : Liste tous les enregistreurs VSS abonnés.

  - **resize shadowstorage** : Modifie la taille maximale de la zone de stockage de cliché instantané.


VssAdmin peut uniquement être utilisé pour administrer des clichés instantanés créés par le fournisseur logiciel système.

VssAdmin est disponible sur les versions du système d’exploitation Windows Server et client Windows.

## <a name="volume-shadow-copy-service-registry-keys"></a>Clés de Registre du service VSS

Les clés de Registre suivantes peuvent être utilisées avec le service VSS :

  - **VssAccessControl**

  - **MaxShadowCopies**

  - **MinDiffAreaFileSize**


### <a name="vssaccesscontrol"></a>VssAccessControl

Cette clé est utilisée pour spécifier les utilisateurs qui ont accès aux clichés instantanés.

Pour plus d’informations, consultez les entrées suivantes sur le site web MSDN :

  - [Considérations relatives à la sécurité pour les enregistreurs](https://go.microsoft.com/fwlink/?linkid=157739) (https://go.microsoft.com/fwlink/?LinkId=157739)

  - [Considérations relatives à la sécurité pour les demandeurs](https://go.microsoft.com/fwlink/?linkid=180908) (https://go.microsoft.com/fwlink/?LinkId=180908)


### <a name="maxshadowcopies"></a>MaxShadowCopies

Cette clé spécifie le nombre maximal de clichés instantanés accessibles par les clients pouvant être stockés sur chaque volume de l’ordinateur. Les clichés instantanés accessibles par les clients sont utilisés par les clichés instantanés pour dossiers partagés.

Pour plus d’informations, consultez l’entrée suivante sur le site web MSDN :

**MaxShadowCopies** sous [Clés de Registre pour la sauvegarde et la restauration](https://go.microsoft.com/fwlink/?linkid=180909) (https://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Cette clé spécifie la taille initiale minimale, en Mo, de la zone de stockage de cliché instantané.

Pour plus d’informations, consultez l’entrée suivante sur le site web MSDN :

**MinDiffAreaFileSize** sous [Clés de Registre pour la sauvegarde et la restauration](https://go.microsoft.com/fwlink/?linkid=180910) (https://go.microsoft.com/fwlink/?LinkId=180910)

### <a name="supported-operating-system-versions"></a>Versions de système d’exploitation prises en charge

Le tableau suivant liste les versions de système d’exploitation minimales prises en charge pour les fonctionnalités du service VSS.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Fonctionnalité du service VSS</th>
<th>Client minimal pris en charge</th>
<th>Serveur minimal pris en charge</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Resynchronisation de numéro d’unité logique</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p>Clé de Registre <strong>FilesNotToSnapshot</strong></p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2008</p></td>
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
<td><p>Versions antérieures de Windows Server</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="even">
<td><p>Récupération rapide par échange de numéro d’unité logique</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2003 avec SP1</p></td>
</tr>
<tr class="odd">
<td><p>Importations multiples de clichés instantanés matériels</p>
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
<td>Il s’agit de la possibilité d’importer un cliché instantané plusieurs fois. Une seule opération d’importation peut être effectuée à la fois.
<p></p></td>
</tr>
</tbody>
</table>
<p></p>
</div></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Clichés instantanés pour dossiers partagés</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Clichés instantanés transportables à récupération automatique</p></td>
<td><p>Aucun pris en charge</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sessions de sauvegarde simultanées (jusqu’à 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Session de restauration unique simultanée avec des sauvegardes</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 avec SP2</p></td>
</tr>
<tr class="even">
<td><p>Jusqu’à 8 sessions de restauration simultanées avec des sauvegardes</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Voir aussi

[Service VSS dans le centre de développement Windows](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)
