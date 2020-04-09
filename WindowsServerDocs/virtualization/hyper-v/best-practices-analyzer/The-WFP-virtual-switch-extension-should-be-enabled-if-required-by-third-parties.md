---
title: L'extension de commutateur virtuel WFP doit être activée si elle est requise par les extensions tierces
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d4cc23ce638f7b5ee95f80de067b4ad5b360d118
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859302"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>L'extension de commutateur virtuel WFP doit être activée si elle est requise par les extensions tierces

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*L’extension de commutateur virtuel WFP (Windows Filtering Platform) est désactivée.*  
  
## <a name="impact"></a>**Effet**  
*Certaines extensions de commutateur virtuel tierces peuvent ne pas fonctionner correctement sur les commutateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez l’applet de commande Windows PowerShell Enable-VMSwitchExtension pour activer la plateforme de filtrage Windows si elle est requise par les extensions tierces.*  
  
### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>Activer la plateforme de filtrage Windows à l’aide de Windows PowerShell  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Cliquez avec le bouton droit sur **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Exécutez cette commande après avoir remplacé External par le nom de votre commutateur externe :  
  
```  
Enable-VMSwitchExtension -VMSwitchName External -Name Microsoft Windows Filtering Platform  
```  
  
## <a name="see-also"></a>Voir aussi  
[Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)  
  


