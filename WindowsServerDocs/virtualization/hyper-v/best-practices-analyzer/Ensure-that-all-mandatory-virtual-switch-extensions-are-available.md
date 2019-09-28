---
title: Vérifier que toutes les extensions de commutateur virtuel obligatoires sont disponibles
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c9363fbce35552a8f7d279662ae9072bcd7ea480
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364823"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Vérifier que toutes les extensions de commutateur virtuel obligatoires sont disponibles

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Une ou plusieurs cartes réseau virtuelles sont connectées à un commutateur virtuel avec des extensions obligatoires qui sont désactivées ou ne sont pas installées.*  
  
## <a name="impact"></a>Impact  
*Le trafic réseau est bloqué sur une ou plusieurs cartes réseau virtuelles sur les machines virtuelles suivantes :*  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>Résolution :  
*First, assurez-vous que l’extension obligatoire a été installée sur l’hôte et installez l’extension si nécessaire. Ensuite, si l’extension obligatoire est désactivée, utilisez le gestionnaire de commutateur virtuel ou l’applet de commande Windows PowerShell Enable-VMSwitchExtension pour activer l’extension.*  
  


