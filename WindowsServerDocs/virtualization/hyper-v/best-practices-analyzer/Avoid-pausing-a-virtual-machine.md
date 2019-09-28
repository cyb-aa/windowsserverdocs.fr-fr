---
title: Éviter la suspension d’un ordinateur virtuel
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 406b24edd4a7e87e32058006590ac7cd37206568
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366452"
---
# <a name="avoid-pausing-a-virtual-machine"></a>Éviter la suspension d’un ordinateur virtuel

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Ce serveur a une ou plusieurs machines virtuelles dans un état suspendu.*  
  
## <a name="impact"></a>Impact  
  
*Selon la quantité de mémoire disponible, vous ne pourrez peut-être pas exécuter d’autres machines virtuelles.*  
  
Les machines virtuelles suspendues ne libèrent pas leur mémoire allouée, ce qui signifie que la mémoire n’est pas disponible pour démarrer d’autres machines virtuelles.  
  
## <a name="resolution"></a>Résolution :  
  
*If cette opération est intentionnelle, aucune autre action n’est requise. Sinon, envisagez de reprendre ces machines virtuelles ou de les arrêter.*  
  
#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>Utiliser le Gestionnaire Hyper-V pour reprendre la machine virtuelle  
  
1.  Ouvrez le Gestionnaire Hyper-V. (Dans le menu **Outils** de gestionnaire de serveur, cliquez sur **Gestionnaire Hyper-V**.)  
  
2.  Dans la liste **machines virtuelles** , recherchez les machines virtuelles dont l’État est **suspendu**.  
  
    > [!IMPORTANT]  
    > Un état de **pause critique** se produit lorsque l’espace libre restant sur le stockage physique de cet ordinateur virtuel est très faible. Avant de tenter de reprendre un ordinateur virtuel dans cet État, libérez de l’espace disponible sur le stockage physique.  
  
3.  Cliquez avec le bouton droit sur chaque nom de machine virtuelle, puis cliquez sur **reprendre**. Cela retourne l’ordinateur virtuel à un État en cours d’exécution. Après cela, si vous souhaitez arrêter l’ordinateur virtuel, cliquez dessus à nouveau avec le bouton droit, puis choisissez **arrêter**.  
  
#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>Utiliser Windows PowerShell pour reprendre la machine virtuelle  
  
Vous pouvez le faire dans une commande en utilisant le filtrage et le pipeline après avoir obtenu tous les ordinateurs virtuels sur l’ordinateur hôte. Tapez :  
  
```  
get-vm | where state -eq 'paused' | resume-vm  
```  
  


