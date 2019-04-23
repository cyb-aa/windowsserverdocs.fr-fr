---
title: Configurer les paramètres TCP/IP de basculement que vous souhaitez que la machine virtuelle de réplication à utiliser en cas de basculement
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c16fbc95c9d679611d57327992a6621d58d4e201
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855750"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>Configurer les paramètres TCP/IP de basculement que vous souhaitez que la machine virtuelle de réplication à utiliser en cas de basculement

>S'applique à : Windows Server 2016
 
Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>Problème  
*Machines virtuelles de réplication configurés avec une adresse IP statique doit être configurés pour utiliser une autre adresse IP à partir de leur équivalent de l’ordinateur virtuel principal en cas de basculement.*  
  
## <a name="impact"></a>Impact  
*Les clients à l’aide de la charge de travail pris en charge par la machine virtuelle principale n’est peut-être pas en mesure de se connecter à l’ordinateur virtuel réplica après un basculement. En outre, adresse IP d’origine de l’ordinateur virtuel principal ne sera plus valide dans la topologie de réseau de machine virtuelle de réplica. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V pour configurer l’adresse IP que l’ordinateur virtuel réplica doit utiliser en cas de basculement.*  
  


