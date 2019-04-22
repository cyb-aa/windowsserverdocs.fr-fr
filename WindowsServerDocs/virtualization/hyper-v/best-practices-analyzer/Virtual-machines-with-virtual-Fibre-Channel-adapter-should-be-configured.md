---
title: Ordinateurs virtuels configurés avec une carte Fibre Channel virtuelle doit être configurés pour la haute disponibilité pour le stockage basé sur Fibre Channel
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 203477a022f7c5f819ef7b99f1b8e37a0b8b0a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816940"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Ordinateurs virtuels configurés avec une carte Fibre Channel virtuelle doit être configurés pour la haute disponibilité pour le stockage basé sur Fibre Channel

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Informations|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Un ou plusieurs machines virtuelles ne disposent pas une connexion au stockage Fibre Channel hautement disponible, car ces ordinateurs virtuels sont configurés avec une carte Fibre Channel virtuelle qui est connectée à l’adaptateur de bus qu’un seul hôte (HBA).*  
  
## <a name="impact"></a>**Impact**  
*Une défaillance de l’adaptateur de bus hôte susceptibles de bloquer la connexion Fibre Channel entre le stockage et les machines virtuelles. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Ajouter une autre connexion à partir de la machine virtuelle à l’adaptateur de bus hôte et configurer Multipath i/o (MPIO) de d’e/s dans le système d’exploitation invité pour établir des connexions redondantes Fibre Channel.*  
  


