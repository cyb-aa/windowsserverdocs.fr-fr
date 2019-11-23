---
title: L’interface d’équipe liée à un commutateur virtuel doit être en mode par défaut
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9bfd0c98e865a0faae8dd70e97696e2c2682531b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393430"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>L’interface d’équipe liée à un commutateur virtuel doit être en mode par défaut

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Certains commutateurs virtuels sont liés à une interface d’équipe, mais l’interface de l’équipe ne transmet pas le trafic sur tous les réseaux locaux virtuels aux commutateurs virtuels.*  
  
## <a name="impact"></a>**Impact**  
*Les commutateurs virtuels suivants ne peuvent pas avoir accès à tous les réseaux locaux virtuels : \n{0}*  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez Gestionnaire de serveur ou l’applet de commande Windows PowerShell Set-NetLbfoTeamNic pour réinitialiser l’interface de l’équipe en mode par défaut.*  
  


