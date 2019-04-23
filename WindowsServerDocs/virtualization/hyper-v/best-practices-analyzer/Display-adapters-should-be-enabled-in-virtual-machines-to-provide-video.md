---
title: Cartes graphiques doivent être activés dans les machines virtuelles pour fournir des fonctionnalités vidéo
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d61461db471a876ddf46c1e5fec6992ffa80373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870690"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Cartes graphiques doivent être activés dans les machines virtuelles pour fournir des fonctionnalités vidéo

>S'applique à : Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*L’appareil de vidéo Microsoft Virtual Machine Bus peut être désactivé dans une machine virtuelle.*  
  
Périphérique vidéo de Microsoft Virtual Machine Bus est une carte vidéo virtuelle optimisée pour les machines virtuelles Hyper-V. Lorsqu’une machine virtuelle n’est pas configurée pour utiliser le périphérique vidéo du Bus d’ordinateur virtuel Microsoft, une carte vidéo héritée est utilisée. Périphérique vidéo de Microsoft Virtual Machine Bus plus performant qu’une carte vidéo héritée.  
  
## <a name="impact"></a>Impact  
  
*Va de réduire les performances vidéo pour les ordinateurs virtuels suivants :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le Gestionnaire de périphériques dans le système d’exploitation invité pour permettre aux appareils de vidéo Microsoft Virtual Machine Bus.*  
  
Les étapes nécessaires pour utiliser le Gestionnaire de périphériques varient selon le système d’exploitation. Pour obtenir des instructions, consultez l’aide dans le système d’exploitation invité.  
  


