---
title: Plus d’une carte réseau doit être disponible
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 56cb747ac44d48b115dbf105ea96e4623d458b28
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861892"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Plus d’une carte réseau doit être disponible

>S’applique à Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Ce serveur est configuré avec une carte réseau, qui doit être partagée par le système d’exploitation de gestion et tous les ordinateurs virtuels qui requièrent l’accès à un réseau physique.*  
  
## <a name="impact"></a>Impact  
  
*Les performances de mise en réseau peuvent être détériorées dans le système d’exploitation de gestion.*  
  
## <a name="resolution"></a>Résolution  
  
*Ajoutez des cartes réseau supplémentaires à cet ordinateur. Pour réserver une carte réseau pour une utilisation exclusive par le système d’exploitation de gestion, ne la configurez pas pour une utilisation avec un réseau virtuel externe.*  
  
Pour plus d’informations sur l’ajout d’une carte réseau à l’ordinateur, consultez la documentation de l’ordinateur ou de la carte réseau. Ensuite, pour le réserver exclusivement au système d’exploitation de gestion, ne le connectez pas à un commutateur virtuel.   
  


