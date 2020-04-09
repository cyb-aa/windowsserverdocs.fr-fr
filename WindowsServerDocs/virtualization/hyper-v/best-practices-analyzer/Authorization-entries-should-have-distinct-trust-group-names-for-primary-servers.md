---
title: Les entrées d’autorisation doivent avoir des noms de groupe d’approbation distincts pour les serveurs principaux avec des ordinateurs virtuels qui ne font pas partie du même groupe d’approbations.
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 22e1a3bdeb40c5440862b4931dda344756cc34a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857812"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>Les entrées d’autorisation doivent avoir des noms de groupe d’approbation distincts pour les serveurs principaux avec des ordinateurs virtuels qui ne font pas partie du même groupe d’approbations.

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
*Le serveur acceptera les demandes de réplication pour l’ordinateur virtuel de réplication à partir de l’un des serveurs de la liste d’autorisations associée à la même balise de réplication que l’ordinateur virtuel.*  
  
## <a name="impact"></a>**Effet**  
*Il peut y avoir des problèmes de sécurité et de confidentialité avec une machine virtuelle acceptant la réplication à partir des serveurs principaux appartenant à différentes entrées d’autorisation. Cela a un impact sur les entrées d’autorisation suivantes : \<liste des entrées d’autorisation >*  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez des balises différentes dans les entrées d’autorisation pour les serveurs principaux avec des ordinateurs virtuels qui ne font pas partie du même groupe de sécurité. Modifiez les paramètres Hyper-V pour configurer les balises de réplication.*  
  


