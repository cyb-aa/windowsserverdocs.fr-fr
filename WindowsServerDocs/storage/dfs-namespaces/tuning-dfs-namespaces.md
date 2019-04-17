---
title: "Réglage des espaces de nomsDFS"
description: "Cet article explique comment régler ou optimiser des espaces de nomsDFS."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4614441fc54913ba5a8b547bbf1ad3e8ce7ee69b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="tuning-dfs-namespaces"></a>Réglage des espaces de nomsDFS

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Après avoir créé un espace de noms et ajouté des dossiers et des cibles, reportez-vous aux sections suivantes pour régler ou optimiser la façon dont l’espace de nomsDFS traite les références et interroge ActiveDirectory DomainServices (ADDS) pour obtenir des données d’espace de noms à jour:

-   [Activer l’énumération basée sur l’accès pour un espace denoms](enable-access-based-enumeration-on-a-namespace.md)
-   [Activer ou désactiver les références et la restauration du client](enable-or-disable-referrals-and-client-failback.md)
-   [Modifier la durée de mise encache des références par les clients](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Définir la méthode de classement des cibles contenues dans les références](set-the-ordering-method-for-targets-in-referrals.md)
-   [Définir la priorité de cible pour remplacer le classement des références](set-target-priority-to-override-referral-ordering.md)
-   [Optimiser l’interrogation des espaces de noms](optimize-namespace-polling.md)
-   [Utilisation d’autorisations héritées avec l’énumération basée sur l’accès](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Pour rechercher des dossiers ou des cibles de dossier, sélectionnez un espace de noms, cliquez sur l’onglet **Recherche**, saisissez votre chaîne de recherche dans la zone de texte, puis cliquez sur **Rechercher**.