---
title: La configuration PVLAN sur un commutateur virtuel doit être cohérente
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 36616a5c4d8e57ae929cdab846db65dcdda57b85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364746"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>La configuration PVLAN sur un commutateur virtuel doit être cohérente

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016| 
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Le réseau local virtuel privé (PVLAN) n’est pas configuré correctement sur une ou plusieurs cartes réseau virtuelles.*  
  
## <a name="impact"></a>**Impact**  
*PVLAN peut ne pas isoler correctement le trafic réseau entre les machines virtuelles.*  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez l’applet de commande Windows PowerShell, Set-VMNetworkAdapterVlan, pour configurer correctement PVLAN.*  
  


