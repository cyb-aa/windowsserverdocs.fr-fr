---
title: Machines virtuelles doivent être sauvegardées au moins une fois par semaine
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e079e3cb225ec9c712233bbf3efc85bb6f09b218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826750"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>Machines virtuelles doivent être sauvegardées au moins une fois par semaine

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
*Un ou plusieurs ordinateurs virtuels n’ont pas été sauvegardés dans la semaine dernière.*  
  
## <a name="impact"></a>Impact  
*Perte importante de données peut se produire si la machine virtuelle rencontre un problème et une sauvegarde récente n’existe pas. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Planifier une sauvegarde des machines virtuelles au moins une fois par semaine. Vous pouvez ignorer cette règle si cette machine virtuelle est un réplica et son ordinateur virtuel principal est en cours de sauvegarde, ou si l’ordinateur virtuel principal et son réplica est en cours de sauvegarde.*  
  


