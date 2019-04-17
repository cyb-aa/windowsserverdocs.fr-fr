---
title: Serveur de fichiers montée en puissance parallèle pour la présentation des données d’application
description: Vue d’ensemble de la fonctionnalité de serveur de fichiers de mise à niveau pour Windows Server R2 201, Windows Server 2012 et Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 04e25e9c69062611d9d14c220614f148ac5de770
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082094"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>Serveur de fichiers montée en puissance parallèle pour la présentation des données d’application

>S’applique à: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Serveur de fichiers de mise à niveau est une fonctionnalité qui est conçue pour fournir des partages de fichiers de la montée en puissance parallèle en permanence disponibles pour le stockage des applications de serveur basé sur le fichier. Partages de fichiers de la montée en puissance parallèle offre la possibilité de partager le même dossier à partir de plusieurs nœuds du même cluster. Ce scénario se concentre sur la façon de planifier et déployer le serveur de fichiers montée en puissance parallèle.

Vous pouvez déployer et configurer un serveur de fichiers en cluster à l’aide d’une des méthodes suivantes:

- **Serveur de fichiers de mise à niveau pour les données d’application** Cette fonctionnalité de serveur de fichiers en cluster a été introduite dans Windows Server 2012, et il vous permet de stocker des données d’application de serveur, tels que les fichiers d’ordinateur virtuel Hyper-V, sur les partages de fichiers et d’obtenir un niveau de fiabilité, de disponibilité, la facilité de gestion et haute semblable performances que vous attendriez d’un réseau de stockage. Tous les partages de fichiers sont simultanément en ligne sur tous les nœuds. Partages de fichiers associés à ce type de serveur de fichiers en cluster sont appelés des partages de fichiers de la montée en puissance parallèle. Il est parfois appelé actif. Il s’agit du type de serveur recommandé lors du déploiement soit Hyper-V Server Message Block (SMB) ou Microsoft SQL Server sur SMB.
- **Serveur de fichiers pour une utilisation générale** Il s’agit de l’indicateur de continuation le cluster de serveur de fichiers pris en charge dans Windows Server depuis l’introduction du Clustering de basculement. Ce type de serveur de fichiers en cluster et par conséquent, toutes les actions associées au serveur de fichiers en cluster, est en ligne sur un nœud à la fois. Il est parfois appelé active / passive ou actif en double. Partages de fichiers associés à ce type de serveur de fichiers en cluster sont appelés des partages de fichiers en cluster. Il s’agit du type de serveur recommandé lors du déploiement des scénarios de travailleur de l’information.

## <a name="scenario-description"></a>Description du scénario

Avec les partages de fichiers de mise à niveau, vous pouvez partager le même dossier à partir de plusieurs nœuds d’un cluster. Par exemple, si vous disposez d’un cluster de serveurs quatre nœuds fichier qui est à l’aide de Server Message Block (SMB) de la montée en puissance parallèle, un ordinateur exécutant Windows Server 2012 R2 ou Windows Server 2012 peut accéder aux partages de fichiers de n’importe lequel des quatre nœuds. Cela en tirant parti des nouvelles fonctionnalités de Clustering de basculement Windows Server et les fonctionnalités du protocole de serveur de fichiers Windows, SMB 3.0. Administrateurs de serveur de fichiers peuvent fournir des partages de fichiers de mise à niveau et des services de fichiers disponibles en permanence pour les applications serveur et répondre rapidement aux demandes d’augmentation en mettant simplement des serveurs en ligne. Tout ceci peut être fait dans un environnement de production, et il est complètement transparent pour l’application serveur.

Clé fourni par le serveur de fichiers de mise à niveau dans les avantages suivants:

- **Partages de fichiers actif**. Tous les nœuds du cluster peuvent accepter et traiter les demandes des clients SMB. En rendant le fichier partager du contenu accessible par le biais de tous les nœuds du cluster simultanément, les clusters SMB 3.0 et les clients coopèrent pour assurer un basculement transparent à des nœuds de cluster sujet pendant la maintenance planifiée et des échecs non planifiés avec le service interruption.
- **Bande passante accrue**. La bande passante maximale de partage est la bande passante totale de tous les nœuds de cluster de serveur de fichiers. Contrairement aux versions antérieures de Windows Server, la bande passante totale est n’est plus limitée à la bande passante d’un nœud de cluster; mais, au lieu de cela, la capacité du système de stockage de sauvegarde définit les contraintes. Vous pouvez augmenter la bande passante totale en ajoutant des nœuds.
- **CHKDSK sans interruption de service**. CHKDSK dans Windows Server 2012 est considérablement améliorée pour réduire considérablement le temps de qu'un système de fichiers est en mode hors connexion pour la réparation. Volumes partagés en cluster (CSVs) aller plus loin en éliminant la phase en mode hors connexion. Un système de fichier CSV (CSVFS) peuvent utiliser CHKDSK sans impact sur les applications avec des descripteurs ouverts sur le système de fichiers.
- **Cache du Volume partagé de cluster**. CSVs dans Windows Server 2012 prend en charge un cache en lecture, ce qui accroît considérablement les performances dans certains scénarios, tels que dans l’Infrastructure VDI (Virtual Desktop).
- **Gestion simplifiée**. Avec montée en puissance parallèle de serveur de fichiers, vous créez les serveurs de fichiers de la montée en puissance parallèle et puis ajoutez les CSVs nécessaires et les partages de fichiers. Il n’est plus nécessaire de créer plusieurs serveurs de fichiers en cluster, chacun avec des disques de cluster distinct, puis développez Stratégies d’emplacement afin d’activité sur chaque nœud du cluster.
- **Automatique rééquilibrage des clients de serveur de fichiers montée en puissance parallèle**. Dans Windows Server 2012 R2 rééquilibrage automatique améliore l’évolutivité et la facilité de gestion pour les serveurs de fichiers de la montée en puissance parallèle. Les connexions client SMB sont suivies par un partage de fichiers (et non par serveur), et les clients sont ensuite redirigés vers le nœud de cluster avec l’accès aux meilleures le volume utilisé par le partage de fichiers. Ceci améliore l’efficacité en réduisant le trafic de redirection entre les nœuds de serveur de fichiers. Les clients sont redirigés après une connexion initiale et lorsque le stockage du cluster est reconfiguré.

## <a name="in-this-scenario"></a>Dans ce scénario

Les rubriques suivantes sont disponibles pour vous aider à déployer un serveur de fichiers de mise à niveau:

- [Plan pour le serveur de fichiers de la montée en puissance parallèle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [Étape 1: Planifier le stockage dans le serveur de fichiers de la montée en puissance parallèle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [Étape 2: Planifier la mise en réseau dans le serveur de fichiers de la montée en puissance parallèle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [Déployer le serveur de fichiers de la montée en puissance parallèle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [Étape 1: Installer les composants requis pour le serveur de fichiers de la montée en puissance parallèle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [Étape 2: Configurer le serveur de fichiers de la montée en puissance parallèle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [Étape 3: Configurer Hyper-V pour utiliser le serveur de fichiers de la montée en puissance parallèle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [Étape 4: Configurer Microsoft SQL Server pour utiliser le serveur de fichiers de la montée en puissance parallèle](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>Quand utiliser le serveur de fichiers montée en puissance parallèle

N’utilisez pas serveur de fichiers montée en puissance parallèle si votre charge de travail génère un grand nombre d’opérations des métadonnées, tels que les fichiers de l’ouverture, fermeture de fichiers, la création de nouveaux fichiers ou renommer un fichier existant. Un travailleur de l’information de type génère un grand nombre d’opérations des métadonnées. Vous devez utiliser un serveur de fichiers montée en puissance parallèle si vous êtes intéressé par l’évolutivité et la simplicité qu’il propose et si vous avez besoin uniquement les technologies prises en charge avec le serveur de fichiers montée en puissance parallèle.

Le tableau suivant répertorie les fonctionnalités dans SMB 3.0, les systèmes de fichiers communs Windows, fichier serveur données technologies et charges de travail courantes. Vous pouvez voir si la technologie est pris en charge avec le serveur de fichiers montée en puissance parallèle ou s’il nécessite un serveur de fichiers en cluster traditionnel (également appelé un serveur de fichiers pour une utilisation générale).

<table>
<thead>
<tr class="header">
<th>Zone de technologie</th>
<th>Fonctionnalité</th>
<th>Cluster de serveurs d’utiliser le fichier général</th>
<th>Serveur de fichiers de la montée en puissance parallèle</th>
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
<td>SMB multicanal</td>
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
<td>Basculement Transparent SMB</td>
<td>Oui (si une disponibilité continue est activée)</td>
<td>Oui</td>
</tr>
<tr class="even">
<td>Système de fichiers</td>
<td>NTFS</td>
<td>Oui</td>
<td>NA</td>
</tr>
<tr class="odd">
<td>Système de fichiers</td>
<td>Système de fichiers résilient (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>Recommandé avec le stockage d’espaces Direct</td>
<td>Recommandé avec le stockage d’espaces Direct</td>
</tr>
<tr class="even">
<td>Système de fichiers</td>
<td>Cluster partagés Volume système de fichiers (CSV)</td>
<td>NA</td>
<td>Oui</td>
</tr>
<tr class="odd">
<td>Gestion de fichiers</td>
<td>BranchCache</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="even">
<td>Gestion de fichiers</td>
<td>Déduplication des données (Windows Server 2012)</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="odd">
<td>Gestion de fichiers</td>
<td>Déduplication des données (Windows Server 2012 R2)</td>
<td>Oui</td>
<td>Oui (VDI uniquement)</td>
</tr>
<tr class="even">
<td>Gestion de fichiers</td>
<td>Racine du serveur racine DFS Namespace (DFS)</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="odd">
<td>Gestion de fichiers</td>
<td>Serveur cible de dossier Namespace DFS (DFS)</td>
<td>Oui</td>
<td>Oui</td>
</tr>
<tr class="even">
<td>Gestion de fichiers</td>
<td>DFS réplication</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="odd">
<td>Gestion de fichiers</td>
<td>Gestionnaire de ressources du serveur de fichiers (Quotas et les écrans)</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="even">
<td>Gestion de fichiers</td>
<td>Infrastructure de Classification de fichier</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="odd">
<td>Gestion de fichiers</td>
<td>Contrôle d’accès (access basée sur les revendications, CAP) dynamique</td>
<td>Oui</td>
<td>Non</td>
</tr>
<tr class="even">
<td>Gestion de fichiers</td>
<td>Redirection de dossiers</td>
<td>Oui</td>
<td>Non recommandé *</td>
</tr>
<tr class="odd">
<td>Gestion de fichiers</td>
<td>Fichiers en mode hors connexion (mise en cache de côté client)</td>
<td>Oui</td>
<td>Non recommandé *</td>
</tr>
<tr class="even">
<td>Gestion de fichiers</td>
<td>Les profils utilisateur itinérants</td>
<td>Oui</td>
<td>Non recommandé *</td>
</tr>
<tr class="odd">
<td>Gestion de fichiers</td>
<td>Répertoires de base</td>
<td>Oui</td>
<td>Non recommandé *</td>
</tr>
<tr class="even">
<td>Gestion de fichiers</td>
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
<td>Microsoft SQL Server</td>
<td>Non recommandé</td>
<td>Oui</td>
</tr>
</tbody>
</table>

\ * La Redirection de dossiers, les fichiers en mode hors connexion, les profils utilisateur itinérants ou répertoires d’accueil génèrent un grand nombre d’écritures qui doivent être écrites immédiatement sur le disque (sans mise en mémoire tampon) lorsque vous utilisez des partages de fichiers disponibles en permanence, ce qui réduit les performances par rapport à usage général des partages de fichiers. En continu les partages de fichiers disponibles sont également incompatibles avec le Gestionnaire de ressources du serveur de fichiers et des ordinateurs exécutant Windows XP. En outre, les fichiers en mode hors connexion ne peuvent pas effectuer la transition en mode hors connexion pendant 3 à 6 minutes après qu’un utilisateur perd l’accès à un partage, ce qui pourrait frustration des utilisateurs qui ne sont pas encore le mode toujours en mode hors connexion des fichiers en mode hors connexion.

## <a name="practical-applications"></a>Applications pratiques

Serveurs de fichiers de montée en puissance parallèle sont idéales pour le stockage des applications de serveur. Voici quelques exemples d’applications de serveur peuvent stocker leurs données sur un partage de fichiers de mise à niveau:

- Le serveur Web Internet Information Services (IIS) peut stocker des données de configuration et pour les sites Web sur un partage de fichiers de la montée en puissance parallèle. Pour plus d’informations, voir [Configuration partagée](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264).
- Hyper-V peut stocker la configuration et disques virtuels live sur un partage de fichiers de la montée en puissance parallèle. Pour plus d’informations, voir [Déploiement d’Hyper-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).
- SQL Server peut stocker les fichiers de base de données active sur un partage de fichiers de la montée en puissance parallèle. Pour plus d’informations, consultez [installer SQL Server avec les fichiers SMB partager comme option de stockage](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option).
- Virtual Machine Manager (VMM) peut stocker un partage de bibliothèque (qui contient les modèles d’ordinateurs virtuels et les fichiers associés) sur un partage de fichiers de la montée en puissance parallèle. Toutefois, le serveur de bibliothèque elle-même ne peut pas être un serveur de fichiers de mise à niveau, il doit se trouver sur un serveur autonome ou un cluster de basculement qui n’utilise pas le rôle de cluster de serveur de fichiers montée en puissance parallèle.

Si vous utilisez un partage de fichiers de la montée en puissance parallèle comme un partage de bibliothèque, vous pouvez utiliser uniquement les technologies qui sont compatibles avec le serveur de fichiers montée en puissance parallèle. Par exemple, vous ne pouvez pas utiliser la réplication DFS pour répliquer un partage de bibliothèque hébergé sur un partage de fichiers de la montée en puissance parallèle. Il est également important que le serveur de fichiers de la montée en puissance parallèle dispose des dernières mises à jour de logiciels installés.

Pour utiliser un partage de fichiers de la montée en puissance parallèle comme un partage de bibliothèque, d’abord ajouter un serveur de bibliothèque (probablement un ordinateur virtuel) avec un partage local ou aucun partage tout. Lorsque vous ajoutez un partage de bibliothèque, choisissez ensuite un partage de fichiers qui est hébergé sur un serveur de fichiers de la montée en puissance parallèle. Ce partage doit être créé pour une utilisation en mode exclusif par le serveur de bibliothèque et géré VMM. Assurez-vous également d’installer les dernières mises à jour sur le serveur de fichiers de la montée en puissance parallèle. Pour plus d’informations sur l’ajout de serveurs de bibliothèque VMM et les partages de bibliothèque, voir [profils d’ajouter à la bibliothèque VMM](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801). Pour obtenir la liste des correctifs actuellement disponibles pour les Services de stockage et de fichiers, voir [l’article 2899011 de la Base de connaissances Microsoft](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie).

>[!NOTE]
>Certains utilisateurs, telles que des travailleurs de l’information, ont des charges de travail qui ont un impact supérieur sur les performances. Par exemple, les opérations telles que l’ouverture et fermeture de fichiers, création de nouveaux fichiers et renommer les fichiers existants, sont effectuées par plusieurs utilisateurs, ont un impact sur les performances. Si un partage de fichiers est activé avec une disponibilité continue, il fournit l’intégrité des données, mais elle affecte également les performances globales. Disponibilité continue nécessite que les données écrit par le biais de sur le disque pour garantir l’intégrité en cas de défaillance d’un nœud de cluster dans un serveur de fichiers montée en puissance parallèle. Par conséquent, un utilisateur qui copie plusieurs fichiers volumineux sur un serveur de fichiers prévoir une baisse de performance sur le partage de fichiers disponibles en permanence.

## <a name="features-included-in-this-scenario"></a>Fonctionnalités incluses dans ce scénario

Le tableau suivant répertorie les fonctionnalités qui font partie de ce scénario et décrit la prise en charge.

<table>
<thead>
<tr class="header">
<th>Fonctionnalité</th>
<th>Comment elle prend en charge ce scénario</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">Clustering de basculement</a></td>
<td>Clusters de basculement ajouté les fonctionnalités suivantes dans Windows Server 2012 pour prendre en charge de serveur de fichiers de la montée en puissance parallèle: nom de réseau de distribution, le type de ressource serveur de fichiers montée en puissance parallèle, Cluster partagés Volumes (CSV) 2 et le rôle de la montée en puissance parallèle fichier Server haute disponibilité. Pour plus d’informations sur ces fonctionnalités, voir <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">Nouveautés de clustering avec basculement de Windows Server 2012 [redirigé]</a>.</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">SMB</a></td>
<td>SMB 3.0 ajouté les fonctionnalités suivantes dans Windows Server 2012 pour prendre en charge de la montée en puissance parallèle serveur de fichiers: basculement Transparent SMB SMB multicanal et SMB Direct.<br />
<br />
Pour plus d’informations sur les fonctionnalités nouvelles et modifiées pour SMB dans Windows Server 2012 R2, voir <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">What ' s New in SMB dans Windows Server</a>.</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>Informations supplémentaires

- [Guide de considérations relatives à la conception de stockage défini par le logiciel](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [L’augmentation de la disponibilité du réseau, de stockage et de serveur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Déployer Hyper\-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [Déploiement de serveurs de fichiers rapide et efficace pour les Applications serveur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [Pour mettre à l’échelle parallèle ou non à la montée en puissance parallèle, qui est la question](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) (billet de blog)
- [Redirection de dossiers, fichiers hors connexion et profils utilisateurs itinérants](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)