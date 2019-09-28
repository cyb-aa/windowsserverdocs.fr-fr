---
title: Une ou plusieurs cartes réseau doivent être configurées comme destination pour la mise en miroir des ports
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b83c166d-f010-47c4-a4bb-02167f2e3361
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: af51b854659adae1bf3132eed4d68e95467bdf85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364813"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-destination-for-port-mirroring"></a>Une ou plusieurs cartes réseau doivent être configurées comme destination pour la mise en miroir des ports

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Une ou plusieurs machines virtuelles ont une carte réseau configurée comme source pour la mise en miroir des ports, mais il n’existe aucune destination correspondante sur le commutateur virtuel.*  
  
## <a name="impact"></a>**Impact**  
*La mise en miroir des ports ne fonctionnera pas correctement pour les commutateurs virtuels et les ordinateurs virtuels suivants :*  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez Windows PowerShell ou le Gestionnaire Hyper-V pour terminer ou corriger la configuration de la mise en miroir des ports.*  
  


