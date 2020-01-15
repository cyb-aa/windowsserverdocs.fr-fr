---
title: Gérer les clusters de basculement avec le centre d’administration Windows
description: Gérer les clusters de basculement avec le centre d’administration Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: b7f015ac4c9906447069501bf0922b36306a51d7
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950493"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>Gérer les clusters de basculement avec le centre d’administration Windows

>S’applique à : Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Vous débutez dans Windows Admin Center ?
> [Téléchargez ou apprenez-en davantage sur le centre d’administration Windows](../overview.md).

## <a name="managing-failover-clusters"></a>Gestion des clusters de basculement
Le [clustering de basculement](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) est une fonctionnalité de Windows Server qui vous permet de regrouper plusieurs serveurs dans un cluster à tolérance de pannes pour augmenter la disponibilité et l’extensibilité des applications et des services tels que serveur de fichiers avec montée en puissance parallèle, Hyper-V et Microsoft SQL Server.

Bien que vous puissiez gérer des nœuds de cluster de basculement en tant que serveurs individuels en les ajoutant en tant que [connexions serveur](manage-servers.md) dans le centre d’administration Windows, vous pouvez également les ajouter en tant que clusters de basculement pour afficher et gérer les ressources, le stockage, le réseau, les nœuds, les ordinateurs virtuels et les commutateurs virtuels de cluster.

![Écran de présentation du cluster de basculement](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>Ajout d’un cluster de basculement au centre d’administration Windows
Pour ajouter un cluster au centre d’administration Windows :

1. Cliquez sur **+ Ajouter** sous toutes les connexions.
2. Choisissez d’ajouter une **connexion de basculement**.
3. Tapez le nom du cluster et, si vous y êtes invité, les informations d’identification à utiliser.
4. Vous aurez la possibilité d’ajouter les nœuds de cluster en tant que connexions de serveur individuelles dans le centre d’administration Windows.
5. Cliquez sur **Envoyer** pour terminer.

Le cluster est ajouté à votre liste de connexions dans la page vue d’ensemble. Cliquez dessus pour vous connecter au cluster.

> [!NOTE]
> Vous pouvez également gérer les clusters hyper-convergés en ajoutant le cluster en tant que [connexion de cluster hyper-convergé](manage-hyper-converged.md) dans le centre d’administration Windows.

## <a name="tools"></a>Outils

Les outils suivants sont disponibles pour les connexions de cluster de basculement :

| Outil | Description |
| ---- | ----------- |
| Vue d'ensemble | Afficher les détails du cluster de basculement et gérer les ressources de cluster |
| Disques | Afficher les volumes et les disques partagés de cluster |
| Réseaux | Afficher les réseaux dans le cluster |
| Nœuds | Afficher et gérer les nœuds de cluster |
| Rôles | Gérer les rôles de cluster ou créer un rôle vide |
| Mises à jour | Gérer les mises à jour prenant en charge les clusters (nécessite [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)) |
| [Machines virtuelles](manage-virtual-machines.md) | Afficher et gérer des machines virtuelles |
| Commutateurs virtuels | Afficher et gérer les commutateurs virtuels |

## <a name="more-coming"></a>Plus à venir

La gestion du cluster de basculement dans le centre d’administration Windows est activement en cours de développement et de nouvelles fonctionnalités seront ajoutées dans un avenir proche. Vous pouvez afficher l’État et voter pour les fonctionnalités de UserVoice :

|Demande de fonctionnalité|
|-------|
| [Afficher plus d’informations sur le disque en cluster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Prendre en charge des actions de cluster supplémentaires](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Prise en charge des clusters convergents exécutant Hyper-V et Serveur de fichiers avec montée en puissance parallèle sur des clusters différents](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Afficher le cache de blocs CSV](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Afficher tout ou proposer une nouvelle fonctionnalité](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |