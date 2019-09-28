---
title: Liste de vérification Régler un espace de noms DFS
description: Cet article explique comment optimiser la façon dont l’espace de noms DFS traite les références et interroge les services AD DS pour obtenir des données d’espaces de noms à jour
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8a4c581801cbb1dcfe3db2813fb66fb4514d6434
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402183"
---
# <a name="checklist-tune-a-dfs-namespace"></a>Liste de vérification : Paramétrer un espace de noms DFS

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Après la création d’un espace de noms et l’ajout de dossiers et de cibles, utilisez la liste de vérification suivante pour ajuster ou optimiser la façon dont l’espace de noms DFS gère les références et les sondages Active Directory Domain Services (AD DS) pour les données d’espace de noms mises à jour.

-   Empêcher les utilisateurs de voir les dossiers d’un espace de noms auquel ils n’ont pas le droit d’accéder. [Activer l’énumération basée sur l’accès sur un espace de noms](enable-access-based-enumeration-on-a-namespace.md) 
-   Autoriser les utilisateurs d’être renvoyés vers un espace de noms ou une cible de dossier quand ils accèdent à un dossier dans l’espace de noms, ou les en empêcher. [Activer ou désactiver les références et la restauration du client](enable-or-disable-referrals-and-client-failback.md) 
-   Régler la durée pendant laquelle les clients mettent en cache une référence avant d’en demander une nouvelle. [Modifier la durée de mise en cache des références des clients](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Optimiser la façon dont les serveurs d’espaces de noms interrogent AD DS pour obtenir les données d’espaces de noms les plus récentes. [Optimiser l’interrogation des espaces de noms](optimize-namespace-polling.md)
-   Utiliser des autorisations héritées pour contrôler quels utilisateurs peuvent afficher les dossiers dans un espace de noms pour lequel l’énumération basée sur l’accès est activée. [Utilisation des autorisations héritées avec l’énumération basée sur l’accès](using-inherited-permissions-with-access-based-enumeration.md)

En outre, à l’aide d’une amélioration des espaces de noms DFS appelée priorité cible, vous pouvez spécifier la priorité des serveurs afin qu’un serveur spécifique soit toujours placé en premier ou en dernier dans la liste des serveurs (appelée référence) que le client reçoit lorsqu’il accède à un dossier. avec les cibles dans l’espace de noms.

-   Spécifier l’ordre dans lequel les utilisateurs doivent être renvoyés à des cibles de dossier. [Définir la méthode de classement des cibles contenues dans les références](set-the-ordering-method-for-targets-in-referrals.md)
-   Remplacer l’ordre des références pour un serveur d’espaces de noms ou une cible de dossier spécifique. [Définir la priorité de cible pour remplacer le classement des références](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>Voir aussi

-   [Espaces](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Liste de vérification : déployer les espaces de noms DFS](checklist-deploy-dfs-namespaces.md)
-   [Optimisation des espaces de noms DFS](tuning-dfs-namespaces.md)


