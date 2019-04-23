---
title: Pour participer à la réplication, les serveurs dans les clusters de basculement doivent avoir un service Broker de réplication Hyper-V configuré
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d4966396af955f9c8bad34b5b2892115e93c3b85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887970"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Pour participer à la réplication, les serveurs dans les clusters de basculement doivent avoir un service Broker de réplication Hyper-V configuré

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Pour les clusters de basculement, Hyper-V Replica requiert l’utilisation d’un nom de service Broker de réplication Hyper-V au lieu d’un nom de serveur individuels.*  
  
## <a name="impact"></a>Impact  
*Si la machine virtuelle est déplacée vers un autre nœud de cluster, la réplication ne peut pas continuer.*  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire de Cluster de basculement pour configurer le service Broker de réplication Hyper-V. Dans le Gestionnaire Hyper-V, assurez-vous que la configuration de réplication utilise le nom de service Broker de réplication Hyper-V en tant que le nom du serveur.*  
  


