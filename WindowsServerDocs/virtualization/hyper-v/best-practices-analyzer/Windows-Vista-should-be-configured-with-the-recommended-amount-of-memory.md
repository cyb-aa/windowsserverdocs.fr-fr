---
title: Windows Vista doit être configuré avec la quantité de mémoire recommandée.
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 64f4e53b-4adb-4e1d-bc48-c24f5f9d222f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 698e795a9157383063c54eb3cb06461ff58532f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364352"
---
# <a name="windows-vista-should-be-configured-with-the-recommended-amount-of-memory"></a>Windows Vista doit être configuré avec la quantité de mémoire recommandée.

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Une machine virtuelle exécutant Windows Vista est configurée avec une quantité inférieure à la quantité recommandée de RAM, qui est de 1 Go.*  
  
## <a name="impact"></a>Impact  
  
les applications et le système d’exploitation invité @no__t 0The peuvent ne pas fonctionner correctement. Il se peut que la mémoire soit insuffisante pour exécuter plusieurs applications à la fois. Cela a un impact sur les ordinateurs virtuels suivants : *  
  
@no__t 0list de noms de machines virtuelles >  
  
## <a name="resolution"></a>Résolution :  
  
*Utilisez le Gestionnaire Hyper-V pour augmenter la mémoire allouée à cette machine virtuelle à au moins 1 Go.*  
  
### <a name="to-increase-the-memory-allocated-to-a-virtual-machine"></a>Pour augmenter la mémoire allouée à un ordinateur virtuel  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **machines virtuelles**, sélectionnez la machine virtuelle que vous souhaitez configurer. L’état de la machine virtuelle doit être défini sur **désactivé**. Si ce n’est pas le cas, cliquez avec le bouton droit sur l’ordinateur virtuel, puis cliquez sur **arrêter**.  
  
3.  Dans le volet **Action**, sous le nom de l'ordinateur virtuel, cliquez sur **Paramètres**.  
  
4.  Dans le volet de navigation, cliquez sur **mémoire**.  
  
5.  Sur la page **mémoire** , définissez la **RAM de démarrage** sur au moins 1 Go, puis cliquez sur **OK**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Augmenter la mémoire à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Cliquez avec le bouton droit sur **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez cette commande après avoir remplacé \<MyVM > par le nom de votre machine virtuelle :  
  
```  
Set-VMMemory <MyVM> -StartupBytes 1GB  
```  
  
## <a name="see-also"></a>Voir aussi  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


