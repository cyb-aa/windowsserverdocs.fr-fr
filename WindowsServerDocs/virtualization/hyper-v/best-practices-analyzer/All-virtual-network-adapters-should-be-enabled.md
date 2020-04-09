---
title: Toutes les cartes réseau virtuelles doivent être activées
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 450a3b42529be9a85991fcaf5263bae7b7827b1a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857832"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Toutes les cartes réseau virtuelles doivent être activées

>S’applique à Windows Server 2016


  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Une ou plusieurs cartes réseau virtuelles associées à une carte réseau physique sont désactivées dans le système d’exploitation de gestion.*  
  
## <a name="impact"></a>Impact  
  
*La configuration de ce serveur n’est pas optimale.*  
  
Le système d’exploitation de gestion ne peut pas se connecter à un réseau physique (externe) à l’aide de l’une des cartes réseau physiques de cet ordinateur, car il est associé à une carte réseau virtuelle désactivée.  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez les paramètres réseau & Internet pour activer la carte réseau virtuelle. Vous pouvez aussi utiliser le gestionnaire de commutateur virtuel pour reconfigurer le commutateur virtuel externe afin qu’il ne soit pas partagé avec le système d’exploitation de gestion.*  
  


