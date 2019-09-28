---
title: Pour participer à la réplication, un service Broker de réplication Hyper-V doit être configuré sur les serveurs des clusters de basculement.
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2e15d2c4a467807397ef4712d2df1730b40d8024
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364596"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Pour participer à la réplication, un service Broker de réplication Hyper-V doit être configuré sur les serveurs des clusters de basculement.

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Pour les clusters de basculement, le réplica Hyper-V requiert l’utilisation d’un nom de service Broker de réplication Hyper-V au lieu d’un nom de serveur individuel.*  
  
## <a name="impact"></a>Impact  
*Si la machine virtuelle est déplacée vers un autre nœud de cluster de basculement, la réplication ne peut pas continuer.*  
  
## <a name="resolution"></a>Résolution :  
*Use Gestionnaire du cluster de basculement pour configurer le Service Broker de réplication Hyper-V. Dans le Gestionnaire Hyper-V, assurez-vous que la configuration de la réplication utilise le nom du Service Broker de réplication Hyper-V comme nom de serveur.*  
  


