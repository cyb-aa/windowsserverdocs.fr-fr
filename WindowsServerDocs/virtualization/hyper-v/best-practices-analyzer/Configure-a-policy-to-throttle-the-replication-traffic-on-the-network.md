---
title: Configurer une stratégie pour limiter le trafic de réplication sur le réseau
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2c358cd930f2b95412b40aa6c87b0bf9ebb5b741
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862172"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurer une stratégie pour limiter le trafic de réplication sur le réseau

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Il se peut qu’il n’y ait pas de limite quant à la quantité de bande passante réseau que la réplication est autorisée à consommer.*  
  
## <a name="impact"></a>Impact  
*La bande passante réseau peut devenir complètement dominée par le trafic de réplication, ce qui affecte les autres activités réseau critiques. Cela a un impact sur les ports suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Si vous utilisez une autre méthode pour limiter le trafic réseau, vous pouvez ignorer cette option. Dans le cas contraire, utilisez l’éditeur de stratégie de groupe pour configurer une stratégie qui limitera le trafic réseau sur le port approprié du serveur de réplication.*  
  
  


