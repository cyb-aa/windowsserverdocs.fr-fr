---
title: Optimiser l’interrogation des espaces de noms
description: Cet article décrit comment optimiser l’interrogation des espaces de noms afin de conserver un espace de noms de domaine cohérent entre les serveurs d’espaces de noms
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8b5a80324851674c8d980bc1b6cda3a5ea51483f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402161"
---
# <a name="optimize-namespace-polling"></a>Optimiser l’interrogation des espaces de noms

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Pour conserver un espace de noms de domaine cohérent entre les serveurs d’espaces de noms, il est nécessaire que ces serveurs interrogent régulièrement les services de domaine Active Directory (AD DS) afin d’obtenir les données d’espace de noms les plus récentes. 

## <a name="to-optimize-namespace-polling"></a>Pour optimiser l’interrogation d’un espace de noms

Utilisez la procédure suivante pour optimiser la manière dont l’interrogation d’un espace de noms se déroule.

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un espace de noms de domaine, puis cliquez sur **Propriétés**.

3.  Sous l’onglet **Avancé**, sélectionnez le type d’optimisation de l’espace de noms, à savoir pour une plus grande cohérence ou pour une meilleure extensibilité.

    -   Choisissez **Optimiser pour la cohérence** si l’espace de noms est hébergé par un maximum de 16 serveurs d’espaces de noms.
    -   Choisissez **Optimiser pour l’extensibilité** dans le cas de plus de 16 serveurs d’espaces de noms. Cela réduit la charge qui s’exerce sur l’émulateur de contrôleur de domaine principal, mais augmente le temps nécessaire à la réplication des modifications de l’espace de noms sur tous les serveurs d’espaces de noms. Tant que les modifications ne sont pas répliquées sur tous les serveurs, les utilisateurs peuvent avoir une vision incohérente de l’espace de noms.

> [!NOTE]
> Pour définir le mode d’interrogation de l’espace de noms à l’aide de Windows PowerShell, utilisez l’applet de commande [Set-DfsnRoot EnableRootScalability](https://technet.microsoft.com/library/jj884281.aspx) , qui a été introduite dans windows server 2012.

## <a name="see-also"></a>Voir aussi

-   [Optimisation des espaces de noms DFS](tuning-dfs-namespaces.md)
-   [Déléguer les autorisations de gestion pour les espaces de noms DFS](delegate-management-permissions-for-dfs-namespaces.md)