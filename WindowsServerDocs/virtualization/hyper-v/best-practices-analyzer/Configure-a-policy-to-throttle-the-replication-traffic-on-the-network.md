---
title: Configurer une stratégie pour limiter le trafic de réplication sur le réseau
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5b3afd594f56973007a2f0f4318de8a8c7a98209
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365112"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurer une stratégie pour limiter le trafic de réplication sur le réseau

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Il se peut qu’il n’y ait pas de limite quant à la quantité de bande passante réseau que la réplication est autorisée à consommer.*  
  
## <a name="impact"></a>Impact  
la bande passante @no__t 0Network peut devenir complètement dominée par le trafic de réplication, ce qui affecte les autres activités réseau critiques. Cela a un impact sur les ports suivants : *  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>Résolution :  
*If vous utilisez une autre méthode pour limiter le trafic réseau, vous pouvez ignorer cette option. Dans le cas contraire, utilisez l’éditeur de stratégie de groupe pour configurer une stratégie qui limitera le trafic réseau vers le port approprié du serveur de réplication.*  
  
  


