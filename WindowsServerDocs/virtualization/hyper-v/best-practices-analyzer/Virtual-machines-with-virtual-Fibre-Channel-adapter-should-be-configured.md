---
title: Les machines virtuelles configurées avec une carte Fibre Channel virtuelle doivent être configurées pour une haute disponibilité sur le stockage basé sur Fibre Channel
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b4c50ac70b51ab6a2e5cb8247b309070d85932a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364560"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Les machines virtuelles configurées avec une carte Fibre Channel virtuelle doivent être configurées pour une haute disponibilité sur le stockage basé sur Fibre Channel

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Informations|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Une ou plusieurs machines virtuelles ne disposent pas d’une connexion à haut niveau de disponibilité pour le stockage Fibre Channel, car ces machines virtuelles sont configurées avec une carte Fibre Channel virtuelle qui est connectée à un seul adaptateur de bus hôte (HBA).*  
  
## <a name="impact"></a>**Impact**  
*Une défaillance de l’adaptateur de bus hôte peut bloquer la connexion Fibre Channel entre le stockage et les ordinateurs virtuels. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Ajoutez une autre connexion de la machine virtuelle à l’adaptateur de bus hôte et configurez MPIO (Multipath I/O) dans le système d’exploitation invité pour établir des connexions Fibre Channel redondantes.*  
  


