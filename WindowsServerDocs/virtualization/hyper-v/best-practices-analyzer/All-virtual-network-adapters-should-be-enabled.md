---
title: Toutes les cartes réseau virtuelles doivent être activées
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fce564fdb47d0677b36078f3d8446579bc06816c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366590"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Toutes les cartes réseau virtuelles doivent être activées

>S'applique à : Windows Server 2016


  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Une ou plusieurs cartes réseau virtuelles associées à une carte réseau physique sont désactivées dans le système d’exploitation de gestion.*  
  
## <a name="impact"></a>Impact  
  
*La configuration de ce serveur n’est pas optimale.*  
  
Le système d’exploitation de gestion ne peut pas se connecter à un réseau physique (externe) à l’aide de l’une des cartes réseau physiques de cet ordinateur, car il est associé à une carte réseau virtuelle désactivée.  
  
## <a name="resolution"></a>Résolution :  
  
*Use réseau & paramètres Internet pour activer la carte réseau virtuelle. Vous pouvez aussi utiliser le gestionnaire de commutateur virtuel pour reconfigurer le commutateur virtuel externe afin qu’il ne soit pas partagé avec le système d’exploitation de gestion.*  
  


