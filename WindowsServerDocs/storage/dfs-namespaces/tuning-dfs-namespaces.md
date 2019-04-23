---
title: Réglage des espaces de noms DFS
description: Cet article explique comment régler ou optimiser des espaces de noms DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c11bbf65c3baebebe1e5143a5e694ca752500aca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844630"
---
# <a name="tuning-dfs-namespaces"></a>Réglage des espaces de noms DFS

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Après avoir créé un espace de noms et ajout de dossiers et des cibles, consultez les sections suivantes pour régler ou optimiser la manière dont les références de handles de DFS Namespace et les sondages Services de domaine Active Directory (AD DS) pour les données de l’espace de noms mis à jour :

-   [Activer l’énumération basée sur l’accès à un Namespace](enable-access-based-enumeration-on-a-namespace.md)
-   [Activer ou désactiver les références et la restauration automatique du Client](enable-or-disable-referrals-and-client-failback.md)
-   [Modifier la quantité de temps que les références sont mises en Cache par les Clients](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Définir la méthode de classement pour les cibles dans les références](set-the-ordering-method-for-targets-in-referrals.md)
-   [Définir la priorité pour remplacer l’ordre des références](set-target-priority-to-override-referral-ordering.md)
-   [Optimiser l’interrogation de Namespace](optimize-namespace-polling.md)
-   [À l’aide d’autorisations héritées avec l’énumération basée sur l’accès](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Pour rechercher des dossiers ou des cibles de dossier, sélectionnez un espace de noms, cliquez sur l’onglet **Recherche**, saisissez votre chaîne de recherche dans la zone de texte, puis cliquez sur **Rechercher**.