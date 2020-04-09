---
title: Activer toutes les cartes réseau virtuelles configurées pour un ordinateur virtuel
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 79971ff503afcc1a087aced579e30989233c2b4a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861962"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>Activer toutes les cartes réseau virtuelles configurées pour un ordinateur virtuel

>S’applique à Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
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
  


