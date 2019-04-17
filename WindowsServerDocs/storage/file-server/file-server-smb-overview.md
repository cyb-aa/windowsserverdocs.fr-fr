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
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233485"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Vue d’ensemble du partage de fichiers à l’aide du protocole SMB 3 dans Windows Server

>S’applique à: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cette rubrique décrit la fonctionnalité 3.0 SMB dans Windows Server® 2012, Windows Server 2012 R2 et Windows Server 2016 — pratique utilise pour la fonctionnalité, les plus importantes de nouvelles ou mises à jour de la fonctionnalité dans cette version par rapport aux versions précédentes et le matériel configuration requise.

## <a name="feature-description"></a>Description des fonctionnalités

Le protocole SMB (Server Message Block) est un protocole de partage de fichiers réseau qui permet à des applications installées sur un ordinateur d’accéder en lecture et en écriture à des fichiers et de solliciter des services auprès de programmes serveur sur un réseau informatique. Le protocole SMB peut être utilisé en plus du protocole TCP/IP ou d‘autres protocoles réseau. Grâce à lui, une application (ou bien l’utilisateur d’une application) peut accéder à des fichiers ou d’autres ressources sur un serveur distant. Les applications peuvent ainsi lire, créer et mettre à jour des fichiers sur le serveur distant. Il permet aussi de communiquer avec n’importe quel programme serveur configuré pour recevoir une demande de client SMB. Windows Server 2012 présente la nouvelle version 3.0 du protocole SMB.

## <a name="practical-applications"></a>Applications pratiques

Cette section décrit certains nouveaux moyens pratique d’utiliser le protocole SMB 3.0.

* **Stockage des fichiers pour la virtualisation (Hyper-V™ sur SMB)**. Hyper-V peut stocker les fichiers des machines virtuelles, telles que la configuration, les fichiers de disque dur (VHD) virtuels et captures instantanées, partages de fichiers via le protocole SMB 3.0. Ce peut être utilisé pour les serveurs de fichiers autonomes et les serveurs de fichiers en cluster qui utilisent Hyper-V avec le stockage des fichiers partagés pour le cluster.
* **Microsoft SQL Server sur SMB**. SQL Server peut stocker les fichiers de base de données utilisateur sur les partages de fichiers SMB. Actuellement, il est pris en charge avec SQL Server 2008 R2 pour les serveurs SQL autonomes. Les versions à venir de SQL Server ajoute la prise en charge pour les serveurs en clusters SQL et les bases de données système.
* **Stockage traditionnel pour les données de l’utilisateur final**. Le protocole SMB 3.0 apporte des améliorations à l’information (ou client) des charges de travail. Ces améliorations incluent la réduction de la latence d’applications rencontrés par les utilisateurs lors de l’accès aux données sur les réseaux étendus (WAN) et la protection des données à partir de l’écoute des attaques.

## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

Pour plus d’informations sur les fonctionnalités nouvelles et modifiées dans Windows Server 2012 R2, voir [What ' s New in SMB dans Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

SMB dans Windows Server 2012 et Windows Server 2016 inclut le protocole SMB 3.0 et nombreuses nouvelles améliorations qui sont décrits dans le tableau suivant.

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
<th><p>Résumé</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Basculement Transparent SMB</p></td>
<td><p>Nouveau</p></td>
<td><p>Permet aux administrateurs d’effectuer la maintenance matérielle ou logicielle de nœuds dans un serveur de fichiers en cluster sans interrompre le stockage des données sur ces partages de fichiers des applications serveur. En outre, si une défaillance matérielle ou logicielle se produit sur un nœud du cluster, clients SMB en toute transparence se reconnectent à un autre nœud du cluster sans interrompre les applications serveur qui stockent des données sur ces partages de fichiers.</p></td>
</tr>
<tr class="even">
<td><p>SMB montée en puissance parallèle</p></td>
<td><p>Nouveau</p></td>
<td><p>À l’aide de Cluster partagés Volumes (CSV) version 2, les administrateurs peuvent créer des partages de fichiers qui fournissent un accès simultané aux fichiers de données, avec direct e/s par le biais de tous les nœuds dans un cluster de serveurs de fichiers. Cela offre une meilleure utilisation de la bande passante réseau et l’équilibrage de charge des clients de serveur de fichiers et optimise les performances pour les applications serveur.</p></td>
</tr>
<tr class="odd">
<td><p>SMB multicanal</p></td>
<td><p>Nouveau</p></td>
<td><p>Si plusieurs chemins sont disponibles entre le client SMB 3.0 et le serveur SMB 3.0 permet d’agrégation de bande passante réseau et de tolérance de panne de réseau. Cela permet aux applications de serveur tirer pleinement parti de toute la bande passante réseau disponible et être résistantes à une défaillance du réseau.</p></td>
</tr>
<tr class="even">
<td><p>SMB Direct</p></td>
<td><p>Nouveau</p></td>
<td><p>Prend en charge l’utilisation de cartes réseau qui ont la possibilité de RDMA et peut fonctionner à la vitesse maximale très faible latence, lors de l’utilisation du processeur très peu. Pour les charges de travail tels que Hyper-V ou Microsoft SQL Server, cela permet à un serveur de fichier distant qui ressemble à celle de stockage local.</p></td>
</tr>
<tr class="odd">
<td><p>Compteurs de performances pour les applications serveur</p></td>
<td><p>Nouveau</p></td>
<td><p>Le nouvel fournissent des compteurs de performances SMB détaillées, par-partage d’informations sur le débit, la latence et e/s par seconde (IOPS), ce qui permet aux administrateurs d’analyser les performances de partages de fichiers SMB 3.0 où les données sont stockées. Ces compteurs sont spécialement conçues pour les applications serveur, telles que Hyper-V et SQL Server, lequel stocker les fichiers sur des partages de fichiers à distance.</p></td>
</tr>
<tr class="even">
<td><p>Optimisation des performances</p></td>
<td><p>Mise à jour</p></td>
<td><p>Le client SMB 3.0 et le serveur SMB 3.0 ont été optimisés pour les e/s de lecture/écriture aléatoire petite, qui est souvent dans les applications serveur telles que SQL Server OLTP. En outre, grande unité maximale de Transmission (MTU) est activée par défaut, ce qui améliore considérablement les performances dans les grandes transferts séquentiels, tels que l’entrepôt de données SQL Server, de sauvegarde de base de données ou de restauration, déploiement ou la copie de disques durs virtuels.</p></td>
</tr>
<tr class="odd">
<td><p>Applets de commande Windows PowerShell SMB spécifiques</p></td>
<td><p>Nouveau</p></td>
<td><p>Applets de commande Windows PowerShell pour les PME/PMI, un administrateur peut gérer des partages de fichiers sur le serveur de fichiers, bout en bout, depuis la ligne de commande.</p></td>
</tr>
<tr class="even">
<td><p>Chiffrement SMB</p></td>
<td><p>Nouveau</p></td>
<td><p>Fournit le chiffrement de bout en bout des données SMB et protège les données des occurrences préviennent sur des réseaux non approuvés. Requiert aucun nouveaux coûts de déploiement et pas nécessaire d’Internet Protocol security (IPsec), matérielle spécialisée ou accélérateurs de réseau étendu. Il peut être configuré par le partage ou pour le serveur de la totalité du fichier et peut être activé pour divers scénarios où les données parcourt les réseaux non approuvés.</p></td>
</tr>
<tr class="odd">
<td><p>Location d’annuaire SMB</p></td>
<td><p>Nouveau</p></td>
<td><p>Améliore les temps de réponse dans les succursales. Avec l’utilisation de baux directory, aller-retour à partir du client vers le serveur est réduits étant donné que les métadonnées récupérées à partir d’un plus courantes du cache de répertoires. Cohérence de cache est conservée, car les clients sont informés lorsque des informations de répertoire sur le serveur changent. Fonctionne avec les scénarios pour <em>HomeFolder</em> (lecture/écriture sans aucun partage) et de <em>composition</em> (en lecture seule au partage).</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>Configuration matérielle requise

Basculement Transparent SMB présente les exigences suivantes:

* Un cluster de basculement exécutant Windows Server 2012 ou Windows Server 2016 avec au moins deux nœuds configurés. Le cluster doit transmettre les tests de validation de cluster inclus dans l’Assistant validation.
* Partages de fichiers doivent être créés avec la propriété disponibilité continue (CA), qui est la valeur par défaut.
* Partages de fichiers doivent être créés sur des chemins d’accès du volume CSV pour atteindre la montée en puissance parallèle SMB.
* Ordinateurs clients doivent exécuter Windows® 8 ou Windows Server 2012, qui incluent le client SMB mis à jour qui prend en charge la disponibilité en continu.

>[!NOTE]
>Les clients de bas niveau peuvent se connecter aux partages de fichiers dont la propriété de l’autorité de certification, mais le basculement transparent ne sera pas prise en charge pour ces clients.

SMB multicanal présente les exigences suivantes:

* Au moins deux ordinateurs exécutant Windows Server 2012 sont requis. Aucune des fonctionnalités supplémentaires ne doivent être installés: la technologie est activée par défaut.
* Pour plus d’informations sur les configurations de réseau recommandés, consultez la section Voir aussi à la fin de cette rubrique Vue d’ensemble.

SMB Direct présente les exigences suivantes:

* Au moins deux ordinateurs exécutant Windows Server 2012 sont requis. Aucune des fonctionnalités supplémentaires ne doivent être installés: la technologie est activée par défaut.
* Cartes réseau avec la fonctionnalité RDMA sont nécessaires. Actuellement, ces cartes sont disponibles dans trois types: iWARP, Infiniband ou RCI (RDMA over Ethernet a convergé).

## <a name="more-information"></a>Informations supplémentaires

La liste suivante fournit des ressources supplémentaires sur le web concernant les PME/PMI et les technologies associées dans Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.

* [Stockage dans WindowsServer](../storage.md)
* [Serveur de fichiers de mise à niveau pour les données d’Application](../../failover-clustering/sofs-overview.md)
* [Améliorer les performances d’un serveur de fichiers avec SMB Direct](smb-direct.md)
* [Déployer Hyper\-V sur SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Déployer SMB multicanal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Déploiement de serveurs de fichiers rapide et efficace pour les Applications serveur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guide de dépannage](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)