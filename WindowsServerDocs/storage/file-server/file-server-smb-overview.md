---
title: Vue d’ensemble du partage de fichiers à l’aide du protocole SMB 3 dans Windows Server
description: Vue d’ensemble de l’utilisation du protocole SMB 3 pour les partages de fichiers et les services de fichiers avec Windows Server.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 01/10/2020
ms.localizationpriority: medium
ms.openlocfilehash: 416145a8c4ec20eaf46cf4b5ac88a0cdf38bdf33
ms.sourcegitcommit: 76469d1b7465800315eaca3e0c7f0438fc3939ed
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75919887"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Vue d’ensemble du partage de fichiers à l’aide du protocole SMB 3 dans Windows Server

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Cette rubrique décrit la fonctionnalité SMB 3 de Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 : les utilisations pratiques de la fonctionnalité, les fonctionnalités nouvelles ou mises à jour les plus significatives de cette version par rapport aux versions précédentes, et la configuration matérielle requise. SMB est également un protocole de structure utilisé par les solutions de [Centre de données définies par logiciel (SDDC)](../../sddc.md) , telles que espaces de stockage direct, le réplica de stockage, etc. La version SMB 3,0 a été introduite avec Windows Server 2012 et a été améliorée de façon incrémentielle dans les versions ultérieures.

## <a name="feature-description"></a>Description des fonctionnalités

Le protocole SMB (Server Message Block) est un protocole de partage de fichiers réseau qui permet à des applications installées sur un ordinateur d’accéder en lecture et en écriture à des fichiers et de solliciter des services auprès de programmes serveur sur un réseau informatique. Le protocole SMB peut être utilisé en plus du protocole TCP/IP ou d‘autres protocoles réseau. Grâce à lui, une application (ou bien l’utilisateur d’une application) peut accéder à des fichiers ou d’autres ressources sur un serveur distant. Les applications peuvent ainsi lire, créer et mettre à jour des fichiers sur le serveur distant. SMB peut également communiquer avec n’importe quel programme serveur configuré pour recevoir une demande de client SMB. SMB est un protocole de structure qui est utilisé par les technologies informatiques du centre de données défini par logiciel (SDDC), telles que espaces de stockage direct, le réplica de stockage. Pour plus d’informations, consultez Centre de données [défini par le logiciel Windows Server](../../sddc.md).

## <a name="practical-applications"></a>Applications pratiques

Cette section aborde quelques-unes des nouvelles pratiques d’utilisation du nouveau protocole SMB 3.0.

* **Stockage des fichiers pour virtualisation (Hyper-V™ sur SMB)** . La technologie Hyper-V permet de stocker des fichiers d’ordinateur virtuel, par exemple une configuration, des fichiers de disque dur virtuel (VHD) et des captures instantanées, dans des partages de fichiers sur le protocole SMB 3.0. Cette méthode peut être employée à la fois pour des serveurs de fichiers autonomes et des serveurs de fichiers en cluster utilisant Hyper-V et conjointement avec un système de stockage de fichiers partagés destiné au cluster.
* **Microsoft SQL Server sur SMB**. SQL Server permet de stocker des fichiers de base de données utilisateur sur des partages de fichiers SMB. Pour l’heure, cette méthode est prise en charge avec SQL Server 2008 R2 pour des serveurs SQL autonomes. Les prochaines versions de SQL Server proposeront une prise en charge des serveurs SQL en cluster et des bases de données système.
* **Stockage classique pour les données de l’utilisateur final**. Le protocole SMB 3.0 apporte des améliorations aux charges de travail des utilisateurs professionnels de l’information (Information Worker ou client). Ces améliorations réduisent notamment les valeurs de latence des applications auxquelles sont confrontés les utilisateurs des filiales lorsqu’ils accèdent à des données sur des réseaux étendus (WAN) et protègent des données contre des tentatives d’écoute clandestine.

## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

Les sections suivantes décrivent les fonctionnalités qui ont été ajoutées dans SMB 3 et les mises à jour ultérieures.

## <a name="features-added-in-windows-server-2019-and-windows-10-version-1809"></a>Fonctionnalités ajoutées dans Windows Server 2019 et Windows 10, version 1809

| Fonctionnalité/fonction  | Nouveauté ou mise à jour  | Récapitulatif  |
| --------- | --------- | --------- |
| Possibilité de demander une écriture sur disque sur des partages de fichiers qui ne sont pas disponibles en continu | Nouveau | Pour fournir une garantie supplémentaire qui consiste à écrire dans un partage de fichiers tout au long de la pile logicielle et matérielle sur le disque physique avant que l’opération d’écriture soit retournée comme terminée, vous pouvez activer l’écriture directe sur le partage de fichiers à l’aide de la commande `NET USE /WRITETHROUGH` dans la `New-SMBMapping -UseWriteThrough` applet de commande PowerShell. L’utilisation de l’accès en écriture peut avoir un certain nombre de performances. Pour plus d’informations, consultez le billet [de blog contrôle des comportements d’écriture dans SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-write-through-behaviors-in-smb/bc-p/1083417#M677) . |

## <a name="features-added-in-windows-server-version-1709-and-windows-10-version-1709"></a>Fonctionnalités ajoutées dans Windows Server, version 1709 et Windows 10, version 1709

| Fonctionnalité/fonction  | Nouveauté ou mise à jour  | Récapitulatif  |
| --------- | --------- | --------- |
| L’accès invité aux partages de fichiers est désactivé | Nouveau | Le client SMB n’autorise plus les actions suivantes : accès du compte invité à un serveur distant ; Le recours au compte invité après des informations d’identification non valides est fourni. Pour plus d’informations, consultez [accès invité dans SMB2 désactivé par défaut dans Windows](https://support.microsoft.com/help/4046019/guest-access-in-smb2-disabled-by-default-in-windows-10-and-windows-ser). | 
| Mappage global SMB | Nouveau | Mappe un partage SMB distant à une lettre de lecteur qui est accessible à tous les utilisateurs sur l’hôte local, y compris les conteneurs. Cela est nécessaire pour permettre aux e/s de conteneur sur le volume de données de traverser le point de montage distant. Sachez que lorsque vous utilisez le mappage global SMB pour les conteneurs, tous les utilisateurs sur l’hôte de conteneur peuvent accéder au partage distant. Toute application s’exécutant sur l’hôte de conteneur a également accès au partage distant mappé. Pour plus d’informations, consultez [prise en charge du stockage de conteneur avec les volumes partagés de cluster (CSV), espaces de stockage direct, mappage global SMB](https://techcommunity.microsoft.com/t5/failover-clustering/container-storage-support-with-cluster-shared-volumes-csv/ba-p/372140). |
| Contrôle de dialecte SMB | Nouveau | Vous pouvez maintenant définir des valeurs de Registre pour contrôler la version SMB minimale (dialecte) et la version maximale de SMB utilisées. Pour plus d’informations, consultez [contrôle des dialectes SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |

## <a name="features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607"></a>Fonctionnalités ajoutées dans SMB 3,11 avec Windows Server 2016 et Windows 10, version 1607

| Fonctionnalité/fonction  | Nouveauté ou mise à jour  | Récapitulatif  |
| --------- | --------- | --------- |
| Chiffrement SMB     |   Mise à jour terminée      | Le chiffrement SMB 3.1.1 avec Advanced Encryption Standard-Galois/Counter mode (AES-GCM) est plus rapide que la signature SMB ou le chiffrement SMB précédent utilisant AES-CCM.   |
| Mise en cache de répertoire | Nouveau | SMB 3.1.1 comprend des améliorations de la mise en cache des annuaires. Les clients Windows peuvent désormais mettre en cache des répertoires de plus grande taille, environ 500 000 entrées. Les clients Windows tentent d’effectuer des requêtes d’annuaire avec des tampons de 1 Mo pour réduire les allers-retours et améliorer les performances. |
| Intégrité de la pré-authentification | Nouveau |  Dans SMB 3.1.1, l’intégrité de la pré-authentification offre une protection améliorée contre les attaques de l’intercepteur de l’intercepteur avec les messages d’authentification et d’établissement de la connexion SMB. Pour plus d’informations, consultez [l’intégrité de pré-authentification SMB 3.1.1 dans Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10). |
| Améliorations du chiffrement SMB | Nouveau | SMB 3.1.1 offre un mécanisme de négociation de l’algorithme de chiffrement par connexion, avec les options AES-128-CCM et AES-128-GCM. AES-128-GCM est la valeur par défaut pour les nouvelles versions de Windows, tandis que les versions antérieures continuent à utiliser AES-128-CCM. |
| Prise en charge de la mise à niveau de cluster | Nouveau | Permet d’effectuer des [mises à niveau de clusters propagées](../../failover-clustering/cluster-operating-system-rolling-upgrade.md) en permettant à SMB de prendre en charge différentes versions max. de SMB pour les clusters en cours de mise à niveau. Pour plus d’informations sur la façon dont SMB communique à l’aide de différentes versions (dialectes) du protocole, consultez le billet de blog [contrôle des dialectes SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |
| Prise en charge des clients SMB direct dans Windows 10 | Nouveau | Windows 10 entreprise, Windows 10 éducation et Windows 10 professionnel pour stations de travail incluent désormais la prise en charge du client direct SMB. |
| Prise en charge native pour les appels d’API FileNormalizedNameInformation | Nouveau | Ajoute la prise en charge native de l’interrogation du nom normalisé d’un fichier. Pour plus d’informations, consultez [FileNormalizedNameInformation](https://docs.microsoft.com/openspecs/windows_protocols/ms-fscc/20bcadba-808c-4880-b757-4af93e41edf6). |

Pour plus d’informations, consultez le billet de blog [Nouveautés de SMB 3.1.1 dans Windows Server 2016 Technical Preview 2](https://docs.microsoft.com/archive/blogs/josebda/whats-new-in-smb-3-1-1-in-the-windows-server-2016-technical-preview-2).

## <a name="features-added-in-smb-302-with-windows-server-2012-r2-and-windows-81"></a>Fonctionnalités ajoutées dans SMB 3,02 avec Windows Server 2012 R2 et Windows 8.1

| Fonctionnalité/fonction  | Nouveauté ou mise à jour  | Récapitulatif  |
| --------- | --------- | --------- |
| Rééquilibrage automatique des clients Serveur de fichiers avec montée en puissance parallèle     |   Nouveau      | Améliore l’extensibilité et la facilité de gestion des serveurs de fichiers avec montée en puissance parallèle. Les connexions client SMB sont suivies par partage de fichier (plutôt que par serveur) et les clients sont redirigés vers le nœud de cluster qui propose le meilleur accès au volume utilisé par le partage de fichiers. Cela améliore l’efficacité en réduisant le trafic entre les nœuds du serveur de fichiers. Les clients sont redirigés suite à une connexion initiale et quand le stockage en cluster est configuré.    |
| Performances sur WAN   | Mise à jour terminée  | Windows 8.1 et Windows 10 offrent une amélioration de CopyFile SRV_COPYCHUNK par rapport à la prise en charge SMB lorsque vous utilisez l’Explorateur de fichiers pour les copies distantes à partir d’un emplacement sur un ordinateur distant vers une autre copie sur le même serveur. Vous ne copiez qu’une petite quantité de métadonnées sur le réseau (1/2KiB par 16MiB de données de fichier est transmis). Cela entraîne une amélioration significative des performances. Il s’agit d’une distinction au niveau du système d’exploitation et de l’Explorateur de fichiers pour SMB. |
| SMB Direct     |   Mise à jour terminée      | Améliore les performances pour les charges d’E/S de petite taille en augmentant l’efficacité lors de l’hébergement des charges de travail avec peu d’E/S (par exemple une base de données OLTP sur un ordinateur virtuel). Ces améliorations sont évidentes quand vous utilisez des interfaces réseau à vitesse élevée, telles que les cartes Ethernet 40 Gbit/s et 56 Gbits/s.  |
| Limites de bande passante SMB | Nouveau | Vous pouvez maintenant utiliser [Set-SmbBandwidthLimit](https://docs.microsoft.com/powershell/module/smbshare/set-smbbandwidthlimit) pour définir des limites de bande passante dans trois catégories : VirtualMachine (Hyper-v sur le trafic SMB), LiveMigration (Hyper-v migration dynamique trafic sur SMB) ou par défaut (tous les autres types de trafic SMB).

Pour plus d’informations sur les fonctionnalités SMB nouvelles et modifiées dans Windows Server 2012 R2, consultez [Nouveautés de SMB dans Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

## <a name="features-added-in-smb-30-with-windows-server-2012-and-windows-8"></a>Fonctionnalités ajoutées dans SMB 3,0 avec Windows Server 2012 et Windows 8

| Fonctionnalité/fonction  | Nouveauté ou mise à jour  | Récapitulatif  |
| --------- | --------- | --------- |
| Basculement transparent SMB     |   Nouveau    | Permet aux administrateurs de procéder à des travaux de maintenance matérielle ou logicielle des nœuds sur un serveur de fichiers en cluster sans interrompre les applications serveur chargées de stocker les données sur ces partages de fichiers. De même, en cas de panne matérielle ou logicielle sur un nœud de cluster, les clients SMB sont reconnectés en toute transparence à un autre nœud de cluster sans interrompre les applications serveur qui stockent les données sur les partages de fichiers.        |
| Montée en charge SMB     |   Nouveau      | Prise en charge de plusieurs instances SMB sur un Serveur de fichiers avec montée en puissance parallèle. Avec la version 2 des volumes partagés de cluster (CSV), les administrateurs peuvent créer des partages de fichiers offrant un accès simultané aux fichiers de données, avec des entrées/sorties (E/S) directes, via l’ensemble des nœuds dans un cluster de serveurs de fichiers. Cette approche garantit une meilleure utilisation de la bande passante réseau et un équilibrage de la charge pour les clients du serveur de fichiers. Elle permet aussi d’optimiser les performances des applications serveur.  |
| SMB Multichannel     |   Nouveau      |  Active l’agrégation de la bande passante réseau et de la tolérance de panne réseau si plusieurs chemins sont disponibles entre le client et le serveur SMB. Les applications serveur peuvent ainsi bénéficier pleinement de toute la bande passante réseau disponible et résister à une panne réseau.<br><br>SMB Multichannel dans SMB 3 contribue à une augmentation substantielle des performances par rapport aux versions précédentes de SMB. |
| SMB Direct     |   Nouveau      | Prend en charge l’usage de cartes réseau dotées de la technologie RDMA et capables de fonctionner à pleine vitesse avec une très faible latence et une consommation minime de l’unité centrale. Pour des charges de travail de type Hyper-V ou Microsoft SQL Server, cette fonctionnalité permet à un serveur de fichiers distant d’agir comme un système de stockage local.<br><br>SMB direct dans SMB 3 contribue à une augmentation substantielle des performances par rapport aux versions précédentes de SMB.  |
| Compteurs de performances des applications serveur     |   Nouveau      |  Les nouveaux compteurs de performances SMB fournissent des informations détaillées par partage sur le débit, la latence et les e/s par seconde (IOPS), ce qui permet aux administrateurs d’analyser les performances des partages de fichiers SMB dans lesquels leurs données sont stockées. Ces compteurs sont spécifiquement conçus pour des applications serveur, à l’instar d’Hyper-V et de SQL Server, qui stockent les fichiers dans des partages de fichiers distants.      |
| Optimisation des performances    |  Mise à jour terminée   | Le client et le serveur SMB ont été optimisés pour les e/s en lecture/écriture aléatoires, ce qui est courant dans les applications serveur telles que SQL Server OLTP. De plus, une unité de transmission maximale (MTU) de taille importante est activée par défaut, ce qui améliore considérablement les performances lors de transferts séquentiels volumineux, notamment les entrepôts de données SQL Server, la sauvegarde ou restauration de bases de données et le déploiement ou la copie de disques durs virtuels. |
| Applets de commande Windows PowerShell pour SMB     |   Nouveau      |  Grâce aux applets de commande Windows PowerShell pour SMB, un administrateur peut gérer des partages de fichiers sur le serveur de fichiers, de bout en bout, depuis la ligne de commande.   |
| Chiffrement SMB     |   Nouveau      | Permet un chiffrement de bout en bout des données SMB et protège les données contre les tentatives d’écoute clandestine sur des réseaux non approuvés. Aucun nouveau coût de déploiement n’est à prévoir. Aucune sécurité du protocole Internet (IPSec) ni aucun matériel spécialisé ou accélérateur WAN ne sont également nécessaires. Cette fonctionnalité peut être configurée par partage ou pour le serveur de fichiers tout entier et peut être activée pour un large éventail de scénarios où les données parcourent des réseaux non approuvés. |
| Bail de répertoire SMB     |  Nouveau | Améliore les temps de réponse des applications dans les filiales. Grâce aux baux de répertoire, les allers retours du client au serveur sont réduits puisque les métadonnées sont extraites à partir d’un cache d’informations de répertoire avec une plus longue durée de vie. La cohérence du cache est maintenue puisque les clients sont informés dès que les informations de répertoire sur le serveur sont modifiées. Les baux d’annuaire fonctionnent avec des scénarios pour HomeFolder (lecture/écriture sans partage) et publication (lecture seule avec partage).    |
| Performances sur WAN   | Nouveau   | Les verrous opportunistes de répertoires (oplocks) et les baux oplock ont été introduits dans SMB 3,0. Pour les charges de travail Office/client typiques, les verrous optionnels et les baux sont affichés pour réduire les allers-retours réseau d’environ 15%.<br><br>Dans SMB 3, l’implémentation Windows de SMB a été affinée pour améliorer le comportement de mise en cache sur le client, ainsi que la possibilité de pousser des débits plus élevés.<br><br>SMB 3 offre des améliorations apportées à l’API CopyFile (), ainsi qu’aux outils associés tels que Robocopy, pour pousser beaucoup plus de données sur le réseau. |
| Négociation de dialecte sécurisée | Nouveau | Permet de vous protéger contre la tentative de mise à niveau de la négociation de dialecte par l’intermédiaire de l’intercepteur. L’idée est d’empêcher un espion de rétrograder le dialecte initialement négocié et les fonctionnalités entre le client et le serveur. Pour plus d’informations, consultez [négociation de dialecte sécurisé SMB3](https://docs.microsoft.com/archive/blogs/openspecification/smb3-secure-dialect-negotiation). Notez que cela a été remplacé par l' [intégrité de pré-authentification de SMB 3.1.1 dans](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10) la fonctionnalité de Windows 10 dans SMB 3.1.1. |


## <a name="hardware-requirements"></a>Configuration matérielle requise

Le basculement transparent SMB nécessite la configuration suivante :

* Un cluster de basculement exécutant Windows Server 2012 ou Windows Server 2016 avec au moins deux nœuds configurés. Le cluster doit réussir les tests de validation de cluster intégrés dans l’Assistant Validation.
* Les partages de fichiers doivent être créés avec la propriété de disponibilité en continu (Continuous Availability ou CA) qui constitue la valeur par défaut.
* Vous devez créer les partages de fichiers sur des chemins d’accès au volume pour garantir une montée en charge SMB.
* Les ordinateurs clients doivent exécuter Windows® 8 ou Windows Server 2012, qui incluent tous les deux le client SMB mis à jour qui prend en charge la disponibilité continue.

> [!NOTE]
> Les clients de niveau supérieur peuvent se connecter aux partages de fichiers qui possèdent la propriété autorité de certification, mais le basculement transparent n’est pas pris en charge pour ces clients.

Les fonctionnalités SMB Multichannel nécessitent la configuration suivante :

* Au moins deux ordinateurs exécutant Windows Server 2012 sont requis. Aucune fonctionnalité supplémentaire n’est à installer ; la technologie est activée par défaut.
* Pour plus d’informations sur les configurations réseau recommandées, consultez la section Voir aussi à la fin de cette rubrique.

SMB Direct nécessite la configuration suivante :

* Au moins deux ordinateurs exécutant Windows Server 2012 sont requis. Aucune fonctionnalité supplémentaire n’est à installer ; la technologie est activée par défaut.
* Des cartes réseau dotées de la technologie RDMA sont requises. Trois types de carte différents sont actuellement disponibles : iWARP, Infiniband ou RoCE (RDMA over Converged Ethernet).

## <a name="more-information"></a>Autres informations

La liste suivante fournit des ressources supplémentaires sur le Web sur SMB et les technologies associées dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.

* [Storage](../storage.md) (Stockage)
* [Serveur de fichiers avec montée en puissance parallèle pour les données d’application](../../failover-clustering/sofs-overview.md)
* [Améliorer les performances d’un serveur de fichiers avec SMB direct](smb-direct.md)
* [Déployer Hyper-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Déployer SMB Multichannel](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Déploiement de serveurs de fichiers rapides et efficaces pour les applications serveur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB : Guide de dépannage](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)
