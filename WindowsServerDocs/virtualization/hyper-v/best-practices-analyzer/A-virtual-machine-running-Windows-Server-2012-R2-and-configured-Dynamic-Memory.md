---
title: Une machine virtuelle exécutant Windows Server 2012 R2 et configurée avec Mémoire dynamique doit utiliser les valeurs recommandées pour les paramètres de mémoire
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 3a53c197-80ce-4b33-a83e-7e89e657a519
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 855d6eba2109f91e25410d37d9167ed777e8969a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857872"
---
# <a name="a-virtual-machine-running-windows-server-2012-r2-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>Une machine virtuelle exécutant Windows Server 2012 R2 et configurée avec Mémoire dynamique doit utiliser les valeurs recommandées pour les paramètres de mémoire

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Une ou plusieurs machines virtuelles sont configurées pour utiliser Mémoire dynamique avec une quantité inférieure à la quantité de mémoire recommandée pour Windows Server 2012 R2.*  
  
## <a name="impact"></a>Impact  
*Le système d’exploitation invité sur les ordinateurs virtuels suivants peut ne pas s’exécuter ou ne pas fonctionner de manière fiable :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V pour augmenter la mémoire minimale à au moins 256 Mo, la mémoire de démarrage à au moins 512 Mo et la mémoire maximale à au moins 2 Go pour cette machine virtuelle.*  
  
#### <a name="increase-memory-using-hyper-v-manager"></a>Augmenter la mémoire à l’aide du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V. (À partir de Gestionnaire de serveur, cliquez sur **outils** > **Gestionnaire Hyper-V**.)  
  
2.  Dans la liste des ordinateurs virtuels, cliquez avec le bouton droit sur celui que vous souhaitez, puis cliquez sur **paramètres**.  
  
3.  Dans le volet de navigation, cliquez sur **mémoire**.  
  
4.  Remplacez la **RAM** par au moins 512 Mo.  
  
5.  Sous **mémoire dynamique**, définissez la **RAM minimale** sur au moins 256 Mo et la **RAM maximale** sur 2 Go.  
  
6.  Cliquez sur **OK**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Augmenter la mémoire à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Cliquez avec le bouton droit sur **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez une commande semblable à la suivante, en remplaçant MyVM par le nom de votre machine virtuelle et les valeurs de mémoire par au moins les valeurs indiquées ci-dessous.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 2GB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


