---
title: Un réseau SAN virtuel doit être associé à un adaptateur de bus hôte physique
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3b9ca1e2da1cf9f4410f465fe95c6cc9c0b07ffc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819080"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Un réseau SAN virtuel doit être associé à un adaptateur de bus hôte physique

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
*Un réseau de zone de stockage virtuel (SAN) a été configuré sans une association à une carte de bus hôte (HBA).*  
  
## <a name="impact"></a>**Impact**  
*Une machine virtuelle démarrage échoue lorsqu’il est configuré avec une carte Fibre Channel virtuelle connectée à un réseau SAN virtuel mal configuré. Cela affecte ce SAN virtuel qui suit :*  
  
  
\<liste des réseaux SAN virtuels >  
  
  
## <a name="resolution"></a>**Résolution**  
*Reconfigurer le réseau SAN virtuel en vous connectant à une carte de bus hôte.*  
  
  
  


