---
title: Authentification par certificat est configurée, mais le certificat spécifié n’est pas installé sur le serveur réplica ou les nœuds de cluster de basculement
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: feef30d2f798057e4bd8e53ebab240af6b1d25b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832940"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>Authentification par certificat est configurée, mais le certificat spécifié n’est pas installé sur le serveur réplica ou les nœuds de cluster de basculement

>S'applique à : Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Le certificat de sécurité que le réplica Hyper-V a été configuré pour utiliser pour assurer la réplication basée sur le certificat n’est pas installée sur le serveur de réplication (ou les nœuds de cluster de basculement).*  
  
## <a name="impact"></a>Impact  
  
*En cas de basculement de cluster ou le déplacement vers un autre nœud, la réplication Hyper-V s’interrompt si le nouveau nœud n’a pas également le certificat approprié est installé. Ceci influe sur les nœuds suivants :*  
  
\<liste de nœuds >  
  
## <a name="resolution"></a>Résolution  
  
*Installez le certificat configuré sur le serveur de réplica (et tous les nœuds du cluster de basculement, le cas échéant).*  
  


