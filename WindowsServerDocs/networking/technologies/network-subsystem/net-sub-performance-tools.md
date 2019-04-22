---
title: Outils d’analyse des performances pour les charges de travail de réseau
description: Cette rubrique fait partie du guide de réglage de performances du sous-système de réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 07/16/2018
ms.openlocfilehash: e71c5f34041145907c30b279dc91a94c03c2abed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824930"
---
# <a name="performance-tools-for-network-workloads"></a>Outils d’analyse des performances pour les charges de travail de réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les outils de performances.

Cette rubrique contient les sections relatives au Client à l’outil le trafic du serveur, la taille de fenêtre de TCP/IP et Microsoft Server Performance Advisor.

##  <a name="bkmk_tuning"></a> Client à l’outil de trafic du serveur

Le Client pour le trafic serveur \(ctsTraffic\) outil vous offre la possibilité de créer et de vérifier le trafic réseau.

Pour plus d’informations et pour télécharger l’outil, consultez [ctsTraffic (trafic de Client à serveur)](https://github.com/Microsoft/ctsTraffic).
  
##  <a name="bkmk_size"></a> Taille de la fenêtre de TCP/IP

Pour les adaptateurs de 1 Go, les paramètres affichés dans le tableau précédent doivent fournir un débit relativement élevé, car NTttcp définit la taille de fenêtre TCP par défaut à 64 Ko via le processeur logique spécifique \(SO_RCVBUF\) pour la connexion. Cela fournit de bonnes performances sur un réseau à faible latence.  

En revanche, pour les réseaux à latence élevée ou pour les cartes de 10 Go, la valeur de taille de fenêtre TCP par défaut pour NTttcp génère inférieure aux performances optimales. Dans les deux cas, vous devez ajuster la taille de fenêtre TCP à autoriser pour le produit de délai de la bande passante supérieure.  

Vous pouvez définir statiquement la taille de fenêtre TCP pour une valeur élevée à l’aide de la **-rb** option. Cette option désactive le réglage automatique de la fenêtre TCP, et nous vous recommandons d’utiliser uniquement si l’utilisateur prend complètement en charge le changement résultant de comportement de TCP/IP. Par défaut, la taille de fenêtre TCP est définie sur une valeur suffisante et ajuste uniquement sous une charge importante ou sur des liaisons à latence élevée.  

##  <a name="bkmk_advisor"></a> Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \(SPA\) permet aux administrateurs informatiques collecter des mesures pour identifier, de comparer et de diagnostiquer les problèmes de performances potentiels dans un Windows Server 2016 Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, ou le déploiement de Windows Server 2008. 

SPA génère des graphiques et des rapports de Diagnostics complets, et il fournit des recommandations pour vous aider à rapidement analysent les problèmes et développement des actions correctives.  
  
 Pour plus d’informations et pour télécharger le conseiller, consultez [Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) dans le centre de développement de matériel Windows.

Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances réseau sous-système](net-sub-performance-top.md).