---
title: L’interface de l’équipe liée à un commutateur virtuel doit être en mode par défaut
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 22e5ad0eed6e6ea07a83150762b76163442f2c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872160"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>L’interface de l’équipe liée à un commutateur virtuel doit être en mode par défaut

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
*Certains commutateurs virtuels sont liés à une interface d’équipe, mais l’interface de l’équipe ne remplit pas le trafic sur tous les réseaux locaux virtuels pour les commutateurs virtuels.*  
  
## <a name="impact"></a>**Impact**  
*Les commutateurs virtuels suivants ne peuvent pas avoir accès à tous les réseaux locaux virtuels : \n{0}*  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez le Gestionnaire de serveur ou l’applet de commande Windows PowerShell Set-NetLbfoTeamNic pour réinitialiser l’interface d’équipe pour le mode par défaut.*  
  


