---
title: Les machines virtuelles doivent être sauvegardées au moins une fois par semaine
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ec11b067de2c9f8cbb3a17731caa0dc526bf54a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393236"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>Les machines virtuelles doivent être sauvegardées au moins une fois par semaine

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
*Une ou plusieurs machines virtuelles n’ont pas été sauvegardées au cours de la semaine dernière.*  
  
## <a name="impact"></a>Impact  
la perte de données de @no__t 0Significant peut se produire si la machine virtuelle rencontre un problème et qu’une sauvegarde récente n’existe pas. Cela a un impact sur les ordinateurs virtuels suivants : *  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>Résolution :  
*Schedule une sauvegarde des machines virtuelles à exécuter au moins une fois par semaine. Vous pouvez ignorer cette règle si cet ordinateur virtuel est un réplica et que son ordinateur virtuel principal est en cours de sauvegarde, ou s’il s’agit de l’ordinateur virtuel principal et que son réplica est en cours de sauvegarde.*  
  


