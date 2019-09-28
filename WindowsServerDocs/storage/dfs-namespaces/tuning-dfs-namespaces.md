---
title: Réglage des espaces de noms DFS
description: Cet article explique comment régler ou optimiser des espaces de noms DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 011512deaeb99ded7d0bfc32a48f19ab3b622475
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386149"
---
# <a name="tuning-dfs-namespaces"></a>Réglage des espaces de noms DFS

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Après la création d’un espace de noms et l’ajout de dossiers et de cibles, reportez-vous aux sections suivantes pour ajuster ou optimiser la façon dont l’espace de noms DFS gère les références et les sondages Active Directory Domain Services (AD DS) pour les données d’espace de noms mises à jour :

-   [Activer l’énumération basée sur l’accès sur un espace de noms](enable-access-based-enumeration-on-a-namespace.md)
-   [Activer ou désactiver les références et la restauration du client](enable-or-disable-referrals-and-client-failback.md)
-   [Modifier la durée de mise en cache des références des clients](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Définir la méthode de classement des cibles contenues dans les références](set-the-ordering-method-for-targets-in-referrals.md)
-   [Définir la priorité de cible pour remplacer le classement des références](set-target-priority-to-override-referral-ordering.md)
-   [Optimiser l’interrogation des espaces de noms](optimize-namespace-polling.md)
-   [Utilisation des autorisations héritées avec l’énumération basée sur l’accès](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Pour rechercher des dossiers ou des cibles de dossier, sélectionnez un espace de noms, cliquez sur l’onglet **Recherche**, saisissez votre chaîne de recherche dans la zone de texte, puis cliquez sur **Rechercher**.