---
title: La compression est recommandée pour le trafic de réplication
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 802f11355bec8903e7f6ab81bf337d38e64513f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833510"
---
# <a name="compression-is-recommended-for-replication-traffic"></a>La compression est recommandée pour le trafic de réplication

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
*Le trafic de réplication envoyé sur le réseau à partir du serveur principal vers le serveur de réplication n’est pas compressé.*  
  
## <a name="impact"></a>Impact  
*Le trafic de réplication utilise davantage de bande passante que nécessaire. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Configurer la réplication Hyper-V pour compresser les données transmises sur le réseau dans les paramètres de la machine virtuelle dans le Gestionnaire Hyper-V. Vous pouvez également utiliser les outils en dehors de Hyper-V pour effectuer une compression.*  
  


