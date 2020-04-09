---
title: La configuration PVLAN sur un commutateur virtuel doit être cohérente
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 7e42c874e883157b3f85f523511b950e2863b155
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861852"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>La configuration PVLAN sur un commutateur virtuel doit être cohérente

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016| 
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Le réseau local virtuel privé (PVLAN) n’est pas configuré correctement sur une ou plusieurs cartes réseau virtuelles.*  
  
## <a name="impact"></a>**Effet**  
*PVLAN peut ne pas isoler correctement le trafic réseau entre les machines virtuelles.*  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez l’applet de commande Windows PowerShell, Set-VMNetworkAdapterVlan, pour configurer correctement PVLAN.*  
  


