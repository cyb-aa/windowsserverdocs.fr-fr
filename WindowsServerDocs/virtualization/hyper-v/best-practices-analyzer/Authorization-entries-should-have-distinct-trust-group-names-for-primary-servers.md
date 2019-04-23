---
title: Entrées d’autorisation doivent porter des noms de groupe de confiance distinctes pour les serveurs principaux avec les machines virtuelles qui ne font pas partie du même groupe de confiance
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c5b1a6bf8ef0bbceb5dde6b28cd951f399fc5e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882960"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>Entrées d’autorisation doivent porter des noms de groupe de confiance distinctes pour les serveurs principaux avec les machines virtuelles qui ne font pas partie du même groupe de confiance

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Le serveur accepte les demandes de réplication pour la machine virtuelle de réplication à partir de tous les serveurs dans la liste d’autorisations associé à la même balise de réplication en tant que la machine virtuelle.*  
  
## <a name="impact"></a>**Impact**  
*Il peut y avoir des problèmes de sécurité avec une machine virtuelle acceptation de la réplication à partir de serveurs principaux qui appartiennent à des entrées d’autorisation différents et de confidentialité. Cela affecte les entrées d’autorisation suivantes : \<liste des entrées d’autorisation >*  
  
## <a name="resolution"></a>**Résolution**  
*Utiliser des balises différentes dans les entrées d’autorisation pour les serveurs principaux avec les machines virtuelles qui ne font pas partie du même groupe de sécurité. Modifiez les paramètres Hyper-V pour configurer les balises de réplication.*  
  


