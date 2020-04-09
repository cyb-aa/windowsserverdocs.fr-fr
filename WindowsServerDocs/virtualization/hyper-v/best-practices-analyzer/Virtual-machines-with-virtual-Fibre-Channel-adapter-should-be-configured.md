---
title: Les machines virtuelles configurées avec une carte Fibre Channel virtuelle doivent être configurées pour une haute disponibilité sur le stockage basé sur Fibre Channel
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: e04b52fc98fd79024970ed525e902132d97701e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855002"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Les machines virtuelles configurées avec une carte Fibre Channel virtuelle doivent être configurées pour une haute disponibilité sur le stockage basé sur Fibre Channel

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Information|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Une ou plusieurs machines virtuelles ne disposent pas d’une connexion à haut niveau de disponibilité pour le stockage Fibre Channel, car ces machines virtuelles sont configurées avec une carte Fibre Channel virtuelle qui est connectée à un seul adaptateur de bus hôte (HBA).*  
  
## <a name="impact"></a>**Effet**  
*Une défaillance de l’adaptateur de bus hôte peut bloquer la connexion Fibre Channel entre le stockage et les ordinateurs virtuels. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Ajoutez une autre connexion de la machine virtuelle à l’adaptateur de bus hôte et configurez MPIO (Multipath I/O) dans le système d’exploitation invité pour établir des connexions Fibre Channel redondantes.*  
  


