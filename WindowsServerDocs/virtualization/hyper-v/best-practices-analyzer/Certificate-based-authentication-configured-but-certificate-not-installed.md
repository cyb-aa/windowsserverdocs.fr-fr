---
title: L’authentification basée sur les certificats est configurée, mais le certificat spécifié n’est pas installé sur le serveur de réplication ou les nœuds de cluster de basculement
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0b107a4760cc3470c7f80d53feef00a2f8f789c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365203"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>L’authentification basée sur les certificats est configurée, mais le certificat spécifié n’est pas installé sur le serveur de réplication ou les nœuds de cluster de basculement

>S’applique à Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Erreur|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Le certificat de sécurité que le réplica Hyper-V a configuré pour fournir la réplication basée sur les certificats n’est pas installé sur le serveur de réplication (ou sur tous les nœuds de cluster de basculement).*  
  
## <a name="impact"></a>Impact  
  
*En cas de basculement ou de déplacement d’un cluster vers un autre nœud, la réplication Hyper-V s’interrompt si le certificat approprié n’est pas installé sur le nouveau nœud. Cela a un impact sur les nœuds suivants :*  
  
\<liste des nœuds >  
  
## <a name="resolution"></a>Résolution  
  
*Installez le certificat configuré sur le serveur de réplication (et tous les nœuds associés dans le cluster de basculement, le cas échéant).*  
  


