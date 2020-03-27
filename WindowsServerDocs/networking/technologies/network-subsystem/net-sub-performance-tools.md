---
title: Outils d’analyse des performances pour les charges de travail de réseau
description: Cette rubrique fait partie du Guide d’optimisation des performances du sous-système réseau pour Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 07/16/2018
ms.openlocfilehash: bcf179484718aa029302281ea91c99588ad2857a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316554"
---
# <a name="performance-tools-for-network-workloads"></a>Outils d’analyse des performances pour les charges de travail de réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les outils de performances.

Cette rubrique contient des sections relatives à l’outil de trafic entre le client et le serveur, la taille de fenêtre TCP/IP et le conseiller Microsoft Server Performance Advisor.

##  <a name="client-to-server-traffic-tool"></a><a name="bkmk_tuning"></a>Outil de trafic entre le client et le serveur

Le trafic client à serveur \(ctsTraffic\) outil vous permet de créer et de vérifier le trafic réseau.

Pour plus d’informations et pour télécharger l’outil, consultez [ctsTraffic (trafic client à serveur)](https://github.com/Microsoft/ctsTraffic).
  
##  <a name="tcpip-window-size"></a><a name="bkmk_size"></a>Taille de la fenêtre TCP/IP

Pour les adaptateurs de 1 Go, les paramètres indiqués dans le tableau précédent doivent fournir un bon débit, car NTttcp définit la taille de fenêtre TCP par défaut sur 64 K via une option de processeur logique spécifique \(SO_RCVBUF\) pour la connexion. Cela offre de bonnes performances sur un réseau à faible latence.  

En revanche, pour les réseaux à latence élevée ou pour les adaptateurs de 10 Go, la valeur de la taille de fenêtre TCP par défaut pour NTttcp génère moins de performances optimales. Dans les deux cas, vous devez ajuster la taille de la fenêtre TCP pour autoriser le produit avec un délai de bande passante supérieur.  

Vous pouvez définir statiquement la taille de la fenêtre TCP sur une valeur élevée à l’aide de l’option **-RB** . Cette option désactive le réglage automatique de la fenêtre TCP, et nous vous recommandons de l’utiliser uniquement si l’utilisateur comprend entièrement la modification résultante dans le comportement TCP/IP. Par défaut, la taille de la fenêtre TCP est définie sur une valeur suffisante et s’ajuste uniquement sous une charge lourde ou sur des liens à latence élevée.  

##  <a name="microsoft-server-performance-advisor"></a><a name="bkmk_advisor"></a>Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \(SPA\) aide les administrateurs informatiques à collecter des mesures pour identifier, comparer et diagnostiquer les problèmes de performances potentiels dans un déploiement de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. 

SPA génère des rapports et des graphiques de diagnostic complets et fournit des recommandations pour vous aider à analyser rapidement les problèmes et à développer des actions correctives.  
  
 Pour plus d’informations et pour télécharger le conseiller, consultez [Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) dans le centre de développement matériel Windows.

Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances du sous-système réseau](net-sub-performance-top.md).