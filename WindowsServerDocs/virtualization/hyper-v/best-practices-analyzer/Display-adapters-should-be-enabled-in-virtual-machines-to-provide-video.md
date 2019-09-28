---
title: Les adaptateurs d’affichage doivent être activés sur les machines virtuelles pour fournir des fonctionnalités vidéo
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0c515c7fb1ed160dfee1e1b7303022082e936157
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364907"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Les adaptateurs d’affichage doivent être activés sur les machines virtuelles pour fournir des fonctionnalités vidéo

>S'applique à : Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Le périphérique vidéo Microsoft Virtual Machine bus peut être désactivé sur une machine virtuelle.*  
  
Microsoft Virtual Machine bus Video Device est une carte vidéo virtuelle optimisée pour une utilisation avec des machines virtuelles Hyper-V. Lorsqu’un ordinateur virtuel n’est pas configuré pour utiliser le périphérique vidéo du bus de machines virtuelles Microsoft, une carte vidéo héritée est utilisée. Microsoft Virtual Machine bus Video Device fonctionne mieux qu’une carte vidéo héritée.  
  
## <a name="impact"></a>Impact  
  
*Les performances vidéo pour les machines virtuelles suivantes seront dégradées :*  
  
@no__t 0list de noms de machines virtuelles >  
  
## <a name="resolution"></a>Résolution :  
  
*Utilisez Device Manager dans le système d’exploitation invité pour activer le périphérique vidéo bus de machine virtuelle Microsoft.*  
  
Les étapes nécessaires à l’utilisation de Device Manager varient selon le système d’exploitation. Pour obtenir des instructions, consultez l’aide dans le système d’exploitation invité.  
  


