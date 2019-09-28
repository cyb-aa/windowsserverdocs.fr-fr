---
title: Modifier la durée de mise en cache des références par les clients
description: Cet article explique comment modifier la durée de mise en cache des références par les clients
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c6fcf64dc15404ca59e3ce5552b258f782441cfb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402265"
---
# <a name="change-the-amount-of-time-that-clients-cache-referrals"></a>Modifier la durée de mise en cache des références par les clients

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Une référence constitue une liste de cibles, organisées selon un ordre particulier, que l’ordinateur client reçoit à partir d’un contrôleur de domaine ou d’un serveur d’espaces de noms lorsque l’utilisateur accède à une racine d’espace de noms ou à un dossier avec cibles dans l’espace de noms. Vous pouvez régler la durée pendant laquelle les clients mettent en cache une référence avant d’en demander une nouvelle.

## <a name="to-change-the-amount-of-time-that-clients-cache-namespace-root-referrals"></a>Pour modifier la durée de mise en cache des références de racines d’espaces de noms par les clients

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un espace de noms, puis cliquez sur **Propriétés**.

3.  Sous l’onglet **Références**, dans la zone de texte **Durée du cache (en secondes)** , tapez la durée, en secondes, pendant laquelle les références de racines d’espaces de noms sont mises en cache par les clients. Le paramètre par défaut est de 300 secondes (cinq minutes).

> [!TIP]
> Pour modifier la durée de mise en cache des références de racines d’espaces de noms par les clients à l’aide de Windows PowerShell, utilisez l’applet de commande [Set-DfsnRoot TimeToLiveSec](https://technet.microsoft.com/library/jj884281.aspx). Ces applets de commande ont été introduites dans Windows Server 2012.

## <a name="to-change-the-amount-of-time-that-clients-cache-folder-referrals"></a>Pour modifier la durée de mise en cache des références de dossiers par les clients

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **Gestion DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un dossier doté de cibles, puis cliquez sur **Propriétés**.

3.  Sous l’onglet **Références**, dans la zone de texte **Durée du cache (en secondes)** , tapez la durée, en secondes, pendant laquelle les références de dossiers sont mises en cache par les clients. Le paramètre par défaut est de 1 800 secondes (30 minutes).

## <a name="see-also"></a>Voir aussi

-   [Optimisation des espaces de noms DFS](tuning-dfs-namespaces.md)
-   [Déléguer les autorisations de gestion pour les espaces de noms DFS](delegate-management-permissions-for-dfs-namespaces.md)


