---
title: Captures instantanées de récupération doivent être supprimées après le basculement
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4663320df91019fc7dc1d8ca7ffdb2fcc3e0de42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837680"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Captures instantanées de récupération doivent être supprimées après le basculement

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016| 
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Une machine virtuelle basculée a un ou plusieurs instantanés de récupération.*  
  
## <a name="impact"></a>**Impact**  
*Espace disponible peut manquer sur le disque physique qui stocke les fichiers d’instantanés. Si cela se produit, aucune opération de disque supplémentaire peut être effectuée sur le stockage physique. Toute machine virtuelle qui s’appuie sur le stockage physique pourrait être affectée. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Pour chaque machine virtuelle basculée, utilisez l’applet de commande Complete-VMFailover dans Windows PowerShell pour supprimer les captures instantanées de récupération et indiquer l’achèvement de basculement.*  
  


