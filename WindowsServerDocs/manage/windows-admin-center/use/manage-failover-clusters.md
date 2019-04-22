---
title: Gérer des Clusters de basculement avec Windows Admin Center
description: Gérer des Clusters de basculement avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7e14581f7f6b14b0cf39308de236b68a07e8c9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824070"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>Gérer des Clusters de basculement avec Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

> [!Tip]
> Vous débutez dans Windows Admin Center ?
> [Découvrez-en davantage sur Windows Admin Center](../understand/windows-admin-center.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## <a name="managing-failover-clusters"></a>La gestion des clusters de basculement
[Le clustering de basculement](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) est une fonctionnalité de Windows Server qui vous permet de regrouper plusieurs serveurs dans un cluster à tolérance de pannes pour accroître la disponibilité et l’évolutivité des applications et services, tels que serveur de fichiers avec montée en puissance, Hyper-V et Microsoft SQL Server.

Bien que vous pouvez gérer les nœuds de cluster de basculement en tant que serveurs individuels en les ajoutant en tant que [connexions au serveur](manage-servers.md) dans Windows Admin Center, vous pouvez également les ajouter comme clusters de basculement pour afficher et gérer les ressources de cluster, stockage, réseau, nœuds, rôles, les machines virtuelles et les commutateurs virtuels.

![Écran de vue d’ensemble de cluster de basculement](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>Ajout d’un cluster de basculement à Windows Admin Center
Pour ajouter un cluster Windows Admin Center :

1. Cliquez sur **+ ajouter** sous toutes les connexions.
2. Choisissez d’ajouter un **connexion de basculement**.
3. Tapez le nom du cluster et, si vous y êtes invité, les informations d’identification à utiliser.
4. Avoir la possibilité d’ajouter les nœuds de cluster en tant que connexions serveur individuel dans Windows Admin Center.
5. Cliquez sur **envoyer** se termine.

Le cluster sera ajouté à votre liste des connexions dans la page Vue d’ensemble. Cliquez dessus pour vous connecter au cluster.

> [!NOTE]
> Vous pouvez également gérer hyperconvergé en cluster en ajoutant le cluster comme un [connexion Hyper-Converged Cluster](manage-hyper-converged.md) dans Windows Admin Center.

## <a name="tools"></a>Outils

Les outils suivants sont disponibles pour les connexions de cluster de basculement :

| Tool | Description |
| ---- | ----------- |
| Vue d'ensemble | Afficher les détails du cluster de basculement et de gérer les ressources de cluster |
| Disques | Partagé de cluster de la vue disques et volumes |
| Réseaux | Afficher les réseaux du cluster |
| Nœuds | Afficher et gérer les nœuds de cluster |
| Rôles | Gérer les rôles de cluster ou créer un rôle vide |
| Mises à jour | Gérer les mises à jour adaptée aux clusters |
| [Machines virtuelles](manage-virtual-machines.md) | Afficher et gérer des machines virtuelles |
| Commutateurs virtuels | Afficher et gérer des commutateurs virtuels |

## <a name="more-coming"></a>Bientôt plus

Gestion du cluster de basculement dans Windows Admin Center est activement en cours de développement et de nouvelles fonctionnalités seront ajoutées dans un avenir proche. Vous pouvez afficher l’état et voter pour les fonctionnalités dans UserVoice :

|Demande de fonctionnalité|
|-------|
| [Afficher plus d’Infos disque en cluster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Prend en charge les actions de cluster supplémentaire](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Prend en charge les clusters convergées exécutant Hyper-V et le serveur de fichiers avec montée en puissance sur des clusters différents](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Cache de bloc partagé de vue](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Afficher tout ou proposer la nouvelle fonctionnalité](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |