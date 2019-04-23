---
title: Plusieurs cartes réseau doivent être disponibles
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 678c0161e97b8dd022bbf0037d9add5de0281f77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884600"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Plusieurs cartes réseau doivent être disponibles

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Ce serveur est configuré avec une carte réseau, qui doit être partagée par le système d’exploitation de gestion et de toutes les machines virtuelles qui requièrent l’accès à un réseau physique.*  
  
## <a name="impact"></a>Impact  
  
*Performances de mise en réseau peuvent être dégradées dans le système d’exploitation de gestion.*  
  
## <a name="resolution"></a>Résolution  
  
*Ajouter plus de cartes réseau à cet ordinateur. Pour réserver une carte réseau pour une utilisation exclusive par le système d’exploitation de gestion, ne configurez pas pour une utilisation avec un réseau virtuel externe.*  
  
Pour plus d’informations sur l’ajout d’une carte réseau à l’ordinateur, consultez la documentation de l’ordinateur ou de la carte réseau. Ensuite, pour réserver exclusivement pour le système d’exploitation de gestion, ne connectez-le à un commutateur virtuel.   
  


