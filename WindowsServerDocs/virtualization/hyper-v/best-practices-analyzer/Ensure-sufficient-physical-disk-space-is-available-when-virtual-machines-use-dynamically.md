---
title: Vérifiez que suffisamment d’espace disque physique est disponible lorsque les machines virtuelles utilisent des disques durs virtuels de taille dynamique
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 09e481b99594ac543dadab2b60bf9b3f4c29e54b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883290"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Vérifiez que suffisamment d’espace disque physique est disponible lorsque les machines virtuelles utilisent des disques durs virtuels de taille dynamique

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
*Un ou plusieurs ordinateurs virtuels utilisent des disques durs virtuels de taille dynamique.*  
  
## <a name="impact"></a>Impact  
*Taille dynamique des disques durs virtuels nécessitent l’espace disponible sur le volume hôte afin que l’espace peut être alloué lorsque des écritures sur les disques durs virtuels se produisent. Si l’espace disponible est épuisé, toute machine virtuelle qui s’appuie sur le stockage physique pourrait être affectée. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Moniteur d’espace disque disponible pour garantir un espace suffisant est disponible pour l’extension. Vous devez arrêter la machine virtuelle et utiliser l’Assistant Modification de disque dans le Gestionnaire Hyper-V pour convertir chaque disque dur virtuel dynamique pour cette machine virtuelle sur un disque dur virtuel taille fixe.*  
  


