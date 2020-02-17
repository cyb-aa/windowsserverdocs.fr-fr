---
title: Vue d’ensemble du partage de fichiers à l’aide du protocole SMB 3 dans Windows Server
description: Vue d’ensemble de l’utilisation du protocole SMB 3 pour les partages et les serveurs de fichiers avec Windows Server.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 01/10/2020
ms.localizationpriority: medium
ms.openlocfilehash: 416145a8c4ec20eaf46cf4b5ac88a0cdf38bdf33
ms.sourcegitcommit: 76469d1b7465800315eaca3e0c7f0438fc3939ed
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75919887"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Vue d’ensemble du partage de fichiers à l’aide du protocole SMB 3 dans Windows Server

>S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit la fonctionnalité SMB 3 disponible dans Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012. Vous y découvrirez les usages pratiques de cette fonctionnalité, les principales fonctionnalités nouvelles ou mises à jour dans cette version comparées aux précédentes, ainsi que la configuration matérielle requise. SMB est également un protocole de structure fabric utilisé par les solutions de [centre de données défini par logiciel (SDDC)](../../sddc.md) comme les espaces de stockage direct, le réplica de stockage, etc. SMB version 3.0 a été introduit dans Windows Server 2012 et a fait l’objet d’améliorations incrémentielles dans les versions ultérieures.

## <a name="feature-description"></a>Description de la fonctionnalité

Le protocole SMB (Server Message Block) est un protocole de partage de fichiers réseau qui permet à des applications installées sur un ordinateur d’accéder en lecture et en écriture à des fichiers et de solliciter des services auprès de programmes serveur sur un réseau informatique. Le protocole SMB peut être utilisé en plus du protocole TCP/IP ou d‘autres protocoles réseau. Grâce à lui, une application (ou bien l’utilisateur d’une application) peut accéder à des fichiers ou d’autres ressources sur un serveur distant. Les applications peuvent ainsi lire, créer et mettre à jour des fichiers sur le serveur distant. SMB permet aussi de communiquer avec n’importe quel programme serveur configuré pour recevoir une demande de client SMB. SMB est un protocole de structure fabric utilisé par les technologies informatiques du centre de données défini par logiciel (SDDC), comme les espaces de stockage direct et le réplica de stockage. Pour plus d’informations, consultez [Centre de données défini par logiciel (SDDC) Windows Server](../../sddc.md).

## <a name="practical-applications"></a>Cas pratiques

Cette section aborde quelques-unes des nouvelles pratiques d’utilisation du nouveau protocole SMB 3.0.

* **Stockage des fichiers pour virtualisation (Hyper-V™ sur SMB)** . La technologie Hyper-V permet de stocker des fichiers d’ordinateur virtuel, par exemple une configuration, des fichiers de disque dur virtuel (VHD) et des captures instantanées, dans des partages de fichiers sur le protocole SMB 3.0. Cette méthode peut être employée à la fois pour des serveurs de fichiers autonomes et des serveurs de fichiers en cluster utilisant Hyper-V et conjointement avec un système de stockage de fichiers partagés destiné au cluster.
* **Microsoft SQL Server sur SMB**. SQL Server permet de stocker des fichiers de base de données utilisateur sur des partages de fichiers SMB. Pour l’heure, cette méthode est prise en charge avec SQL Server 2008 R2 pour des serveurs SQL autonomes. Les prochaines versions de SQL Server proposeront une prise en charge des serveurs SQL en cluster et des bases de données système.
* **Stockage classique pour les données de l’utilisateur final**. Le protocole SMB 3.0 apporte des améliorations aux charges de travail des utilisateurs professionnels de l’information (Information Worker ou client). Ces améliorations réduisent notamment les valeurs de latence des applications auxquelles sont confrontés les utilisateurs des filiales lorsqu’ils accèdent à des données sur des réseaux étendus (WAN) et protègent des données contre des tentatives d’écoute clandestine.

## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

Les sections suivantes décrivent les fonctionnalités ajoutées à SMB 3 et les mises à jour ultérieures.

## <a name="features-added-in-windows-server-2019-and-windows-10-version-1809"></a>Fonctionnalités ajoutées dans Windows Server 2019 et Windows 10, version 1809

| Fonctionnalité/fonction  | Nouveauté ou mise à jour  | Résumé  |
| --------- | --------- | --------- |
| Possibilité de demander une écriture directe sur disque sur des partages de fichiers qui ne sont pas disponibles en continu | Nouveau | Pour mieux garantir que les écritures sur un partage de fichiers parcourent bien toute la pile logicielle et matérielle jusqu’au disque physique avant que l’opération d’écriture ne soit retournée comme étant terminée, vous pouvez activer l’écriture directe sur le partage de fichiers à l’aide de la commande `NET USE /WRITETHROUGH` ou de l’applet de commande PowerShell `New-SMBMapping -UseWriteThrough`. L’utilisation de l’écriture directe permet un gain de performances. Pour plus d’informations, consultez le billet de blog [Controlling write-through behaviors in SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-write-through-behaviors-in-smb/bc-p/1083417#M677). |

## <a name="features-added-in-windows-server-version-1709-and-windows-10-version-1709"></a>Fonctionnalités ajoutées dans Windows Server version 1709 et Windows 10 version 1709

| Fonctionnalité/fonction  | Nouveauté ou mise à jour  | Résumé  |
| --------- | --------- | --------- |
| L’accès invité aux partages de fichiers est désactivé | Nouveau | Le client SMB n’autorise plus les actions suivantes : Accès du compte invité à un serveur distant. Le recours au compte invité consécutivement à des informations d’identification non valides est proposé. Pour plus d’informations, consultez [Accès invité dans SMB 2 désactivé par défaut dans Windows](https://support.microsoft.com/help/4046019/guest-access-in-smb2-disabled-by-default-in-windows-10-and-windows-ser). | 
| Mappage global SMB | Nouveau | Mappe un partage SMB distant à une lettre de lecteur accessible à tous les utilisateurs sur l’hôte local, y compris les conteneurs. Ce mappage est nécessaire pour permettre aux E/S de conteneur sur le volume de données de traverser le point de montage distant. Gardez à l’esprit que quand le mappage global SMB est appliqué aux conteneurs, tous les utilisateurs de l’hôte conteneur peuvent accéder au partage distant. Toute application en cours d’exécution sur l’hôte conteneur a également accès au partage distant mappé. Pour plus d’informations, consultez le billet de blog [Container Storage Support with Cluster Shared Volumes (CSV), Storage Spaces Direct, SMB Global Mapping](https://techcommunity.microsoft.com/t5/failover-clustering/container-storage-support-with-cluster-shared-volumes-csv/ba-p/372140). |
| Contrôle de dialecte SMB | Nouveau | Vous pouvez maintenant définir des valeurs de Registre pour contrôler la version SMB minimale (dialecte) et la version SMB maximale utilisées. Pour plus d’informations, consultez le billet de blog [Controlling SMB Dialects](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |

## <a name="features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607"></a>Fonctionnalités ajoutées dans SMB 3.11 avec Windows Server 2016 et Windows 10, version 1607

| Fonctionnalité/fonction  | Nouveauté ou mise à jour  | Résumé  |
| --------- | --------- | --------- |
| Chiffrement SMB     |   Mis à jour      | Le chiffrement SMB 3.1.1 avec AES-GCM (Advanced Encryption Standard-Galois/Counter mode) est plus rapide que la signature SMB ou le chiffrement SMB précédent qui utilisait AES-CCM.   |
| Mise en cache des répertoires | Nouveau | SMB 3.1.1 inclut des améliorations apportées à la mise en cache des répertoires. Les clients Windows peuvent désormais mettre en cache des répertoires beaucoup plus volumineux, d’environ 500 000 entrées. Les clients Windows tentent d’effectuer des demandes de répertoires avec des tampons de 1 Mo pour réduire les allers-retours et améliorer les performances. |
| Intégrité préalable à l’authentification | Nouveau |  Dans SMB 3.1.1, l’intégrité préalable à l’authentification renforce la protection contre les attaques de l’intercepteur sur les messages d’établissement de connexion et d’authentification SMB. Pour plus d’informations, consultez [Intégrité préalable à l’authentification de SMB 3.1.1 dans Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10). |
| Améliorations du chiffrement SMB | Nouveau | SMB 3.1.1 offre un mécanisme de négociation de l’algorithme de chiffrement par connexion, avec les options AES-128-CCM et AES-128-GCM. AES-128-GCM est la valeur par défaut pour les nouvelles versions de Windows, tandis que les versions antérieures continuent d’utiliser AES-128-CCM. |
| Prise en charge des mises à niveau de cluster | Nouveau | Permet d’effectuer des [mises à niveau de cluster](../../failover-clustering/cluster-operating-system-rolling-upgrade.md) en permettant à SMB de prendre en charge différentes versions maximales de SMB pour les clusters en cours de mise à niveau. Pour plus d’informations sur la façon dont SMB communique à l’aide de différentes versions (dialectes) du protocole, consultez le billet de blog [Controlling SMB Dialects](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |
| Prise en charge des clients SMB Direct dans Windows 10 | Nouveau | Windows 10 Entreprise, Windows 10 Éducation et Windows 10 Professionnel pour les Stations de travail incluent désormais la prise en charge des clients SMB Direct. |
| Prise en charge native des appels d’API FileNormalizedNameInformation | Nouveau | Ajoute la prise en charge native de l’interrogation du nom normalisé d’un fichier. Pour plus d’informations, consultez [FileNormalizedNameInformation](https://docs.microsoft.com/openspecs/windows_protocols/ms-fscc/20bcadba-808c-4880-b757-4af93e41edf6). |

Pour plus d’informations, consultez le billet de blog [What’s new in SMB 3.1.1 in the Windows Server 2016 Technical Preview 2](https://docs.microsoft.com/archive/blogs/josebda/whats-new-in-smb-3-1-1-in-the-windows-server-2016-technical-preview-2).

## <a name="features-added-in-smb-302-with-windows-server-2012-r2-and-windows-81"></a>Fonctionnalités ajoutées dans SMB 3.02 avec Windows Server 2012 R2 et Windows 8.1

| Fonctionnalité/fonction  | Nouveauté ou mise à jour  | Résumé  |
| --------- | --------- | --------- |
| Rééquilibrage automatique des clients Serveur de fichiers avec montée en puissance parallèle     |   Nouveau      | Améliore la scalabilité et la facilité de gestion des serveurs de fichiers avec montée en puissance parallèle. Les connexions client SMB sont suivies par partage de fichier (plutôt que par serveur) et les clients sont redirigés vers le nœud de cluster qui propose le meilleur accès au volume utilisé par le partage de fichiers. Cela améliore l’efficacité en réduisant le trafic entre les nœuds du serveur de fichiers. Les clients sont redirigés suite à une connexion initiale et quand le stockage en cluster est configuré.    |
| Performances sur WAN   | Mis à jour  | Windows 8.1 et Windows 10 offrent une amélioration de la prise en charge de CopyFile SRV_COPYCHUNK sur SMB quand vous utilisez l’Explorateur de fichiers pour effectuer des copies distantes depuis un emplacement sur un ordinateur distant vers une autre copie sur le même serveur. Vous ne copiez qu’une petite quantité de métadonnées sur le réseau (1/2 Kio pour 16 Mio de données de fichiers sont transmis). Il en résulte un gain de performances considérable. C’est une différence pour SMB au niveau du système d’exploitation et au niveau de l’Explorateur de fichiers. |
| SMB Direct     |   Mis à jour      | Améliore les performances pour les charges d’E/S de petite taille en augmentant l’efficacité lors de l’hébergement des charges de travail avec peu d’E/S (par exemple une base de données OLTP sur un ordinateur virtuel). Ces améliorations sont particulièrement notables sur des interfaces réseau rapides, telles que Ethernet 40 Gbits/s et InfiniBand 56 Gbits/s.  |
| Limites de bande passante SMB | Nouveau | Vous pouvez maintenant utiliser [Set-SmbBandwidthLimit](https://docs.microsoft.com/powershell/module/smbshare/set-smbbandwidthlimit) pour définir des limites de bande passante dans trois catégories : VirtualMachine (trafic Hyper-V sur SMB), LiveMigration (trafic Migration dynamique Hyper-V sur SMB) ou Default (tous les autres types de trafic SMB).

Pour plus d’informations sur les fonctionnalités SMB nouvelles et modifiées dans Windows Server 2012 R2, consultez [Nouveautés de SMB dans Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

## <a name="features-added-in-smb-30-with-windows-server-2012-and-windows-8"></a>Fonctionnalités ajoutées dans SMB 3.0 avec Windows Server 2012 et Windows 8

| Fonctionnalité/fonction  | Nouveauté ou mise à jour  | Résumé  |
| --------- | --------- | --------- |
| Basculement transparent SMB     |   Nouveau    | Permet aux administrateurs de procéder à des travaux de maintenance matérielle ou logicielle des nœuds sur un serveur de fichiers en cluster sans interrompre les applications serveur chargées de stocker les données sur ces partages de fichiers. De même, en cas de panne matérielle ou logicielle sur un nœud de cluster, les clients SMB sont reconnectés en toute transparence à un autre nœud de cluster sans interrompre les applications serveur qui stockent les données sur les partages de fichiers.        |
| Montée en charge SMB     |   Nouveau      | Prise en charge de plusieurs instances SMB sur un serveur de fichiers avec montée en puissance parallèle. Avec la version 2 des volumes partagés de cluster (CSV), les administrateurs peuvent créer des partages de fichiers offrant un accès simultané aux fichiers de données, avec des entrées/sorties (E/S) directes, via l’ensemble des nœuds dans un cluster de serveurs de fichiers. Cette approche garantit une meilleure utilisation de la bande passante réseau et un équilibrage de la charge pour les clients du serveur de fichiers. Elle permet aussi d’optimiser les performances des applications serveur.  |
| SMB Multichannel     |   Nouveau      |  Autorise l’agrégation de la bande passante réseau et de la tolérance de panne réseau lorsque plusieurs chemins sont disponibles entre le client SMB et le serveur SMB. Les applications serveur peuvent ainsi bénéficier pleinement de toute la bande passante réseau disponible et résister à une panne réseau.<br><br>SMB Multichannel dans SMB 3 contribue à augmenter considérablement les performances par rapport aux versions précédentes de SMB. |
| SMB Direct     |   Nouveau      | Prend en charge l’usage de cartes réseau dotées de la technologie RDMA et capables de fonctionner à pleine vitesse avec une très faible latence et une consommation minime de l’unité centrale. Pour des charges de travail de type Hyper-V ou Microsoft SQL Server, cette fonctionnalité permet à un serveur de fichiers distant d’agir comme un système de stockage local.<br><br>SMB Direct dans SMB 3 contribue à augmenter considérablement les performances par rapport aux versions précédentes de SMB.  |
| Compteurs de performances des applications serveur     |   Nouveau      |  Les nouveaux compteurs de performances SMB offrent des informations détaillées par partage sur le débit, la latence et les opérations d’E/S par seconde (IOPS), permettant aux administrateurs d’analyser les performances des partages de fichiers SMB dans lesquels leurs données sont stockées. Ces compteurs sont spécifiquement conçus pour des applications serveur, à l’instar d’Hyper-V et de SQL Server, qui stockent les fichiers dans des partages de fichiers distants.      |
| Optimisation des performances    |  Mis à jour   | Le client SMB et le serveur SMB ont tous deux été optimisés pour de petites opérations d’E/S en lecture/écriture aléatoires, ce qui est fréquent dans des applications serveur comme SQL Server OLTP. De plus, une unité de transmission maximale (MTU) de taille importante est activée par défaut, ce qui améliore considérablement les performances lors de transferts séquentiels volumineux, notamment les entrepôts de données SQL Server, la sauvegarde ou restauration de bases de données et le déploiement ou la copie de disques durs virtuels. |
| Applets de commande Windows PowerShell pour SMB     |   Nouveau      |  Grâce aux applets de commande Windows PowerShell pour SMB, un administrateur peut gérer des partages de fichiers sur le serveur de fichiers, de bout en bout, depuis la ligne de commande.   |
| Chiffrement SMB     |   Nouveau      | Permet un chiffrement de bout en bout des données SMB et protège les données contre les tentatives d’écoute clandestine sur des réseaux non approuvés. Aucun nouveau coût de déploiement n’est à prévoir. Aucune sécurité du protocole Internet (IPSec) ni aucun matériel spécialisé ou accélérateur WAN ne sont également nécessaires. Cette fonctionnalité peut être configurée par partage ou pour le serveur de fichiers tout entier et peut être activée pour un large éventail de scénarios où les données parcourent des réseaux non approuvés. |
| Bail de répertoire SMB     |  Nouveau | Améliore les temps de réponse des applications dans les filiales. Grâce aux baux de répertoire, les allers retours du client au serveur sont réduits puisque les métadonnées sont extraites à partir d’un cache d’informations de répertoire avec une plus longue durée de vie. La cohérence du cache est maintenue puisque les clients sont informés dès que les informations de répertoire sur le serveur sont modifiées. Les baux de répertoire fonctionnent avec des scénarios pour HomeFolder (lecture/écriture sans partage) et Publication (lecture seule avec partage).    |
| Performances sur WAN   | Nouveau   | Les verrouillages opportunistes de répertoires (oplock) et les baux oplock ont été introduits dans SMB 3.0. Pour les charges de travail de bureau/client types, les verrouillages opportunistes (oplock)/baux réduisent les allers-retours réseau d’environ 15 %.<br><br>Dans SMB 3, l’implémentation Windows de SMB a été affinée pour améliorer le comportement de mise en cache sur le client et la possibilité d’obtenir des débits plus élevés.<br><br>SMB 3 comprend des améliorations apportées à l’API CopyFile(), ainsi qu’aux outils associés comme Robocopy, pour envoyer (push) beaucoup plus de données sur le réseau. |
| Négociation de dialecte sécurisée | Nouveau | Permet une protection contre la tentative de mise à niveau de la négociation de dialecte par le biais d’une attaque de l’intercepteur. L’idée est d’empêcher un espion de rétrograder le dialecte initialement négocié et les fonctionnalités entre le client et le serveur. Pour plus d’informations, consultez le billet de blog [SMB3 Secure Dialect Negotiation](https://docs.microsoft.com/archive/blogs/openspecification/smb3-secure-dialect-negotiation). Notez que cette fonctionnalité a été remplacée par la fonctionnalité [Intégrité préalable à l’authentification SMB 3.1.1 dans Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10). |


## <a name="hardware-requirements"></a>Configuration matérielle requise

Le basculement transparent SMB nécessite la configuration suivante :

* Un cluster de basculement exécutant Windows Server 2012 ou Windows Server 2016 avec au moins deux nœuds configurés. Le cluster doit réussir les tests de validation de cluster intégrés dans l’Assistant Validation.
* Les partages de fichiers doivent être créés avec la propriété de disponibilité en continu (Continuous Availability ou CA) qui constitue la valeur par défaut.
* Vous devez créer les partages de fichiers sur des chemins d’accès au volume pour garantir une montée en charge SMB.
* Les ordinateurs clients doivent exécuter Windows® 8 ou Windows Server 2012, les deux incluant le client SMB mis à jour qui prend en charge la disponibilité continue.

> [!NOTE]
> Les clients de niveau inférieur peuvent se connecter à des partages de fichiers dotés de la propriété CA mais que le basculement transparent ne sera pas pris en charge pour ces clients.

Les fonctionnalités SMB Multichannel nécessitent la configuration suivante :

* Au moins deux ordinateurs exécutant Windows Server 2012 sont nécessaires. Aucune fonctionnalité supplémentaire n’est à installer ; la technologie est activée par défaut.
* Pour plus d’informations sur les configurations réseau recommandées, consultez la section Voir aussi à la fin de cette rubrique.

SMB Direct nécessite la configuration suivante :

* Au moins deux ordinateurs exécutant Windows Server 2012 sont nécessaires. Aucune fonctionnalité supplémentaire n’est à installer ; la technologie est activée par défaut.
* Des cartes réseau dotées de la technologie RDMA sont requises. Trois types de carte différents sont actuellement disponibles : iWARP, Infiniband ou RoCE (RDMA over Converged Ethernet).

## <a name="more-information"></a>Autres informations

La liste suivante fournit des ressources supplémentaires disponibles sur le web sur SMB et les technologies associées dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.

* [Storage](../storage.md) (Stockage)
* [Serveur de fichiers avec montée en puissance parallèle pour les données d’application](../../failover-clustering/sofs-overview.md)
* [Améliorer les performances d’un serveur de fichiers avec SMB Direct](smb-direct.md)
* [Déployer Hyper-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Déployer SMB Multichannel](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Déploiement de serveurs de fichiers rapides et efficaces pour les applications serveur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB : Guide de résolution des problèmes](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)
