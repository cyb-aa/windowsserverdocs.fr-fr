---
title: Les tests de basculement doivent être effectués au moins une fois par mois pour vérifier que le basculement fonctionne correctement et que les charges de travail des machines virtuelles fonctionneront comme prévu après le basculement.
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 57a8aa50-e59e-4a4b-8571-1099d5a8eee4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: f99ccfe065015d1731978ba5e7f31c766c343efc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858932"
---
# <a name="test-failovers-should-be-carried-out-at-least-monthly-to-verify-that-failover-will-succeed-and-that-virtual-machine-workloads-will-operate-as-expected-after-failover"></a>Les tests de basculement doivent être effectués au moins une fois par mois pour vérifier que le basculement fonctionne correctement et que les charges de travail des machines virtuelles fonctionneront comme prévu après le basculement.

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016| 
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Aucun test de basculement n’a été fait dans au moins un mois.*  
  
## <a name="impact"></a>Impact  
*Il n’y a aucune confirmation qu’un basculement planifié ou non planifié échoue ou que les opérations de charge de travail se poursuivent correctement après un basculement. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V pour effectuer un test de basculement.*  
  


