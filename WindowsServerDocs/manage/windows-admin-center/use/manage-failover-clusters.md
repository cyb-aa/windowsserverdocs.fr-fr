---
title: Gérer les Clusters de basculement avec Windows Admin Center
description: Gérer les Clusters de basculement avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 63f5fca8e7ef63200e01cfc6e00a979e7f045b51
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296671"
---
# Gérer les Clusters de basculement avec Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Vous débutez dans Windows Admin Center?
> [Découvrez-en davantage sur Windows Admin Center](../understand/windows-admin-center.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## La gestion des clusters de basculement
[Le clustering de basculement](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) est une fonctionnalité de Windows Server qui vous permet de regrouper plusieurs serveurs dans un cluster à tolérance de pannes pour augmenter la disponibilité et l’évolutivité des applications et services tels que Hyper-V, serveur de fichiers avec montée en et Microsoft SQL Server.

Pendant que vous pouvez gérer les nœuds de cluster de basculement en tant que serveurs individuels en les ajoutant en tant que [les connexions de serveur](manage-servers.md) dans Windows Admin Center, vous pouvez également les ajouter comme des clusters de basculement pour afficher et gérer les ressources de cluster, stockage, réseau, nœuds, rôles, virtuels ordinateurs et des commutateurs virtuels.

![Écran de vue d’ensemble du cluster de basculement](../media/manage-failover-clusters/fcm-overview.png)

## Ajout d’un cluster de basculement dans Windows Admin Center
Pour ajouter un cluster à Windows Admin Center:

1. Cliquez sur **+ Ajouter** sous toutes les connexions.
2. Choisir d’ajouter une **Connexion de basculement**.
3. Tapez le nom du cluster et, si vous y êtes invité, les informations d’identification à utiliser.
4. Vous aurez la possibilité pour ajouter les nœuds de cluster en tant que les connexions de serveur individuel dans Windows Admin Center.
5. Cliquez sur **Envoyer** pour terminer.

Le cluster sera ajouté à votre liste de connexion sur la page de présentation. Cliquez dessus pour se connecter au cluster.

> [!NOTE]
> Vous pouvez également gérer hyperconvergé mis en cluster en ajoutant le cluster en tant qu’une [connexion de Cluster hyperconvergé](manage-hyper-converged.md) dans Windows Admin Center.

## Outils

Les outils suivants sont disponibles pour les connexions de cluster de basculement:

| Outil | Description |
| ---- | ----------- |
| Vue d'ensemble | Afficher les détails de cluster de basculement et de gérer les ressources de cluster |
| Disques | Partagés de cluster de vue disques et les volumes |
| Réseaux | Afficher les réseaux du cluster |
| Nœuds | Afficher et gérer les nœuds de cluster |
| Rôles | Gérer des rôles de cluster ou créer un rôle vide |
| Mises à jour | Gérer les mises à jour adaptée aux clusters (nécessite [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)) |
| [Machines virtuelles](manage-virtual-machines.md) | Afficher et gérer des machines virtuelles |
| Commutateurs virtuels | Afficher et gérer des commutateurs virtuels |

## Entrée plus

Gestion du cluster de basculement dans Windows Admin Center est activement en cours de développement et de nouvelles fonctionnalités sont ajoutées à l’avenir proche. Vous pouvez afficher l’état et les votes pour les fonctionnalités dans UserVoice:

|Demande de fonctionnalité|
|-------|
| [Afficher plus d’Infos disque en cluster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Prise en charge des actions supplémentaires de cluster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Prise en charge des clusters convergés exécutant Hyper-V et le serveur de fichiers avec montée en sur les clusters de différentes](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Vue CSV bloc cache](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Voir tous les ou proposer la nouvelle fonctionnalité](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |