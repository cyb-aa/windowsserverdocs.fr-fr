---
title: Configurer une stratégie pour limiter le trafic de réplication sur le réseau
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2c1f1865fa1d611c0b5baaf981140f9807b51458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818690"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurer une stratégie pour limiter le trafic de réplication sur le réseau

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
*Il existe peut-être pas une limite sur la quantité de bande passante réseau que la réplication est autorisée à consommer.*  
  
## <a name="impact"></a>Impact  
*La bande passante réseau peut devenir complètement dominée par le trafic de réplication, qui affectent d’autres activités réseau critiques. Cela affecte les ports suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Si vous utilisez une autre méthode pour limiter le trafic réseau, vous pouvez l’ignorer. Sinon, utilisez l’éditeur de stratégie de groupe pour configurer une stratégie qui limite le trafic réseau au port approprié du serveur réplica.*  
  
  


