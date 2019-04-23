---
title: Liste de vérification Régler un espace de noms DFS
description: Cet article explique comment optimiser la façon dont l’espace de noms DFS traite les références et interroge les services AD DS pour obtenir des données d’espaces de noms à jour
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5e2d43f75a64a6a7950539c14386c6a037bc3c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872640"
---
# <a name="checklist-tune-a-dfs-namespace"></a>Liste de vérification : Paramétrer un espace de noms DFS

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Après la création d’un espace de noms et en ajoutant des dossiers et des cibles, utilisez la liste de vérification suivante pour régler ou optimiser la façon dont l’espace de noms DFS gère les références et interroge les Services de domaine Active Directory (AD DS) pour les données de l’espace de noms mis à jour.

-   Empêcher les utilisateurs de voir les dossiers d’un espace de noms auquel ils n’ont pas le droit d’accéder. [Activer l’énumération basée sur l’accès à un Namespace](enable-access-based-enumeration-on-a-namespace.md) 
-   Autoriser les utilisateurs d’être renvoyés vers un espace de noms ou une cible de dossier quand ils accèdent à un dossier dans l’espace de noms, ou les en empêcher. [Activer ou désactiver les références et la restauration automatique du Client](enable-or-disable-referrals-and-client-failback.md) 
-   Régler la durée pendant laquelle les clients mettent en cache une référence avant d’en demander une nouvelle. [Modifier la quantité de temps que les références sont mises en Cache par les Clients](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Optimiser la façon dont les serveurs d’espaces de noms interrogent AD DS pour obtenir les données d’espaces de noms les plus récentes. [Optimiser l’interrogation de Namespace](optimize-namespace-polling.md)
-   Utiliser des autorisations héritées pour contrôler quels utilisateurs peuvent afficher les dossiers dans un espace de noms pour lequel l’énumération basée sur l’accès est activée. [À l’aide d’autorisations héritées avec l’énumération basée sur l’accès](using-inherited-permissions-with-access-based-enumeration.md)

En outre, grâce à une amélioration d’espaces de noms DFS appelée priorité cible, vous pouvez spécifier la priorité de serveurs afin qu’un serveur spécifique est toujours placé tout d’abord ou en dernier dans la liste des serveurs (appelée « référence ») que le client reçoit lorsqu’il accède à un dossier avec des cibles dans l’espace de noms.

-   Spécifier l’ordre dans lequel les utilisateurs doivent être renvoyés à des cibles de dossier. [Définir la méthode de classement pour les cibles dans les références](set-the-ordering-method-for-targets-in-referrals.md)
-   Remplacer l’ordre des références pour un serveur d’espaces de noms ou une cible de dossier spécifique. [Définir la priorité pour remplacer l’ordre des références](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>Voir aussi

-   [Espaces de noms](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Liste de vérification : Déployer des espaces de noms DFS](checklist-deploy-dfs-namespaces.md)
-   [Réglage des espaces de noms DFS](tuning-dfs-namespaces.md)


