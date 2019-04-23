---
title: Éviter les incohérences d’alignement entre les blocs virtuels et de secteurs de disque physique sur les disques durs virtuels dynamiques ou des disques de différenciation
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4973c199a5507d00e15da8f621a09f0c602a29fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833860"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>Éviter les incohérences d’alignement entre les blocs virtuels et de secteurs de disque physique sur les disques durs virtuels dynamiques ou des disques de différenciation

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
*Incohérences d’alignement ont été détectées pour un ou plusieurs disques durs virtuels.*  
  
### <a name="impact"></a>Impact  
*Si les disques durs virtuels sont stockés sur un disque physique qui a une taille de secteur de 4K, l’ordinateur virtuel ou les applications qui utilisent le disque dur virtuel peut rencontrer des problèmes de performances. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez l’Assistant Création d’un disque dur virtuel pour créer un nouveau format de disque dur virtuel ou au format VHDX disque dur virtuel et spécifiez le disque dur virtuel existant en tant que le disque source. Le nouveau disque dur virtuel est créé avec un alignement entre les blocs virtuels et le disque physique.*  
  


