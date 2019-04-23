---
title: Les disques durs virtuels dynamiques au format VHD ne sont pas recommandés pour les machines virtuelles qui exécutent des charges de travail de serveur dans un environnement de production
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 324a60a0-1d15-4ef2-9f17-23cbd2eb42ce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6acd27fa0efa27ba74c28e290c789edca599f66f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849880"
---
# <a name="vhd-format-dynamic-virtual-hard-disks-are-not-recommended-for-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Les disques durs virtuels dynamiques au format VHD ne sont pas recommandés pour les machines virtuelles qui exécutent des charges de travail de serveur dans un environnement de production

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
*Un ou plusieurs machines virtuelles utilisent des disques durs virtuels de taille dynamique au format VHD.*  
  
## <a name="impact"></a>**Impact**  
*Les disques durs virtuels dynamiques au format VHD peut rencontrer des problèmes de cohérence en cas de panne d’électricité. Problèmes de cohérence peuvent se produire si le disque physique effectue une mise à jour incomplet ou incorrect à un secteur dans un fichier .vhd qui est en cours de modification lors de la panne d’électricité. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Arrêter la machine virtuelle et convertissez au format VHD dynamique disque dur virtuel sur un disque dur virtuel d’au format VHDX ou sur un disque dur virtuel fixe. (Le format VHDX dispose de mécanismes de fiabilité qui permettent de protéger le disque à un endommagement en raison de pannes d’alimentation système.) Toutefois, ne convertissez pas le disque dur virtuel si elle est susceptible d’être attaché à une version antérieure de Windows à un moment donné. Windows les versions antérieures à Windows Server 2012 ne prennent pas en charge le format VHDX.*  
  


