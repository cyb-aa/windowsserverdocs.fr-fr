---
title: "Liste de vérification Régler un espace de nomsDFS"
description: "Cet article explique comment optimiser la façon dont l’espace de noms DFS traite les références et interroge les services ADDS pour obtenir des données d’espaces de noms àjour"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 827484c2ffcbb2617626129011a8b510473fe669
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="checklist-tune-a-dfs-namespace"></a>Liste de vérification: Régler un espace de nomsDFS

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Après avoir créé un espace de noms et ajouté des dossiers et des cibles, utilisez la liste de vérification suivante pour régler ou optimiser la façon dont l’espace de nomsDFS traite les références et interroge ActiveDirectory DomainServices (ADDS) pour obtenir des données d’espace de noms àjour.

-   Empêcher les utilisateurs de voir les dossiers d’un espace de noms auquel ils n’ont pas le droit d’accéder. [Activer l’énumération basée sur l’accès pour un espace denoms](enable-access-based-enumeration-on-a-namespace.md) 
-   Autoriser les utilisateurs d’être renvoyés vers un espace de noms ou une cible de dossier quand ils accèdent à un dossier dans l’espace de noms, ou les en empêcher. [Activer ou désactiver les références et la restauration du client](enable-or-disable-referrals-and-client-failback.md) 
-   Régler la durée pendant laquelle les clients mettent en cache une référence avant d’en demander une nouvelle. [Modifier la durée de mise encache des références par les clients](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Optimiser la façon dont les serveurs d’espaces de noms interrogent ADDS pour obtenir les données d’espaces de noms les plus récentes. [Optimiser l’interrogation des espaces de noms](optimize-namespace-polling.md)
-   Utiliser des autorisations héritées pour contrôler quels utilisateurs peuvent afficher les dossiers dans un espace de noms pour lequel l’énumération basée sur l’accès est activée. [Utilisation d’autorisations héritées avec l’énumération basée sur l’accès](using-inherited-permissions-with-access-based-enumeration.md)

Par ailleurs, en utilisant une amélioration des espaces de noms DFS connue sous le nom de «priorité de cible», vous pouvez spécifier l’ordre de priorité des serveurs pour qu’un serveur spécifique soit toujours placé en premier ou en dernier dans la liste des serveurs (appelée «référence») que le client reçoit quand il accède à un dossier avec cibles dans l’espace de noms.

-   Spécifier l’ordre dans lequel les utilisateurs doivent être renvoyés à des cibles de dossier. [Définir la méthode de classement des cibles contenues dans les références](set-the-ordering-method-for-targets-in-referrals.md)
-   Remplacer l’ordre des références pour un serveur d’espaces de noms ou une cible de dossier spécifique. [Définir la priorité de cible pour remplacer le classement des références](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>Articles associés

-   [Espaces de noms](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Liste de vérification: Déployer des espaces de nomsDFS](checklist-deploy-dfs-namespaces.md)
-   [Réglage des espaces de nomsDFS](tuning-dfs-namespaces.md)


