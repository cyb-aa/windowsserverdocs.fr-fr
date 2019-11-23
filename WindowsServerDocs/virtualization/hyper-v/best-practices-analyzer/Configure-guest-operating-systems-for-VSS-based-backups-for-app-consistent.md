---
title: Configurer des systèmes d’exploitation invités pour les sauvegardes basées sur VSS afin d’activer des captures instantanées cohérentes avec les applications pour le réplica Hyper-V
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 032ca585da1c556fff6f9e06b3bde0662a5d64db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364958"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configurer des systèmes d’exploitation invités pour les sauvegardes basées sur VSS afin d’activer des captures instantanées cohérentes avec les applications pour le réplica Hyper-V

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Les captures instantanées cohérentes avec les applications requièrent que le service VSS (Volume Shadow Copy Services) soit activé et configuré dans les systèmes d’exploitation invités des ordinateurs virtuels participant à la réplication.*  
  
## <a name="impact"></a>Impact  
*Même si des captures instantanées de cohérence des applications sont spécifiées dans la configuration de réplication, Hyper-V ne les utilise pas, sauf si VSS est configuré. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez connexion à un ordinateur virtuel pour installer les services d’intégration sur l’ordinateur virtuel.*  
  


