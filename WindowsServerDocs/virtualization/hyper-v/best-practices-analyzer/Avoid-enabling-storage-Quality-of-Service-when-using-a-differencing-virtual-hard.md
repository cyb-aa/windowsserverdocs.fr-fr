---
title: Évitez d’activer la qualité de service de stockage lors de l’utilisation d’un disque dur virtuel de différenciation lorsque les disques durs virtuels parents et enfants se trouvent sur des volumes différents
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 716a32de2f9327e5eca38c470fa1b7c44150e9cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366439"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Évitez d’activer la qualité de service de stockage lors de l’utilisation d’un disque dur virtuel de différenciation lorsque les disques durs virtuels parents et enfants se trouvent sur des volumes différents

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*La qualité de service de stockage est activée sur un disque dur virtuel de différenciation avec les disques durs virtuels parents et enfants sur des volumes différents.*  
  
## <a name="impact"></a>**Impact**  
*Cette configuration peut entraîner un comportement de qualité de service de stockage inattendu pour le disque dur virtuel de différenciation, ainsi que d’autres disques durs virtuels sur les volumes parents et enfants. Cela a un impact sur les disques durs virtuels suivants :*  
  
\<liste des disques durs virtuels >  
  
## <a name="resolution"></a>**Résolution**  
*Désactivez la qualité de service de stockage sur les disques durs virtuels référencés ou effectuez une migration du stockage pour déplacer le disque dur virtuel parent et le disque dur virtuel enfant vers le même volume.*  
  


