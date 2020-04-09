---
title: La compression est recommandée pour le trafic de réplication
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: aca66991eae57d702f38e2282eeb4253bc1cd244
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857662"
---
# <a name="compression-is-recommended-for-replication-traffic"></a>La compression est recommandée pour le trafic de réplication

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Le trafic de réplication envoyé sur le réseau à partir du serveur principal vers le serveur de réplication n’est pas compressé.*  
  
## <a name="impact"></a>Impact  
*Le trafic de réplication utilise plus de bande passante qu’il n’est nécessaire. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Configurez la réplication Hyper-V pour compresser les données transmises sur le réseau dans les paramètres de la machine virtuelle dans le Gestionnaire Hyper-V. Vous pouvez également utiliser des outils en dehors d’Hyper-V pour effectuer la compression.*  
  


