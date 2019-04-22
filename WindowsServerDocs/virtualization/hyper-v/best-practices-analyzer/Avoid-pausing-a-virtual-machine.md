---
title: Éviter l’interruption d’une machine virtuelle
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4492ac385a289d075ebcd48b1c7c1c78c1af2f8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814350"
---
# <a name="avoid-pausing-a-virtual-machine"></a>Éviter l’interruption d’une machine virtuelle

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
  
*Ce serveur a une ou plusieurs machines virtuelles dans un état suspendu.*  
  
## <a name="impact"></a>Impact  
  
*Selon la quantité de mémoire disponible, vous ne serez peut-être pas en mesure d’exécuter des machines virtuelles supplémentaires.*  
  
Ordinateurs virtuels suspendus ne libérer la mémoire allouée, ce qui signifie que la mémoire n’est pas disponible pour démarrer d’autres machines virtuelles.  
  
## <a name="resolution"></a>Résolution  
  
*Si cela est intentionnel, aucune action supplémentaire n’est requise. Sinon, envisagez de reprendre ces machines virtuelles ou de les arrêter.*  
  
#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>Utilisez le Gestionnaire Hyper-V pour reprendre la machine virtuelle  
  
1.  Ouvrez le Gestionnaire Hyper-V. (À partir de la **outils** menu du Gestionnaire de serveur, cliquez sur **Gestionnaire Hyper-V**.)  
  
2.  À partir de la **Machines virtuelles** liste, recherchez les ordinateurs virtuels dont l’état de **suspendu**.  
  
    > [!IMPORTANT]  
    > Un état de **stratégiques en pause** se produit lorsqu’il existe très peu d’espace libre restant sur le stockage physique de cet ordinateur virtuel. Avant de tenter de relancer un ordinateur virtuel dans cet état, libérez de l’espace disponible sur le stockage physique.  
  
3.  Le bouton droit sur chaque machine virtuelle, puis cliquez sur **Resume**. Cela renvoie la machine virtuelle à un état en cours d’exécution. Après cela, si vous souhaitez arrêter la machine virtuelle, faites un clic droit à nouveau et choisissez **arrêter**.  
  
#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>Utiliser Windows PowerShell pour reprendre la machine virtuelle  
  
Faire cela en une seule commande en utilisant le filtrage et le pipeline après avoir obtenir tous les ordinateurs virtuels sur l’hôte. Tapez :  
  
```  
get-vm | where state -eq 'paused' | resume-vm  
```  
  


