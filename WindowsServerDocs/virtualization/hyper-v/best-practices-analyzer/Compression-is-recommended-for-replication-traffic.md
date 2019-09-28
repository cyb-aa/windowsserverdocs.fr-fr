---
title: La compression est recommandée pour le trafic de réplication
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 77a314e816c36f626ea3edb10b80f65e3897e7c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365099"
---
# <a name="compression-is-recommended-for-replication-traffic"></a>La compression est recommandée pour le trafic de réplication

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
*Le trafic de réplication envoyé sur le réseau à partir du serveur principal vers le serveur de réplication n’est pas compressé.*  
  
## <a name="impact"></a>Impact  
le trafic *Replication utilise plus de bande passante qu’il n’en faut. Cela a un impact sur les ordinateurs virtuels suivants :*  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>Résolution :  
Réplica Hyper-V @no__t 0Configure pour compresser les données transmises sur le réseau dans les paramètres de la machine virtuelle dans le Gestionnaire Hyper-V. Vous pouvez également utiliser des outils en dehors d’Hyper-V pour effectuer la compression. *  
  


