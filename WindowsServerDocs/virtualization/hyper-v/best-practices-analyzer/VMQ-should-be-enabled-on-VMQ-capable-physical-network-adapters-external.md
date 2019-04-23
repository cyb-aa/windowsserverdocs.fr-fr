---
title: VMQ doit être activée sur les cartes réseau physiques prenant en charge les ordinateurs virtuels liés à un commutateur virtuel externe
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9a552c15675e6ca7a7310c8c9eaec883653987be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889470"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>VMQ doit être activée sur les cartes réseau physiques prenant en charge les ordinateurs virtuels liés à un commutateur virtuel externe

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Les cartes réseau suivantes sont capables de file d’attente de la machine virtuelle (VMQ), mais la fonctionnalité est désactivée.*  
  
## <a name="impact"></a>**Impact**  
*Windows ne peut pas tirer pleinement parti des déchargements de matériel disponible sur les cartes réseau suivantes :*  
  
\<liste des cartes réseau >  
  
## <a name="resolution"></a>**Résolution**  
*Activer les ordinateurs virtuels à l’aide de l’applet de commande Enable-NetAdapterVmq Windows PowerShell ou à l’aide de l’interface utilisateur de propriétés avancées pour la carte réseau.*  
  


