---
title: Configurer les systèmes d’exploitation invités, pour les sauvegardes VSS activer les instantanés cohérents au niveau de l’application pour réplica Hyper-V
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b4300dd4b7adc0cef8544215b5da62044a97301b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863890"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configurer les systèmes d’exploitation invités, pour les sauvegardes VSS activer les instantanés cohérents au niveau de l’application pour réplica Hyper-V

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Instantanés cohérents au niveau de l’application nécessite que le Volume Shadow Copy Services (VSS) est activé et configuré dans les systèmes d’exploitation invités d’ordinateurs virtuels participant à la réplication.*  
  
## <a name="impact"></a>Impact  
*Même si des captures instantanées cohérentes de l’application sont spécifiées dans la configuration de réplication, Hyper-V n’utilise pas les sauf si VSS est configuré. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utiliser la connexion de Machine virtuelle pour installer integration services dans la machine virtuelle.*  
  


