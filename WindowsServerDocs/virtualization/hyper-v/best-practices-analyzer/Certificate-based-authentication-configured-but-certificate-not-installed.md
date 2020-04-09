---
title: L’authentification basée sur les certificats est configurée, mais le certificat spécifié n’est pas installé sur le serveur de réplication ou les nœuds de cluster de basculement
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: cc89aa201de4b25e4c221b770e6f88908785c859
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857682"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>L’authentification basée sur les certificats est configurée, mais le certificat spécifié n’est pas installé sur le serveur de réplication ou les nœuds de cluster de basculement

>S’applique à Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Le certificat de sécurité que le réplica Hyper-V a configuré pour fournir la réplication basée sur les certificats n’est pas installé sur le serveur de réplication (ou sur tous les nœuds de cluster de basculement).*  
  
## <a name="impact"></a>Impact  
  
*En cas de basculement ou de déplacement d’un cluster vers un autre nœud, la réplication Hyper-V s’interrompt si le certificat approprié n’est pas installé sur le nouveau nœud. Cela a un impact sur les nœuds suivants :*  
  
\<liste des nœuds >  
  
## <a name="resolution"></a>Résolution  
  
*Installez le certificat configuré sur le serveur de réplication (et tous les nœuds associés dans le cluster de basculement, le cas échéant).*  
  


