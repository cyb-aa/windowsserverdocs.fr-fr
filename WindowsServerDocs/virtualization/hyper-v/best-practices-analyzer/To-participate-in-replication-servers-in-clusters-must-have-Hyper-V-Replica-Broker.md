---
title: Pour participer à la réplication, un service Broker de réplication Hyper-V doit être configuré sur les serveurs des clusters de basculement.
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 921d31aa63bcaaf0946c487d327144f5e29bcfe0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854582"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Pour participer à la réplication, un service Broker de réplication Hyper-V doit être configuré sur les serveurs des clusters de basculement.

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Pour les clusters de basculement, le réplica Hyper-V requiert l’utilisation d’un nom de service Broker de réplication Hyper-V au lieu d’un nom de serveur individuel.*  
  
## <a name="impact"></a>Impact  
*Si la machine virtuelle est déplacée vers un autre nœud de cluster de basculement, la réplication ne peut pas continuer.*  
  
## <a name="resolution"></a>Résolution  
*Utilisez Gestionnaire du cluster de basculement pour configurer le Service Broker de réplication Hyper-V. Dans le Gestionnaire Hyper-V, assurez-vous que la configuration de la réplication utilise le nom du Service Broker de réplication Hyper-V comme nom de serveur.*  
  


