---
title: Outils de performance pour les charges de travail de réseau
description: Cette rubrique fait partie du guide de réglage des performances sous-système réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 81c31871b3dfa4644690fe074ae15eaaa680d7f2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="performance-tools-for-network-workloads"></a>Outils de performance pour les charges de travail de réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les outils de performance.

Cette rubrique contient les sections sur le Client à l’outil le trafic du serveur, la taille de fenêtre TCP/IP et Microsoft Server Performance Advisor.

##  <a name="bkmk_tuning"></a>Client à l’outil de trafic de serveur

Le Client à l’outil \(ctsTraffic\) de trafic de serveur vous offre la possibilité de créer et vérifier le trafic réseau.

Pour plus d’informations et pour télécharger l’outil, voir [ctsTraffic (trafic Client-serveur)](http://ctstraffic.codeplex.com/).
  
##  <a name="bkmk_size"></a>Taille de la fenêtre de TCP/IP

Pour les cartes de 1 Go, les paramètres affichés dans le tableau précédent doivent fournir un débit satisfaisant car NTttcp définit la taille de fenêtre TCP par défaut à 64 Ko par le biais d’un processeur logique spécifique option \(SO_RCVBUF\) pour la connexion. Cela offre de bonnes performances sur un réseau à faible latence.  

En revanche, pour les réseaux à latence élevée, ou pour les cartes de 10 Go, la valeur de taille de fenêtre TCP par défaut pour NTttcp donne inférieure à des performances optimales. Dans les deux cas, vous devez régler la taille de fenêtre TCP pour permettre le produit de délai de la bande passante plus large.  

Vous pouvez statiquement définir la taille de fenêtre TCP sur une valeur élevée à l’aide de la **-rb** option. Cette option désactive le réglage automatique de la fenêtre TCP, et nous vous recommandons d’utiliser uniquement si l’utilisateur comprend parfaitement le changement de comportement TCP/IP résultant. Par défaut, la taille de fenêtre TCP est définie sur une valeur suffisamment et ajuste uniquement surchargée ou sur des liaisons à latence élevée.  

##  <a name="bkmk_advisor"></a>Microsoft Server Performance Advisor

\(SPA\) Microsoft Server Performance Advisor permet aux administrateurs informatiques de collecter des mesures pour identifier, de comparer et de diagnostiquer les éventuels problèmes de performances dans un Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou déploiement de Windows Server 2008. 

SPA génère des graphiques et des rapports de diagnostic complets, et il fournit des recommandations pour vous aider à rapidement analyser les problèmes et développement des actions correctives.  
  
 Pour plus d’informations et pour télécharger le conseiller, voir [Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) dans le centre de développement matériel Windows.

Pour obtenir des liens vers toutes les rubriques de ce guide, voir [réglage des performances réseau sous-système](net-sub-performance-top.md).