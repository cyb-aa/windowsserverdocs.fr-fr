---
title: Plus d’une carte réseau doit être disponible
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6b043900c6fde4522e5805a1f0c1a635de335e31
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364796"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Plus d’une carte réseau doit être disponible

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Ce serveur est configuré avec une carte réseau, qui doit être partagée par le système d’exploitation de gestion et tous les ordinateurs virtuels qui requièrent l’accès à un réseau physique.*  
  
## <a name="impact"></a>Impact  
  
*Les performances de mise en réseau peuvent être détériorées dans le système d’exploitation de gestion.*  
  
## <a name="resolution"></a>Résolution :  
  
*Add plus de cartes réseau sur cet ordinateur. Pour réserver une carte réseau pour une utilisation exclusive par le système d’exploitation de gestion, ne la configurez pas pour une utilisation avec un réseau virtuel externe.*  
  
Pour plus d’informations sur l’ajout d’une carte réseau à l’ordinateur, consultez la documentation de l’ordinateur ou de la carte réseau. Ensuite, pour le réserver exclusivement au système d’exploitation de gestion, ne le connectez pas à un commutateur virtuel.   
  


