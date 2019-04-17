---
title: "Modifier la durée de mise en cache des références par les clients"
description: "Cet article explique comment modifier la durée de mise en cache des références par les clients"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1f70a5a1a6770cc1bead66b5543f02e4b29b8895
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="change-the-amount-of-time-that-clients-cache-referrals"></a>Modifier la durée de mise en cache des références par les clients

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Une référence constitue une liste de cibles, organisée selon un ordre particulier, que l’ordinateur client reçoit à partir d’un contrôleur de domaine ou d’un serveur d’espace de noms lorsque l’utilisateur accède à une racine d’espace de noms ou à un dossier avec cibles dans l’espace de noms. Vous pouvez régler la durée pendant laquelle les clients mettent en cache une référence avant d’en demander une nouvelle.

## <a name="to-change-the-amount-of-time-that-clients-cache-namespace-root-referrals"></a>Pour modifier la durée de mise en cache des références de racines d’espaces de noms par les clients

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **GestionDFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un espace de noms, puis cliquez sur **Propriétés**.

3.  Sous l’onglet **Références**, dans la zone de texte **Durée du cache (en secondes)**, tapez la durée, en secondes, pendant laquelle les références de dossiers d’espaces de noms sont mises en cache par les clients. Le paramètre par défaut est de 300secondes (cinq minutes).

> [!TIP]
> Pour modifier la durée de mise en cache des références de racines d’espaces de noms par les clients à l’aide de Windows PowerShell, utilisez l’applet de commande [Set-DfsnRoot TimeToLiveSec](https://technet.microsoft.com/library/jj884281.aspx). Ces applets de commande ont été introduites dans WindowsServer2012.

## <a name="to-change-the-amount-of-time-that-clients-cache-folder-referrals"></a>Pour modifier la durée de mise en cache des références de dossiers par les clients

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **GestionDFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un dossier doté de cibles, puis cliquez sur **Propriétés**.

3.  Sous l’onglet **Références**, dans la zone de texte **Durée du cache (en secondes)**, tapez la durée, en secondes, pendant laquelle les références de dossiers sont mises en cache par les clients. Le paramètre par défaut est de 1800secondes (30minutes).

## <a name="see-also"></a>Articles associés

-   [Réglage des espaces de nomsDFS](tuning-dfs-namespaces.md)
-   [Déléguer les autorisations de gestion pour les espaces de nomsDFS](delegate-management-permissions-for-dfs-namespaces.md)


