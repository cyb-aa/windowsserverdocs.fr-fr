---
title: "Optimiser l’interrogation des espaces de noms"
description: "Cet article décrit comment optimiser l’interrogation des espaces de noms afin de conserver un espace de noms de domaine cohérent entre les serveurs d’espaces de noms"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: be9a7623089d99a5b9c791b219dbcc64d61466cf
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="optimize-namespace-polling"></a>Optimiser l’interrogation des espaces de noms

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Pour conserver un espace de noms de domaine cohérent entre les serveurs d’espaces de noms, il est nécessaire que ces serveurs interrogent régulièrement les Active Directory Domain Services (ADDS) afin d’obtenir les données d’espace de noms les plus récentes. 

## <a name="to-optimize-namespace-polling"></a>Pour optimiser l’interrogation d’un espace de noms

Utilisez la procédure suivante pour optimiser la manière dont l’interrogation d’un espace de noms se déroule.

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **GestionDFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un espace de noms de domaine, puis cliquez sur **Propriétés**.

3.  Sous l’onglet **Avancé**, sélectionnez le type d’optimisation de l’espace de noms, à savoir pour une plus grande cohérence ou pour une meilleure extensibilité.

    -   Choisissez **Optimiser pour la cohérence** si l’espace de noms est hébergé par un maximum de 16serveurs d’espaces de noms.
    -   Choisissez **Optimiser pour l’extensibilité** dans le cas de plus de 16serveurs d’espaces de noms. Cela réduit la charge qui s’exerce sur l’émulateur de contrôleur de domaine principal, mais augmente le temps nécessaire à la réplication des modifications de l’espace de noms sur tous les serveurs d’espaces de noms. Tant que les modifications ne sont pas répliquées sur tous les serveurs, les utilisateurs peuvent avoir une vision incohérente de l’espace de noms.

> [!NOTE]
> Pour définir le mode d’interrogation des espaces de noms à l’aide de WindowsPowerShell, utilisez l’applet de commande [Set-DfsnRoot EnableRootScalability](https://technet.microsoft.com/library/jj884281.aspx) qui a été introduite dans WindowsServer2012.

## <a name="see-also"></a>Articles associés

-   [Réglage des espaces de nomsDFS](tuning-dfs-namespaces.md)
-   [Déléguer les autorisations de gestion pour les espaces de nomsDFS](delegate-management-permissions-for-dfs-namespaces.md)