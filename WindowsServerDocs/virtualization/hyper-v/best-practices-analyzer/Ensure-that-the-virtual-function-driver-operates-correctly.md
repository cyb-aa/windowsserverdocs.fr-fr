---
title: Assurez-vous que le pilote de fonction virtuelle fonctionne correctement à un ordinateur virtuel est configuré pour utiliser SR-IOV
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d3d0a5008b55d4823cef9a8dd2a7bce4a6a2a33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852080"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>Assurez-vous que le pilote de fonction virtuelle fonctionne correctement à un ordinateur virtuel est configuré pour utiliser SR-IOV

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Le pilote de fonction virtuelle ne fonctionne pas correctement dans le système d’exploitation invité d’un ou plusieurs ordinateurs virtuels.*  
  
## <a name="impact"></a>Impact  
*Performances de mise en réseau n’est pas optimale sur les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Dans le système d’exploitation invité, procédez comme suit : Vérifiez que les pilotes appropriés sont installés et tous les périphériques réseau sont activés et vérifiez le journal des événements d’avertissements ou erreurs.*  
  


