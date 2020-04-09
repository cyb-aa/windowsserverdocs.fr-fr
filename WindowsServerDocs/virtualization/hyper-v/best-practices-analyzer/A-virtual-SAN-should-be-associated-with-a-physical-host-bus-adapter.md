---
title: Un réseau SAN virtuel doit être associé à un adaptateur de bus hôte physique
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 10a17f8661f1436f5b4db317648edde57a0dfe74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857932"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Un réseau SAN virtuel doit être associé à un adaptateur de bus hôte physique

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
*Un réseau de zone de stockage (SAN) virtuel a été configuré sans association à une carte de bus hôte (HBA).*  
  
## <a name="impact"></a>**Effet**  
*Un ordinateur virtuel ne peut pas démarrer lorsqu’il est configuré avec une carte Fibre Channel virtuelle connectée à un réseau SAN virtuel mal configuré. Cela a un impact sur les réseaux SAN virtuels suivants :*  
  
  
\<la liste des réseaux SAN virtuels >  
  
  
## <a name="resolution"></a>**Résolution**  
*Reconfigurez le réseau SAN virtuel en le connectant à un adaptateur de bus hôte.*  
  
  
  


