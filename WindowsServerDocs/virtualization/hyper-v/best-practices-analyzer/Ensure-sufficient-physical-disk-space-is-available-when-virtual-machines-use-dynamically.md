---
title: Assurez-vous que l’espace disque physique est suffisant lorsque les machines virtuelles utilisent des disques durs virtuels de taille dynamique.
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c38d7c11a05eef9d29097e625fec2830000cf550
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393641"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Assurez-vous que l’espace disque physique est suffisant lorsque les machines virtuelles utilisent des disques durs virtuels de taille dynamique.

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Une ou plusieurs machines virtuelles utilisent des disques durs virtuels de taille dynamique.*  
  
## <a name="impact"></a>Impact  
*Les disques durs virtuels de taille dynamique requièrent de l’espace disponible sur le volume hôte afin que de l’espace puisse être alloué lors de l’écriture sur les disques durs virtuels. Si l’espace disponible est épuisé, toute machine virtuelle qui s’appuie sur le stockage physique peut être affectée. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Surveillez l’espace disque disponible pour vous assurer que l’espace disponible est suffisant pour l’extension. Envisagez d’arrêter la machine virtuelle et d’utiliser l’Assistant modification du disque dans le Gestionnaire Hyper-V pour convertir chaque disque dur virtuel de taille dynamique de cet ordinateur virtuel en disque dur virtuel de taille fixe.*  
  


