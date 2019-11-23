---
title: Un réseau SAN virtuel doit être associé à un adaptateur de bus hôte physique
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9e86f8d9b9a4a87fd6457954c3a4723857faac3b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366689"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Un réseau SAN virtuel doit être associé à un adaptateur de bus hôte physique

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Un réseau de zone de stockage (SAN) virtuel a été configuré sans association à une carte de bus hôte (HBA).*  
  
## <a name="impact"></a>**Impact**  
*Un ordinateur virtuel ne peut pas démarrer lorsqu’il est configuré avec une carte Fibre Channel virtuelle connectée à un réseau SAN virtuel mal configuré. Cela a un impact sur les réseaux SAN virtuels suivants :*  
  
  
\<la liste des réseaux SAN virtuels >  
  
  
## <a name="resolution"></a>**Résolution**  
*Reconfigurez le réseau SAN virtuel en le connectant à un adaptateur de bus hôte.*  
  
  
  


