---
title: La réplication est suspendue pour un ou plusieurs ordinateurs virtuels sur ce serveur
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 17d50f116c6cee488367c924bfbce3791a8d879f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393543"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>La réplication est suspendue pour un ou plusieurs ordinateurs virtuels sur ce serveur

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*La réplication est suspendue pour un ou plusieurs ordinateurs virtuels. Lorsque la machine virtuelle principale est suspendue, toutes les modifications qui se produisent sont accumulées et envoyées à la machine virtuelle de réplication une fois la réplication reprise.*  
  
## <a name="impact"></a>Impact  
*Tant que la réplication est suspendue, les modifications accumulées qui se produisent sur l’ordinateur virtuel principal consomment de l’espace disque disponible sur le serveur principal. Une fois la réplication reprise, il peut y avoir un grand nombre de trafic réseau vers le serveur de réplication. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Confirmez que la suspension de la réplication était prévue. Si la réplication a été suspendue pour gérer l’espace disque insuffisant ou la connectivité réseau, reprenez la réplication dès que ces problèmes sont résolus.*  
  


