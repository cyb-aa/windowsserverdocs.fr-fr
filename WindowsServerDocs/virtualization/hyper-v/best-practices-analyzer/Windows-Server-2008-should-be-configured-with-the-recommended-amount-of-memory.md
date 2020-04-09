---
title: Windows Server 2008 doit être configuré avec la quantité de mémoire recommandée
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a98a8594-603b-487a-8739-78887c568e57
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1df2c7d10f35c7dcabbb210d8bd09233468697c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861022"
---
# <a name="windows-server-2008-should-be-configured-with-the-recommended-amount-of-memory"></a>Windows Server 2008 doit être configuré avec la quantité de mémoire recommandée

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
  
*Une machine virtuelle exécutant Windows Server 2008 est configurée avec une quantité de RAM inférieure à la quantité recommandée, qui est de 2 Go.*  
  
## <a name="impact"></a>Impact  
  
*Le système d’exploitation invité et les applications peuvent ne pas fonctionner correctement. Il se peut que la mémoire soit insuffisante pour exécuter plusieurs applications à la fois. Cela a un impact sur les machines virtuelles suivantes :*  
   
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le Gestionnaire Hyper-V pour augmenter la mémoire allouée à cette machine virtuelle à au moins 2 Go.*  
  
### <a name="increase-the-memory-using-hyper-v-manager"></a>Augmenter la mémoire à l’aide du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **machines virtuelles**, sélectionnez la machine virtuelle que vous souhaitez configurer. L’état de la machine virtuelle doit être défini sur **désactivé**. Si ce n’est pas le cas, cliquez avec le bouton droit sur l’ordinateur virtuel, puis cliquez sur **arrêter**.  
  
3.  Dans le volet **Action**, sous le nom de l’ordinateur virtuel, cliquez sur **Paramètres**.  
  
4.  Dans le volet de navigation, cliquez sur **mémoire**.  
  
5.  Sur la page **mémoire** , définissez la **RAM de démarrage** sur au moins 2 Go, puis cliquez sur **OK**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Augmenter la mémoire à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Cliquez avec le bouton droit sur **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez une commande semblable à la suivante, en remplaçant \<MyVM > par le nom de votre machine virtuelle et les valeurs de mémoire par au moins les valeurs indiquées ci-dessous.  
  
```  
Set-VMMemory <MyVM> -StartupBytes 2GB  
```  
  
## <a name="see-also"></a>Voir aussi  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


