---
title: Vue d’ensemble du partage de fichiers à l’aide du protocole SMB 3 dans Windows Server
description: Vue d’ensemble de l’utilisation du protocole SMB 3 pour les partages de fichiers et les serveurs de fichiers avec Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc4c8b341ee78db80f862ee412400f0a930fe810
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845050"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Vue d’ensemble du partage de fichiers à l’aide du protocole SMB 3 dans Windows Server

>S’applique à : Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cette rubrique décrit la fonctionnalité SMB 3.0 dans Windows Server® 2012, Windows Server 2012 R2 et Windows Server 2016, usages pratiques pour la fonctionnalité, les plus importantes nouvelles ou mises à jour de fonctionnalités dans cette version par rapport aux versions précédentes et le matériel configuration requise.

## <a name="feature-description"></a>Description des fonctionnalités

Le protocole SMB (Server Message Block) est un protocole de partage de fichiers réseau qui permet à des applications installées sur un ordinateur d’accéder en lecture et en écriture à des fichiers et de solliciter des services auprès de programmes serveur sur un réseau informatique. Le protocole SMB peut être utilisé en plus du protocole TCP/IP ou d‘autres protocoles réseau. Grâce à lui, une application (ou bien l’utilisateur d’une application) peut accéder à des fichiers ou d’autres ressources sur un serveur distant. Les applications peuvent ainsi lire, créer et mettre à jour des fichiers sur le serveur distant. Il permet aussi de communiquer avec n’importe quel programme serveur configuré pour recevoir une demande de clientSMB. Windows Server 2012 introduit la nouvelle version 3.0 du protocole SMB.

## <a name="practical-applications"></a>Cas pratiques

Cette section aborde quelques-unes des nouvelles pratiques d’utilisation du nouveau protocole SMB 3.0.

* **Stockage des fichiers pour virtualisation (Hyper-V™ sur SMB)**. La technologie Hyper-V permet de stocker des fichiers d’ordinateur virtuel, par exemple une configuration, des fichiers de disque dur virtuel (VHD) et des captures instantanées, dans des partages de fichiers sur le protocole SMB 3.0. Cette méthode peut être employée à la fois pour des serveurs de fichiers autonomes et des serveurs de fichiers en cluster utilisant Hyper-V et conjointement avec un système de stockage de fichiers partagés destiné au cluster.
* **Microsoft SQL Server sur SMB**. SQL Server permet de stocker des fichiers de base de données utilisateur sur des partages de fichiers SMB. Pour l’heure, cette méthode est prise en charge avec SQL Server 2008 R2 pour des serveurs SQL autonomes. Les prochaines versions de SQL Server proposeront une prise en charge des serveurs SQL en cluster et des bases de données système.
* **Stockage classique pour les données de l’utilisateur final**. Le protocole SMB 3.0 apporte des améliorations aux charges de travail des utilisateurs professionnels de l’information (Information Worker ou client). Ces améliorations réduisent notamment les valeurs de latence des applications auxquelles sont confrontés les utilisateurs des filiales lorsqu’ils accèdent à des données sur des réseaux étendus (WAN) et protègent des données contre des tentatives d’écoute clandestine.

## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

Pour plus d’informations sur les fonctionnalités nouvelles et modifiées dans Windows Server 2012 R2, consultez [What ' s New in SMB dans Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

SMB dans Windows Server 2012 et Windows Server 2016 inclut le nouveau protocole SMB 3.0 et nombreuses nouvelles améliorations qui sont décrites dans le tableau suivant.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Fonctionnalité/fonction</p></th>
<th><p>Nouveauté ou mise à jour</p></th>
<th><p>Récapitulatif</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Basculement transparent SMB</p></td>
<td><p>Nouveau</p></td>
<td><p>Permet aux administrateurs de procéder à des travaux de maintenance matérielle ou logicielle des nœuds sur un serveur de fichiers en cluster sans interrompre les applications serveur chargées de stocker les données sur ces partages de fichiers. De même, en cas de panne matérielle ou logicielle sur un nœud de cluster, les clients SMB sont reconnectés en toute transparence à un autre nœud de cluster sans interrompre les applications serveur qui stockent les données sur les partages de fichiers.</p></td>
</tr>
<tr class="even">
<td><p>Montée en charge SMB</p></td>
<td><p>Nouveau</p></td>
<td><p>Avec la version 2 des volumes partagés de cluster (CSV), les administrateurs peuvent créer des partages de fichiers offrant un accès simultané aux fichiers de données, avec des entrées/sorties (E/S) directes, via l’ensemble des nœuds dans un cluster de serveurs de fichiers. Cette approche garantit une meilleure utilisation de la bande passante réseau et un équilibrage de la charge pour les clients du serveur de fichiers. Elle permet aussi d’optimiser les performances des applications serveur.</p></td>
</tr>
<tr class="odd">
<td><p>SMB Multichannel</p></td>
<td><p>Nouveau</p></td>
<td><p>Autorise l’agrégation de la bande passante réseau et de la tolérance de panne réseau lorsque plusieurs chemins sont disponibles entre le client SMB 3.0 et le serveur SMB 3.0. Les applications serveur peuvent ainsi bénéficier pleinement de toute la bande passante réseau disponible et résister à une panne réseau.</p></td>
</tr>
<tr class="even">
<td><p>SMB Direct</p></td>
<td><p>Nouveau</p></td>
<td><p>Prend en charge l’usage de cartes réseau dotées de la technologie RDMA et capables de fonctionner à pleine vitesse avec une très faible latence et une consommation minime de l’unité centrale. Pour des charges de travail de type Hyper-V ou Microsoft SQL Server, cette fonctionnalité permet à un serveur de fichiers distant d’agir comme un système de stockage local.</p></td>
</tr>
<tr class="odd">
<td><p>Compteurs de performances des applications serveur</p></td>
<td><p>Nouveau</p></td>
<td><p>Les nouveaux compteurs de performances SMB offrent des informations détaillées par partage sur le débit, la latence et les opérations d’E/S par seconde (IOPS), permettant aux administrateurs d’analyser les performances des partages de fichiers SMB 3.0 dans lesquels leurs données sont stockées. Ces compteurs sont spécifiquement conçus pour des applications serveur, à l’instar d’Hyper-V et de SQL Server, qui stockent les fichiers dans des partages de fichiers distants.</p></td>
</tr>
<tr class="even">
<td><p>Optimisation des performances</p></td>
<td><p>Mise à jour terminée</p></td>
<td><p>Le client SMB 3.0 et le serveur SMB 3.0 ont tous deux été optimisés pour de petites opérations d’E/S en lecture/écriture aléatoires, ce qui est fréquent dans des applications serveur comme SQL Server OLTP. De plus, une unité de transmission maximale (MTU) de taille importante est activée par défaut, ce qui améliore considérablement les performances lors de transferts séquentiels volumineux, notamment les entrepôts de données SQL Server, la sauvegarde ou restauration de bases de données et le déploiement ou la copie de disques durs virtuels.</p></td>
</tr>
<tr class="odd">
<td><p>Applets de commande Windows PowerShell pour SMB</p></td>
<td><p>Nouveau</p></td>
<td><p>Grâce aux applets de commande Windows PowerShell pour SMB, un administrateur peut gérer des partages de fichiers sur le serveur de fichiers, de bout en bout, depuis la ligne de commande.</p></td>
</tr>
<tr class="even">
<td><p>Chiffrement SMB</p></td>
<td><p>Nouveau</p></td>
<td><p>Permet un chiffrement de bout en bout des données SMB et protège les données contre les tentatives d’écoute clandestine sur des réseaux non approuvés. Aucun nouveau coût de déploiement n’est à prévoir. Aucune sécurité du protocole Internet (IPSec) ni aucun matériel spécialisé ou accélérateur WAN ne sont également nécessaires. Cette fonctionnalité peut être configurée par partage ou pour le serveur de fichiers tout entier et peut être activée pour un large éventail de scénarios où les données parcourent des réseaux non approuvés.</p></td>
</tr>
<tr class="odd">
<td><p>Bail de répertoire SMB</p></td>
<td><p>Nouveau</p></td>
<td><p>Améliore les temps de réponse des applications dans les filiales. Grâce aux baux de répertoire, les allers retours du client au serveur sont réduits puisque les métadonnées sont extraites à partir d’un cache d’informations de répertoire avec une plus longue durée de vie. La cohérence du cache est maintenue puisque les clients sont informés dès que les informations de répertoire sur le serveur sont modifiées. Fonctionne avec des scénarios pour <em>HomeFolder</em> (lecture/écriture sans partage) et <em>Publication</em> (lecture seule avec partage).</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>Configuration matérielle requise

Le basculement transparent SMB nécessite la configuration suivante :

* Un cluster de basculement exécutant Windows Server 2012 ou Windows Server 2016 avec au moins deux nœuds configurés. Le cluster doit réussir les tests de validation de cluster intégrés dans l’Assistant Validation.
* Les partages de fichiers doivent être créés avec la propriété de disponibilité en continu (Continuous Availability ou CA) qui constitue la valeur par défaut.
* Vous devez créer les partages de fichiers sur des chemins d’accès au volume pour garantir une montée en charge SMB.
* Ordinateurs clients doivent exécuter Windows® 8 ou Windows Server 2012, qui incluent le client SMB mis à jour qui prend en charge une disponibilité continue.

>[!NOTE]
>Les clients de bas niveau peuvent se connecter aux partages de fichiers dont la propriété de l’autorité de certification, mais le basculement transparent ne sera pas pris en charge pour ces clients.

Les fonctionnalités SMB Multichannel nécessitent la configuration suivante :

* Au moins deux ordinateurs exécutant Windows Server 2012 sont nécessaires. Aucune fonctionnalité supplémentaire n’est à installer ; la technologie est activée par défaut.
* Pour plus d’informations sur les configurations réseau recommandées, consultez la section Voir aussi à la fin de cette rubrique.

SMB Direct nécessite la configuration suivante :

* Au moins deux ordinateurs exécutant Windows Server 2012 sont nécessaires. Aucune fonctionnalité supplémentaire n’est à installer ; la technologie est activée par défaut.
* Des cartes réseau dotées de la technologie RDMA sont requises. Trois types de carte différents sont actuellement disponibles : iWARP, Infiniband ou RoCE (RDMA over Converged Ethernet).

## <a name="more-information"></a>Informations supplémentaires

La liste suivante fournit des ressources supplémentaires sur le web, concernant SMB et les technologies connexes dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.

* [Stockage dans Windows Server](../storage.md)
* [Serveur de fichiers de montée en puissance pour les données d’Application](../../failover-clustering/sofs-overview.md)
* [Améliorer les performances d’un serveur de fichiers avec SMB Direct](smb-direct.md)
* [Déployer Hyper-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Déployer SMB Multichannel](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Déploiement de serveurs de fichiers rapides et efficaces pour les Applications serveur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB : Guide de dépannage](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)