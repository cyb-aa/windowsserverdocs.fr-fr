---
title: Une équipe liée à un commutateur virtuel ne doit avoir qu’une seule interface d’équipe exposée
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6baa9e4ae900c9b671003872b4eb4589efb2f085
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365399"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Une équipe liée à un commutateur virtuel ne doit avoir qu’une seule interface d’équipe exposée

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
*Un ou plusieurs commutateurs virtuels sont liés à une équipe qui a plusieurs interfaces d’équipe.*  
  
## <a name="impact"></a>**Impact**  
*Les commutateurs virtuels suivants n’ont peut-être pas accès aux réseaux locaux virtuels et à la bande passante utilisés par d’autres interfaces d’équipe :*  
  
\<liste des commutateurs virtuels >  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez l’applet de commande Windows PowerShell Remove-NetLbfoTeamNic pour supprimer toutes les interfaces d’équipe de l’équipe, à l’exception de l’interface d’équipe par défaut.*  
  


