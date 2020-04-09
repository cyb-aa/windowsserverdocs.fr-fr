---
title: Les ordinateurs virtuels doivent être activés sur les cartes réseau physiques compatibles avec les ordinateurs virtuels et liées à un commutateur virtuel externe
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a66c1c4580f8ecda90caa5446e74bc9b12ac0476
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855042"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>Les ordinateurs virtuels doivent être activés sur les cartes réseau physiques compatibles avec les ordinateurs virtuels et liées à un commutateur virtuel externe

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Les cartes réseau suivantes peuvent être associées à la file d’attente d’ordinateurs virtuels, mais la fonctionnalité est désactivée.*  
  
## <a name="impact"></a>**Effet**  
*Windows ne peut pas tirer pleinement parti des déchargements de matériel disponibles sur les cartes réseau suivantes :*  
  
\<liste des cartes réseau >  
  
## <a name="resolution"></a>**Résolution**  
*Activez les ordinateurs virtuels à l’aide de l’applet de commande Windows PowerShell Enable-NetAdapterVmq ou en utilisant l’interface utilisateur des propriétés avancées pour la carte réseau.*  
  


