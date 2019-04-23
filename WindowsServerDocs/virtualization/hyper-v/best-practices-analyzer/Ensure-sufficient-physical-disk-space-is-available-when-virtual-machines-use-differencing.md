---
title: Vérifiez que suffisamment d’espace disque physique est disponible lorsque les machines virtuelles utilisent des disques durs virtuels de différenciation
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6d4d219402cdc321a75cc27d75ea7749eb6127e0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855570"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Vérifiez que suffisamment d’espace disque physique est disponible lorsque les machines virtuelles utilisent des disques durs virtuels de différenciation

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
*Un ou plusieurs ordinateurs virtuels utilisent des disques durs virtuels de différenciation.*  
  
## <a name="impact"></a>Impact  
*Disques durs virtuels de différenciation nécessitent la quantité d’espace disponible sur le volume hôte afin que l’espace peut être alloué lorsque des écritures sur les disques durs virtuels se produisent. Si l’espace disponible est épuisé, toute machine virtuelle qui s’appuie sur le stockage physique pourrait être affectée. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Surveille l’espace disque disponible pour garantir un espace suffisant est disponible pour l’expansion du disque dur virtuel. Si possible, fusionnez les disques durs virtuels de différenciation dans leur parent. Dans le Gestionnaire Hyper-V, inspecter le disque de différenciation pour déterminer le disque dur virtuel parent. Si vous fusionnez un disque de différenciation à un disque parent qui est partagé par d’autres disques de différenciation, cette action endommagera la relation entre les disques de différenciation et le disque parent, ce qui les rend inutilisable. Après avoir vérifié que le disque dur virtuel parent n’est pas partagé, vous pouvez utiliser l’Assistant Modification de disque à fusionner le disque de différenciation au disque dur virtuel parent.*  
  


