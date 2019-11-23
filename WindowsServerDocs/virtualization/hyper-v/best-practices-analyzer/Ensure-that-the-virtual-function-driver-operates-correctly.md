---
title: S’assurer que le pilote de fonction virtuelle fonctionne correctement lorsqu’un ordinateur virtuel est configuré pour utiliser SR-IOV
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5e2c666973aa1ac0d5eb2c4e0d5d29793dc0ce75
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393618"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>S’assurer que le pilote de fonction virtuelle fonctionne correctement lorsqu’un ordinateur virtuel est configuré pour utiliser SR-IOV

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Le pilote de fonction virtuelle ne fonctionne pas correctement dans le système d’exploitation invité d’un ou de plusieurs ordinateurs virtuels.*  
  
## <a name="impact"></a>Impact  
*Les performances de mise en réseau ne sont pas optimales sur les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Dans le système d’exploitation invité, procédez comme suit : Vérifiez que les pilotes appropriés sont installés et que tous les périphériques réseau sont activés, et recherchez des erreurs ou des avertissements dans le journal des événements.*  
  


