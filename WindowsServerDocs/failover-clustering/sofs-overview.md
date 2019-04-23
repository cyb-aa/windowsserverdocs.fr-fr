---
title: Vue d’ensemble d’un serveur de fichiers avec montée en puissance parallèle pour les données d’application
description: Vue d’ensemble de la fonctionnalité de serveur de fichiers avec montée en puissance pour Windows Server R2 201, Windows Server 2012 et Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 04e25e9c69062611d9d14c220614f148ac5de770
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884770"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>Vue d’ensemble d’un serveur de fichiers avec montée en puissance parallèle pour les données d’application

>S’applique à : Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Le serveur de fichiers avec montée en puissance parallèle est une fonctionnalité conçue pour fournir des partages de fichiers avec montée en puissance parallèle disponibles de manière continue pour le stockage des applications de serveur basées sur des fichiers. Les partages de fichiers avec montée en puissance parallèle permettent de partager le même dossier à partir de plusieurs nœuds d’un même cluster. Ce scénario se concentre sur la façon de planifier et de déployer des serveur de fichiers avec montée en puissance parallèle.

Vous pouvez déployer et configurer un serveur de fichiers en cluster à l’aide d’une des méthodes suivantes :

- **Serveur de fichiers avec montée en puissance pour les données d’application** cette fonctionnalité de serveur de fichiers en cluster a été introduite dans Windows Server 2012, et vous permet de stocker des données d’application de serveur, tels que les fichiers de machine virtuelle Hyper-V sur des partages de fichiers et obtenir un niveau similaire de fiabilité, disponibilité, la facilité de gestion et hautes performances, attendez-vous à partir d’un réseau de stockage. Tous les partages de fichiers sont simultanément en ligne sur tous les nœuds. Les partages de fichiers associés à ce type de serveur de fichiers en cluster sont appelés partages de fichiers avec montée en puissance parallèle. Ils sont parfois qualifiés d’actif-actif. Il s’agit du type de serveur de fichiers recommandé lors du déploiement d’Hyper-V sur SMB (Server Message Block) ou Microsoft SQL Server.
- **Serveur de fichiers pour une utilisation générale** Ceci est la continuation du serveur de fichiers en cluster qui était pris en charge dans Windows Server depuis l’introduction du clustering avec basculement. Ce type de serveur de fichiers en cluster, et donc tous les partages associés au serveur de fichiers en cluster, sont en ligne sur un seul nœud à la fois. Ils sont parfois qualifiés d’actif-passif ou d’actif double. Les partages de fichiers associés à ce type de serveur de fichiers en cluster sont appelés partages de fichiers en cluster. Il s’agit du type de serveur de fichiers recommandé lors du déploiement de scénarios de travailleur de l’information.

## <a name="scenario-description"></a>Description du scénario

Les partages de fichiers avec montée en puissance parallèle permettent de partager le même dossier à partir de plusieurs nœuds d’un cluster. Par exemple, si vous avez un cluster de serveurs de fichiers à quatre nœuds qui est à l’aide de Server Message Block (SMB) Scale-Out, un ordinateur exécutant Windows Server 2012 R2 ou Windows Server 2012 peut accéder aux partages de fichiers à partir d’un des quatre nœuds. Cela est possible en tirant profit des nouvelles fonctionnalités du clustering avec basculement de Windows Server et des fonctionnalités de la version du protocole de serveur de fichiers Windows, SMB 3.0. Les administrateurs de serveur de fichiers peuvent fournir des partages de fichiers avec montée en puissance parallèle et des services de fichier disponibles en continu aux applications de serveur et répondent rapidement aux demandes croissantes en mettant simplement en ligne plus de serveurs. Tout ceci est possible dans un environnement de production et cela est complètement transparent pour l’application serveur.

Le serveur de fichiers avec montée en puissance parallèle fournit les avantages clés suivants :

- **Partages de fichiers actif-actif**. Tous les nœuds de cluster peuvent accepter et traiter les demandes du client SMB. En rendant accessible le contenu des partages de fichiers via tous les nœuds du cluster simultanément, les clients et clusters SMB 3.0 coopèrent pour fournir un basculement transparent vers d’autres nœuds de cluster au cours de la maintenance planifiée et des défaillances non planifiées avec interruption de service.
- **Bande passante augmentée**. La bande passante maximale de partage est la bande passante totale de tous les nœuds de cluster de serveurs de fichiers. À la différence des versions précédentes de Windows Server, la bande passante totale n’est plus limitée à la bande passante d’un seul nœud du cluster : c’est la capacité du système de stockage de secours qui définit les contraintes. Vous pouvez augmenter la bande passante totale en ajoutant des nœuds.
- **CHKDSK avec temps d’interruption nul**. CHKDSK dans Windows Server 2012 a été nettement amélioré pour raccourcir considérablement le temps de qu'un système de fichiers est hors connexion pour réparation. Les volumes partagés de cluster (CSV) vont encore plus loin en éliminant la phase hors connexion. Un système de fichiers CSV (CSVFS) peut utiliser CHKDSK sans affecter les applications avec des handles ouverts sur le système de fichiers.
- **Cache de Volume partagé de cluster**. Volumes partagés de cluster dans Windows Server 2012 introduit la prise en charge pour un cache de lecture, ce qui peut améliorer considérablement les performances dans certains scénarios, comme dans Virtual Desktop Infrastructure (VDI).
- **Gestion simplifiée**. Avec le serveur de fichiers avec montée en puissance, vous créez les serveurs de fichiers de montée en puissance, puis ajoutez les volumes partagés de cluster nécessaires et les partages de fichiers. Il n’est plus nécessaire de créer plusieurs serveurs de fichiers en cluster, chacun avec des disques de cluster distincts, puis de développer des stratégies de positionnement pour garantir l’activité sur chaque nœud du cluster.
- **Rééquilibrage automatique des clients de serveur de fichiers avec montée en puissance**. Dans Windows Server 2012 R2, le rééquilibrage automatique améliore l’extensibilité et la facilité de gestion des serveurs de fichiers de montée en puissance. Les connexions client SMB sont suivies par partage de fichier (plutôt que par serveur) et les clients sont redirigés vers le nœud de cluster qui propose le meilleur accès au volume utilisé par le partage de fichiers. Cela améliore l’efficacité en réduisant le trafic entre les nœuds du serveur de fichiers. Les clients sont redirigés suite à une connexion initiale et quand le stockage en cluster est configuré.

## <a name="in-this-scenario"></a>Dans ce scénario

Les rubriques suivantes sont disponibles pour vous aider à déployer un serveur de fichiers avec montée en puissance parallèle :

- [Plan pour le serveur de fichiers avec montée en puissance](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [Étape 1 : Planifier le stockage dans le serveur de fichiers avec montée en puissance](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [Étape 2 : Plan for Networking in Scale-Out File Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [Déployer le serveur de fichiers avec montée en puissance](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [Étape 1 : Installez les composants requis pour le serveur de fichiers avec montée en puissance](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [Étape 2 : Configurer le serveur de fichiers avec montée en puissance](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [Étape 3 : Configurer Hyper-V pour utiliser le serveur de fichiers avec montée en puissance](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [Étape 4 : Configurer Microsoft SQL Server pour utiliser le serveur de fichiers avec montée en puissance](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>Quand utiliser un serveur de fichiers avec montée en puissance parallèle

Vous ne devez pas utiliser un serveur de fichiers avec montée en puissance parallèle si votre charge de travail génère un nombre élevé d’opérations de métadonnées, telles que l’ouverture de fichiers, la fermeture de fichiers, la création de nouveaux fichiers ou le changement de nom des fichiers existants. Un travailleur de l’information standard peut générer un grand nombre d’opérations de métadonnées. Vous devez utiliser un serveur de fichiers avec montée en puissance parallèle si vous êtes intéressé par l’extensibilité et la simplicité qu’il offre et si vous avez seulement besoin des technologies prises en charge avec le serveur de fichiers avec montée en puissance parallèle.

Le tableau suivant répertorie les fonctionnalités de SMB 3.0, les systèmes de fichiers Windows courants, les technologies de gestion des données du serveur de fichiers et les charges de travail courantes. Vous pouvez voir si la technologie est pris en charge par le serveur de fichiers avec montée en puissance parallèle, ou si elle nécessite un serveur de fichiers en cluster traditionnel (également appelé serveur de fichiers pour une utilisation générale).

<table>
<thead>
<tr class="header">
<th>Domaine technologique</th>
<th>Fonctionnalité</th>
<th>Cluster de serveurs de fichiers pour une utilisation générale</th>
<th>Serveur de fichiers avec montée en puissance parallèle</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>SMB</td>
<td>Disponibilité continue SMB</td>
<td>Oui</td>
<td>Oui</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>SMB Multichannel</td>
<td>Oui</td>
<td>Oui</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>SMB Direct</td>
<td>Oui</td>
<td>Oui</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>Chiffrement SMB</td>
<td>Oui</td>
<td>Oui</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>Basculement transparent SMB</td>
<td>Oui (si la disponibilité continue est activée)</td>
<td>Oui</td>
</tr>
<tr class="even">
<td>Système de fichiers</td>
<td>NTFS</td>
<td>Oui</td>
<td>N/A</td>
</tr>
<tr class="odd">
<td>Système de fichiers</td>
<td>Système de fichiers résilient (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>Recommandé avec stockage espaces Direct</td>
<td>Recommandé avec stockage espaces Direct</td>
</tr>
<tr class="even">
<td>Système de fichiers</td>
<td>Système de fichier de volume partagé de cluster (CSV)</td>
<td>N/A</td>
<td>Oui</td>
</tr>
<tr class="odd">
<td>Gestion des fichiers</td>
<td>BranchCache</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="even">
<td>Gestion des fichiers</td>
<td>Déduplication des données (Windows Server 2012)</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="odd">
<td>Gestion des fichiers</td>
<td>Déduplication des données (Windows Server 2012 R2)</td>
<td>Oui</td>
<td>Oui (VDI uniquement)</td>
</tr>
<tr class="even">
<td>Gestion des fichiers</td>
<td>Racine du serveur racine d’espace de noms DFS (DFSN)</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="odd">
<td>Gestion des fichiers</td>
<td>Serveur cible du dossier d’espace de noms DFS (DFSN)</td>
<td>Oui</td>
<td>Oui</td>
</tr>
<tr class="even">
<td>Gestion des fichiers</td>
<td>Réplication DFS (DFSR)</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="odd">
<td>Gestion des fichiers</td>
<td>Gestionnaire de ressources du serveur de fichiers (Écrans et quotas)</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="even">
<td>Gestion des fichiers</td>
<td>Infrastructure de classification des fichiers</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="odd">
<td>Gestion des fichiers</td>
<td>Contrôle d’accès dynamique (accès à base de revendications, CAP)</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="even">
<td>Gestion des fichiers</td>
<td>Redirection de dossiers</td>
<td>Oui</td>
<td>Non recommandé*</td>
</tr>
<tr class="odd">
<td>Gestion des fichiers</td>
<td>Fichiers hors connexion (cache côté client)</td>
<td>Oui</td>
<td>Non recommandé*</td>
</tr>
<tr class="even">
<td>Gestion des fichiers</td>
<td>Profils utilisateur itinérants</td>
<td>Oui</td>
<td>Non recommandé*</td>
</tr>
<tr class="odd">
<td>Gestion des fichiers</td>
<td>Répertoires de base</td>
<td>Oui</td>
<td>Non recommandé*</td>
</tr>
<tr class="even">
<td>Gestion des fichiers</td>
<td>Dossiers de travail</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="odd">
<td>NFS</td>
<td>Serveur NFS</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="even">
<td>Applications</td>
<td>Hyper-V</td>
<td>Non recommandé</td>
<td>Oui</td>
</tr>
<tr class="odd">
<td>Applications</td>
<td>Microsoft SQL Server</td>
<td>Non recommandé</td>
<td>Oui</td>
</tr>
</tbody>
</table>

\* La Redirection de dossiers, fichiers hors connexion, les profils utilisateur itinérants ou répertoires de base génèrent un grand nombre d’écritures qui doivent être écrites immédiatement sur le disque (sans mise en mémoire tampon) lorsque vous utilisez des partages de fichiers disponibles en permanence, ce qui réduit les performances par rapport à partages de fichiers à usage général. Les partages de fichiers disponibles en permanence sont également incompatibles avec les outils de gestion de ressources pour serveur de fichiers et les PC exécutant Windows XP. En outre, la fonctionnalité Fichiers hors connexion ne peut pas passer en mode hors connexion durant un délai compris entre 3 et 6 minutes après qu’un utilisateur perd l’accès à un partage, ce qui peut nuire aux utilisateurs qui n’utilisent pas le mode Toujours hors connexion de la fonctionnalité Fichiers hors connexion.

## <a name="practical-applications"></a>Cas pratiques

Les serveurs de fichiers avec montée en puissance parallèle sont idéaux pour le stockage d’applications serveur. Voici quelques exemples d’applications serveur qui peuvent stocker leurs données sur un partage de fichiers avec montée en puissance parallèle :

- Le serveur web IIS (Internet Information Services) peut stocker la configuration et les données des sites web sur un partage de fichiers avec montée en puissance parallèle. Pour plus d’informations, voir [Configuration partagée](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264).
- Hyper-V peut stocker la configuration et les disques virtuels en direct sur un partage de fichiers avec montée en puissance parallèle. Pour plus d’informations, voir [Déployer Hyper-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).
- SQL Server permet de stocker des fichiers de base de données en direct sur des partages de fichiers avec montée en puissance parallèle. Pour plus d’informations, voir [Installer SQL Server avec le partage de fichiers SMB en tant qu’option de stockage](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option).
- Virtual Machine Manager (VMM) peut stocker un partage de bibliothèque (contenant des modèles d’ordinateurs virtuels et leurs fichiers associés) sur un partage de fichiers avec montée en puissance parallèle. Toutefois, le serveur de bibliothèque lui-même ne peut pas être un serveur de fichiers avec montée en puissance, il doit se trouver sur un serveur autonome ou un cluster de basculement qui n’utilise pas le rôle de cluster de serveur de fichiers avec montée en puissance.

Si vous utilisez un partage de fichiers avec montée en puissance parallèle en tant que partage de bibliothèque, vous pouvez utiliser uniquement les technologies compatibles avec le serveur de fichiers avec montée en puissance parallèle. Par exemple, vous ne pouvez pas utiliser la réplication DFS pour répliquer un partage de bibliothèque hébergé sur un partage de fichiers avec montée en puissance parallèle. Il est également important que les dernières mises à jour de logiciels soient installées sur le serveur de fichiers avec montée en puissance parallèle.

Pour utiliser un partage de fichiers avec montée en puissance parallèle en tant que partage de bibliothèque, vous devez d’abord ajouter un serveur de bibliothèque (généralement, un ordinateur virtuel) avec un partage local ou sans aucun partage. Puis, lorsque vous ajoutez un partage de bibliothèque, choisissez un partage de fichiers hébergé sur un serveur de fichiers avec montée en puissance parallèle. Ce partage doit être géré par VMM et créé pour être utilisé exclusivement par le serveur de bibliothèque. Veillez également à installer les dernières mises à jour sur le serveur de fichiers avec montée en puissance parallèle. Pour plus d’informations sur l’ajout de serveurs de bibliothèque VMM et partages de bibliothèque, consultez [ajouter des profils à la bibliothèque VMM](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801). Pour obtenir la liste des correctifs actuellement disponibles pour les services de fichiers et de stockage, consultez [l’article 2899011 de la Base de connaissances Microsoft](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie).

>[!NOTE]
>Certains utilisateurs, comme les travailleurs de l’information, ont des charges de travail qui ont un impact plus important sur les performances. Par exemple, lorsque des opérations telles que l’ouverture et la fermeture de fichiers, la création de fichiers et le changement de nom de fichiers existants, sont effectuées par plusieurs utilisateurs, cela peut avoir un impact sur les performances. Si un partage de fichiers est activé avec une disponibilité continue, il permet l’intégrité des données, mais affecte également les performances globales. La disponibilité continue nécessite une écriture des données sur le disque afin de garantir l’intégrité en cas de défaillance d’un nœud de cluster dans un serveur de fichiers avec montée en puissance parallèle. Par conséquent, un utilisateur qui copie plusieurs fichiers volumineux sur un serveur de fichiers peut s’attendre à une baisse des performances sur le partage de fichiers disponible en continu.

## <a name="features-included-in-this-scenario"></a>Fonctionnalités incluses dans ce scénario

Le tableau ci-dessous répertorie les fonctionnalités incluses dans ce scénario et détaille la manière dont elles prennent en charge ce dernier.

<table>
<thead>
<tr class="header">
<th>Fonctionnalité</th>
<th>Prise en charge de ce scénario</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">Clustering de basculement</a></td>
<td>Clusters de basculement ajouté les fonctionnalités suivantes dans Windows Server 2012 pour prendre en charge du serveur de fichiers avec montée en puissance : Nom de réseau distribué, le type de ressource de serveur de fichiers avec montée en puissance, Cluster Shared Volumes (CSV) 2 et le rôle serveur de fichiers Scale-Out haute disponibilité. Pour plus d’informations sur ces fonctionnalités, consultez <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">Nouveautés du clustering avec basculement dans Windows Server 2012 [redirigé]</a>.</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">Server Message Block</a></td>
<td>SMB 3.0 a ajouté les fonctionnalités suivantes dans Windows Server 2012 pour prendre en charge du serveur de fichiers avec montée en puissance : Basculement transparent SMB, SMB Multichannel et SMB Direct.<br />
<br />
Pour plus d’informations sur les fonctionnalités nouvelles et modifiées pour SMB dans Windows Server 2012 R2, consultez <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">What ' s New in SMB dans Windows Server</a>.</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>Informations supplémentaires

- [Guide de considérations de conception de Software-Defined Storage](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [Augmenter la disponibilité des serveurs, stockage et réseau](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Déployer Hyper-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [Déploiement de serveurs de fichiers rapides et efficaces pour les Applications serveur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [Monter en puissance, ou ne pas monter en puissance, là est la question](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) (billet de blog)
- [La Redirection de dossiers, les fichiers hors connexion et les profils utilisateur itinérants](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)