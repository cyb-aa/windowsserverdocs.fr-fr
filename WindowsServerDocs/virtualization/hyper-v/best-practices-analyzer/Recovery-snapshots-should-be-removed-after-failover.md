---
title: Les instantanés de récupération doivent être supprimés après le basculement
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4b8574956fb1b46ca0cf9678187fffcd68c2d261
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393528"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Les instantanés de récupération doivent être supprimés après le basculement

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016| 
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Une machine virtuelle basculée possède un ou plusieurs instantanés de récupération.*  
  
## <a name="impact"></a>**Impact**  
l’espace @no__t 0Available peut s’exécuter sur le disque physique qui stocke les fichiers d’instantanés. Si cela se produit, aucune opération de disque supplémentaire ne peut être effectuée sur le stockage physique. Tout ordinateur virtuel qui s’appuie sur le stockage physique peut être affecté. Cela a un impact sur les ordinateurs virtuels suivants : *  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Pour chaque machine virtuelle basculée, utilisez l’applet de commande Complete-VMFailover dans Windows PowerShell pour supprimer les instantanés de récupération et indiquer la fin du basculement.*  
  


