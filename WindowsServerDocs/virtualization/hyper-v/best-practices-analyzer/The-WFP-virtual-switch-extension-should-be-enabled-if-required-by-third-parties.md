---
title: L'extension de commutateur virtuel WFP doit être activée si elle est requise par les extensions tierces
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5afe706c246276597b32400109370ba3129e5a24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850620"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>L'extension de commutateur virtuel WFP doit être activée si elle est requise par les extensions tierces

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*L’extension de commutateur virtuel de plateforme de filtrage Windows (WFP) est désactivée.*  
  
## <a name="impact"></a>**Impact**  
*Certaines extensions de commutateur virtuel tiers peuvent ne pas fonctionnent correctement sur les commutateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez l’applet de commande Windows PowerShell, Enable-VMSwitchExtension, pour activer la plateforme de filtrage Windows si elle est requise par les extensions tierces.*  
  
### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>Activer la plateforme de filtrage Windows à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Avec le bouton droit **Windows PowerShell** et cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez cette commande après avoir remplacé externe par le nom de votre commutateur externe :  
  
```  
Enable-VMSwitchExtension -VMSwitchName External -Name "Microsoft Windows Filtering Platform"  
```  
  
## <a name="see-also"></a>Voir aussi  
[Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)  
  


