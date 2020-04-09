---
title: Au moins un réseau pour le trafic de migration dynamique doit avoir une vitesse de liaison d’au moins 1 Gbits/s
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5714df3f-f810-4618-8c93-e24881651100
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 04ce4ea86e39e8bd98216ae4e6b12899c9366421
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857822"
---
# <a name="at-least-one-network-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Au moins un réseau pour le trafic de migration dynamique doit avoir une vitesse de liaison d’au moins 1 Gbits/s

>S’applique à Windows Server 2016


  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Aucun des réseaux pour le trafic de migration dynamique n’a une vitesse de liaison d’au moins 1 Gbits/s.*  
  
## <a name="impact"></a>Impact  
*Les migrations dynamiques peuvent survenir lentement, ce qui peut perturber la connexion réseau en raison d’un délai de connexion TCP.*  
  
## <a name="resolution"></a>Résolution  
*Configurez au moins un réseau de migration dynamique avec une vitesse de 1 Gbit/s ou plus.*  
  
Consultez la documentation de votre fournisseur de matériel réseau pour savoir si l’une de vos cartes réseau existantes peut prendre en charge une vitesse de liaison d’au moins 1 Gbits/s.  
  


