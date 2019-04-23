---
title: Évitez d’activer la qualité de Service de stockage lors de l’utilisation d’un disque dur virtuel de différenciation lorsque les disques durs virtuels parents et enfants sont sur des volumes différents
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2bdc8462c4d9dc50dbb69792f2f294add0ca3a74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856200"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Évitez d’activer la qualité de Service de stockage lors de l’utilisation d’un disque dur virtuel de différenciation lorsque les disques durs virtuels parents et enfants sont sur des volumes différents

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Un disque dur virtuel de différenciation avec les disques durs virtuels parents et enfants sur des volumes différents dispose d’un stockage de qualité de Service est activé.*  
  
## <a name="impact"></a>**Impact**  
*Cette configuration peut entraîner de stockage inattendu comportement de qualité de Service pour le disque dur virtuel de différenciation, ainsi que d’autres disques durs virtuels sur les volumes parents et enfants. Cela affecte ce qui suit les disques durs virtuels :*  
  
\<liste des disques durs virtuels >  
  
## <a name="resolution"></a>**Résolution**  
*Désactiver la qualité de Service de stockage sur les disques durs virtuels référencés, ou effectuer une migration de stockage pour déplacer le parent et le disque dur virtuel d’enfant dans le même volume.*  
  


