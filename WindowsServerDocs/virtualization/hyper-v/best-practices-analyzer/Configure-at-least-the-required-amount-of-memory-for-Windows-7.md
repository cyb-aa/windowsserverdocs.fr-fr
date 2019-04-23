---
title: Configurer au moins la quantité de mémoire pour une machine virtuelle exécutant Windows 7 et activé pour la mémoire dynamique requise
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 119965bf-6154-414d-b3a1-aa5b30eac5f6
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 414746d5b039be27420a8507384cc1a759906d46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887810"
---
# <a name="configure-at-least-the-required-amount-of-memory-for-a-virtual-machine-running-windows-7-and-enabled-for-dynamic-memory"></a>Configurer au moins la quantité de mémoire pour une machine virtuelle exécutant Windows 7 et activé pour la mémoire dynamique requise

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Un ou plusieurs ordinateurs virtuels sont configurés pour utiliser la mémoire dynamique avec inférieure à la quantité de mémoire requise pour Windows 7.*  
  
## <a name="impact"></a>Impact  
*Le système d’exploitation invité sur les ordinateurs virtuels suivants ne peut pas s’exécuter ou peut s’exécuter unreliably :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V ou Windows PowerShell pour augmenter la mémoire minimale pour au moins 256 Mo et la mémoire de démarrage et la mémoire maximale au moins 512 Mo.*  
  
### <a name="increase-memory-using-hyper-v-manager"></a>Augmentez la mémoire à l’aide du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V. (Dans le Gestionnaire de serveur, cliquez sur **outils** > **Gestionnaire Hyper-V**.)  
  
2.  Dans la liste des machines virtuelles, cliquez sur celui de votre choix, puis cliquez sur **paramètres**.  
  
3.  Dans le volet de navigation, cliquez sur **mémoire**.  
  
4.  Modifier le **RAM** au moins 512 Mo.  
  
5.  Sous **mémoire dynamique**, modifier le **RAM Minimum** au moins 256 Mo et le **RAM maximale** à 512 Mo.  
  
6.  Cliquez sur **OK**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Augmentez la mémoire à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Avec le bouton droit **Windows PowerShell** et cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez une commande semblable à ce qui suit, en remplaçant MyVM par le nom de votre machine virtuelle et la mémoire des valeurs au moins les valeurs indiquées ci-dessous.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 512MB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


