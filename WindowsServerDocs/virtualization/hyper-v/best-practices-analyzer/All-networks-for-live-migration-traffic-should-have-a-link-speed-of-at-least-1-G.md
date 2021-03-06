---
title: Tous les réseaux pour le trafic de migration dynamique doivent avoir une vitesse de liaison d’au moins 1 Gbits/s
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 89411b63-bec8-463d-b486-107548ed440e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: adb16b1c4618e0874f48f4715440a9d903f5bc9f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857842"
---
# <a name="all-networks-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Tous les réseaux pour le trafic de migration dynamique doivent avoir une vitesse de liaison d’au moins 1 Gbits/s

>S’applique à Windows Server 2016


  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Aucun des réseaux pour le trafic de migration dynamique n’a une vitesse de liaison d’au moins 1 Gbits/s.*  
  
## <a name="impact"></a>Impact  
*Les migrations dynamiques peuvent survenir lentement, ce qui peut perturber la connexion réseau en raison d’un délai de connexion TCP.*  
  
## <a name="resolution"></a>Résolution  
*Configurez au moins un réseau de migration dynamique avec une vitesse de 1 Gbit/s ou plus.*  
  
Consultez la documentation de votre fournisseur de matériel réseau pour savoir si l’une de vos cartes réseau existantes peut prendre en charge une vitesse de liaison d’au moins 1 Gbits/s.  
  


