---
title: L’interface d’équipe liée à un commutateur virtuel doit être en mode par défaut
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: fe19de5dd380d08c01c917da9d4e2ef9465de042
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854602"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>L’interface d’équipe liée à un commutateur virtuel doit être en mode par défaut

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
*Certains commutateurs virtuels sont liés à une interface d’équipe, mais l’interface de l’équipe ne transmet pas le trafic sur tous les réseaux locaux virtuels aux commutateurs virtuels.*  
  
## <a name="impact"></a>**Effet**  
*Les commutateurs virtuels suivants ne peuvent pas avoir accès à tous les réseaux locaux virtuels : \n{0}*  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez Gestionnaire de serveur ou l’applet de commande Windows PowerShell Set-NetLbfoTeamNic pour réinitialiser l’interface de l’équipe en mode par défaut.*  
  


