---
title: Toutes les cartes réseau virtuel doivent être activés.
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0a769c3203f6c6946f01cd91b66fbec38af83bbd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837130"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Toutes les cartes réseau virtuel doivent être activés.

>S'applique à : Windows Server 2016


  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Un ou plusieurs cartes réseau virtuelles associées à une carte réseau physique sont désactivées dans le système d’exploitation de gestion.*  
  
## <a name="impact"></a>Impact  
  
*La configuration de ce serveur n’est pas optimale.*  
  
Le système d’exploitation de gestion ne peut pas se connecter à un réseau physique (externe), à l’aide d’une des cartes réseau physiques sur cet ordinateur, car il est associé à une carte réseau virtuelle désactivé.  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez les paramètres Internet & réseau pour activer la carte réseau virtuelle. Sinon, utilisez le Gestionnaire de commutateur virtuel pour reconfigurer le commutateur virtuel externe afin qu’il n’est pas partagé avec le système d’exploitation de gestion.*  
  


