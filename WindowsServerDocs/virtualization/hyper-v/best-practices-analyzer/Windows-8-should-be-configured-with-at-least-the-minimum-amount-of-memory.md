---
title: Windows 8 doit être configuré avec au moins la quantité minimale de mémoire
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 519d1091-fa4d-44d7-83ca-83f6aa71fb7d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1c979299fbd66ef23ae637488fe3c4f3a3ccbbc6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857652"
---
# <a name="windows-8-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>Windows 8 doit être configuré avec au moins la quantité minimale de mémoire

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Les sections suivantes fournissent des détails sur le problème spécifique. L’italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour le problème spécifique.  
  
## <a name="issue"></a>**Problème**  
*Une machine virtuelle exécutant Windows 8 est configurée avec une quantité inférieure à la quantité minimale de RAM, qui est de 512 Mo.*  
  
## <a name="impact"></a>**Effet**  
*Le système d’exploitation invité sur les ordinateurs virtuels suivants peut ne pas s’exécuter ou ne pas fonctionner de manière fiable :*  
```  
<list of virtual machines>  
```  
## <a name="resolution"></a>**Résolution**  
*Utilisez le Gestionnaire Hyper-V pour augmenter la mémoire allouée à cette machine virtuelle à au moins 512 Mo.*  
  
#### <a name="increase-the-memory-using-hyper-v-manager"></a>Augmenter la mémoire à l’aide du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **machines virtuelles**, sélectionnez la machine virtuelle que vous souhaitez configurer. L’état de la machine virtuelle doit être défini sur **désactivé**. Si ce n’est pas le cas, cliquez avec le bouton droit sur l’ordinateur virtuel, puis cliquez sur **arrêter**.  
  
3.  Dans le volet **Action**, sous le nom de l’ordinateur virtuel, cliquez sur **Paramètres**.  
  
4.  Dans le volet de navigation, cliquez sur **mémoire**.  
  
5.  Sur la page **mémoire** , définissez la **RAM de démarrage** sur au moins 512 Mo, puis cliquez sur **OK**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Augmenter la mémoire à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Cliquez avec le bouton droit sur **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez cette commande après avoir remplacé \<MyVM > par le nom de votre machine virtuelle :  
  
```  
Set-VMMemory <MyVM> -StartupBytes 512MB  
```  
  
## <a name="see-also"></a>Voir aussi  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


