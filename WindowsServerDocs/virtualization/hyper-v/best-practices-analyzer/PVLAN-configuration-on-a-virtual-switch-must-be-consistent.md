---
title: Configuration de PVLAN sur un commutateur virtuel doit être cohérente
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b7d6d068027aa9497b00138bd1d889ea86aa3308
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813250"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>Configuration de PVLAN sur un commutateur virtuel doit être cohérente

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016| 
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Réseau Local (PVLAN) privé virtuel n’est pas correctement configuré sur un ou plusieurs cartes réseau virtuelles.*  
  
## <a name="impact"></a>**Impact**  
*PVLAN ne peut pas isoler le trafic réseau entre les machines virtuelles correctement.*  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez l’applet de commande Windows PowerShell, Set-VMNetworkAdapterVlan, pour configurer les PVLAN correctement.*  
  


