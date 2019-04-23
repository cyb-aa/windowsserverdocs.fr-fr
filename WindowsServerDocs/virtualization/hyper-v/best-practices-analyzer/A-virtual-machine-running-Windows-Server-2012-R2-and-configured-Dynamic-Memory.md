---
title: Une machine virtuelle exécutant Windows Server 2012 R2 et configurés avec la mémoire dynamique doit utiliser les valeurs recommandées pour les paramètres de mémoire
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 3a53c197-80ce-4b33-a83e-7e89e657a519
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9ea9b1c2ae377b39a93e332ea1d403388769b2fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890160"
---
# <a name="a-virtual-machine-running-windows-server-2012-r2-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>Une machine virtuelle exécutant Windows Server 2012 R2 et configurés avec la mémoire dynamique doit utiliser les valeurs recommandées pour les paramètres de mémoire

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
*Un ou plusieurs ordinateurs virtuels sont configurés pour utiliser la mémoire dynamique avec inférieure à la quantité de mémoire recommandée pour Windows Server 2012 R2.*  
  
## <a name="impact"></a>Impact  
*Le système d’exploitation invité sur les ordinateurs virtuels suivants ne peut pas s’exécuter ou peut s’exécuter unreliably :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V pour augmenter la mémoire minimale pour au moins 256 Mo, mémoire de démarrage au moins 512 Mo et de mémoire maximale au moins 2 Go pour cet ordinateur virtuel.*  
  
#### <a name="increase-memory-using-hyper-v-manager"></a>Augmentez la mémoire à l’aide du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V. (Dans le Gestionnaire de serveur, cliquez sur **outils** > **Gestionnaire Hyper-V**.)  
  
2.  Dans la liste des machines virtuelles, cliquez sur celui de votre choix, puis cliquez sur **paramètres**.  
  
3.  Dans le volet de navigation, cliquez sur **mémoire**.  
  
4.  Modifier le **RAM** au moins 512 Mo.  
  
5.  Sous **mémoire dynamique**, modifiez le **RAM Minimum** au moins 256 Mo et le **RAM maximale** à 2 Go.  
  
6.  Cliquez sur **OK**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Augmentez la mémoire à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Avec le bouton droit **Windows PowerShell** et cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez une commande semblable à ce qui suit, en remplaçant MyVM par le nom de votre machine virtuelle et la mémoire des valeurs au moins les valeurs indiquées ci-dessous.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 2GB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


