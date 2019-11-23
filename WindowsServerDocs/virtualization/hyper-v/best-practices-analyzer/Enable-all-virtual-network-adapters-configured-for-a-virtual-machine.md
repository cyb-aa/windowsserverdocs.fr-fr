---
title: Activer toutes les cartes réseau virtuelles configurées pour un ordinateur virtuel
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bdca25be4af41d0f6ddfafe885f8c2b1301b71fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393650"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>Activer toutes les cartes réseau virtuelles configurées pour un ordinateur virtuel

>S’applique à Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Une ou plusieurs cartes réseau peuvent être désactivées sur un ordinateur virtuel.*  
  
## <a name="impact"></a>Impact  
  
*Les machines virtuelles suivantes peuvent ne pas disposer d’une connectivité réseau :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez Device Manager dans le système d’exploitation invité pour activer toutes les cartes réseau virtuelles. Si l’adaptateur n’est pas requis, utilisez le Gestionnaire Hyper-V pour le supprimer de la machine virtuelle.*  
  


