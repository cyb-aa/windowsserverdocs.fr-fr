---
title: Configurez au moins la quantité de mémoire requise pour un ordinateur virtuel exécutant Windows Server 2008 et activé pour Mémoire dynamique
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 0aaa7138-ce83-4b17-add7-fdfa3d469bea
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: f34332724bad3d2a0d150496ea8f0c610f5fd2b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862102"
---
# <a name="configure-at-least-the-required-amount-of-memory-for-a-virtual-machine-running-windows-server-2008-and-enabled-for-dynamic-memory"></a>Configurez au moins la quantité de mémoire requise pour un ordinateur virtuel exécutant Windows Server 2008 et activé pour Mémoire dynamique

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Une ou plusieurs machines virtuelles sont configurées pour utiliser Mémoire dynamique avec une quantité de mémoire inférieure à celle requise pour Windows Server 2008.*  
  
## <a name="impact"></a>Impact  
*Le système d’exploitation invité sur les ordinateurs virtuels suivants peut ne pas s’exécuter ou ne pas fonctionner de manière fiable :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V ou Windows PowerShell pour augmenter la mémoire minimale à au moins 256 Mo, et la mémoire de démarrage et la mémoire maximale à au moins 512 Mo.*  
  
### <a name="increase-memory-using-hyper-v-manager"></a>Augmenter la mémoire à l’aide du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V. (À partir de Gestionnaire de serveur, cliquez sur **outils** > **Gestionnaire Hyper-V**.)  
  
2.  Dans la liste des ordinateurs virtuels, cliquez avec le bouton droit sur celui que vous souhaitez, puis cliquez sur **paramètres**.  
  
3.  Dans le volet de navigation, cliquez sur **mémoire**.  
  
4.  Remplacez la **RAM** par au moins 512 Mo.  
  
5.  Sous **mémoire dynamique**, définissez la **RAM minimale** sur au moins 256 Mo et la **RAM maximale** sur 512 Mo.  
  
6.  Cliquez sur **OK**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Augmenter la mémoire à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Cliquez avec le bouton droit sur **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez une commande semblable à la suivante, en remplaçant MyVM par le nom de votre machine virtuelle et les valeurs de mémoire par au moins les valeurs indiquées ci-dessous.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 512MB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


