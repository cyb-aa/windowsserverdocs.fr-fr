---
title: La réplication est suspendue pour une ou plusieurs machines virtuelles sur ce serveur
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 248b5fbdbfb54380e441d14cde6beaa9146ce800
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827770"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>La réplication est suspendue pour une ou plusieurs machines virtuelles sur ce serveur

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*La réplication est suspendue pour une ou plusieurs des ordinateurs virtuels. Lorsque la machine virtuelle principale est suspendue, toutes les modifications qui se produisent sont additionnées et seront envoyées à la machine virtuelle de réplication une fois que la reprise de la réplication.*  
  
## <a name="impact"></a>Impact  
*Tant que la réplication est suspendue, les modifications cumulées qui se produisent dans l’ordinateur virtuel principal consomme d’espace disque disponible sur le serveur principal. Une fois que la reprise de la réplication, il peut y avoir une grande rafale du trafic réseau vers le serveur de réplication. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Vérifiez que la réplication suspension a été conçue. Si la réplication a été suspendue à l’adresse disque faible espace ou connectivité réseau, reprendre la réplication dès que ces problèmes soient résolus.*  
  


