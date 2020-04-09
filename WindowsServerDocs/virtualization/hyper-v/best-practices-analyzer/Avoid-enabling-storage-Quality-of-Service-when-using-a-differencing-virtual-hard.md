---
title: Évitez d’activer la qualité de service de stockage lors de l’utilisation d’un disque dur virtuel de différenciation lorsque les disques durs virtuels parents et enfants se trouvent sur des volumes différents
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 4190b164b473e54c7fcecd68ddf8f746cebcfdc9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857782"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Évitez d’activer la qualité de service de stockage lors de l’utilisation d’un disque dur virtuel de différenciation lorsque les disques durs virtuels parents et enfants se trouvent sur des volumes différents

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*La qualité de service de stockage est activée sur un disque dur virtuel de différenciation avec les disques durs virtuels parents et enfants sur des volumes différents.*  
  
## <a name="impact"></a>**Effet**  
*Cette configuration peut entraîner un comportement de qualité de service de stockage inattendu pour le disque dur virtuel de différenciation, ainsi que d’autres disques durs virtuels sur les volumes parents et enfants. Cela a un impact sur les disques durs virtuels suivants :*  
  
\<liste des disques durs virtuels >  
  
## <a name="resolution"></a>**Résolution**  
*Désactivez la qualité de service de stockage sur les disques durs virtuels référencés ou effectuez une migration du stockage pour déplacer le disque dur virtuel parent et le disque dur virtuel enfant vers le même volume.*  
  


