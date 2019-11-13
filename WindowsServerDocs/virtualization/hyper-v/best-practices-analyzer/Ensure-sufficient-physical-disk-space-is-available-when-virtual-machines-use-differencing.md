---
title: Assurez-vous que l’espace disque physique est suffisant lorsque les machines virtuelles utilisent des disques durs virtuels de différenciation
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3827d149d2d691b4ecd7fe6ae8f6d7255c85a31c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364869"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Assurez-vous que l’espace disque physique est suffisant lorsque les machines virtuelles utilisent des disques durs virtuels de différenciation

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Une ou plusieurs machines virtuelles utilisent des disques durs virtuels de différenciation.*  
  
## <a name="impact"></a>Impact  
*Les disques durs virtuels de différenciation requièrent de l’espace disponible sur le volume hôte afin que de l’espace puisse être alloué lors de l’écriture sur les disques durs virtuels. Si l’espace disponible est épuisé, toute machine virtuelle qui s’appuie sur le stockage physique peut être affectée. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Surveillez l’espace disque disponible pour vous assurer que l’espace disponible est suffisant pour l’extension du disque dur virtuel. Envisagez de fusionner les disques durs virtuels de différenciation dans leur parent. Dans le Gestionnaire Hyper-V, inspectez le disque de différenciation pour déterminer le disque dur virtuel parent. Si vous fusionnez un disque de différenciation sur un disque parent qui est partagé par d’autres disques de différenciation, cette action endommage la relation entre les autres disques de différenciation et le disque parent, ce qui les rend inutilisables. Après avoir vérifié que le disque dur virtuel parent n’est pas partagé, vous pouvez utiliser l’Assistant modification de disque pour fusionner le disque de différenciation avec le disque dur virtuel parent.*  
  


