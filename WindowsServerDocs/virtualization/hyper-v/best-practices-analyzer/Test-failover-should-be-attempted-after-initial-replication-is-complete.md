---
title: Test de basculement doit être tentée après que la réplication initiale est terminée
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 347b089330548e5fb2631e43ca66c96bb6bc4e9d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880410"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>Test de basculement doit être tentée après que la réplication initiale est terminée

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="problem"></a>Problème  
*Aucun test de basculement n’a été au moins un mois plus tard.*  
  
## <a name="impact"></a>Impact  
*Il n’existe aucune confirmation qui réussit un basculement non planifié ou non planifié ou d’opérations de la charge de travail continue correctement après un basculement. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V pour effectuer un test de basculement.*  
  


