---
title: Les entrées d’autorisation doivent avoir des noms de groupe d’approbation distincts pour les serveurs principaux avec des ordinateurs virtuels qui ne font pas partie du même groupe d’approbations.
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ab71e0e6562b73d81b871914fd0e9e76570c518e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366541"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>Les entrées d’autorisation doivent avoir des noms de groupe d’approbation distincts pour les serveurs principaux avec des ordinateurs virtuels qui ne font pas partie du même groupe d’approbations.

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Le serveur acceptera les demandes de réplication pour l’ordinateur virtuel de réplication à partir de l’un des serveurs de la liste d’autorisations associée à la même balise de réplication que l’ordinateur virtuel.*  
  
## <a name="impact"></a>**Impact**  
*Il peut y avoir des problèmes de sécurité et de confidentialité avec une machine virtuelle acceptant la réplication à partir des serveurs principaux appartenant à différentes entrées d’autorisation. Cela a un impact sur les entrées d’autorisation suivantes : \<liste des entrées d’autorisation >*  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez des balises différentes dans les entrées d’autorisation pour les serveurs principaux avec des ordinateurs virtuels qui ne font pas partie du même groupe de sécurité. Modifiez les paramètres Hyper-V pour configurer les balises de réplication.*  
  


