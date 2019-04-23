---
title: La resynchronisation de la réplication doit être planifiée pendant les heures creuses
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2d6c18b7e37c5d17f56f41c7ff03ed8796457de0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840020"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>La resynchronisation de la réplication doit être planifiée pendant les heures creuses

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
*La resynchronisation de la réplication pour les machines virtuelles principales n’est pas planifiée pendant les heures creuses.*  
  
## <a name="impact"></a>Impact  
*Est plus une machine virtuelle dans un état nécessitant une resynchronisation, plus les fichiers journaux de réplication s’accroît, et les modifications non plus répliquées se produisent sur les machines virtuelles principales. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V pour modifier les paramètres de réplication pour la machine virtuelle pour effectuer la resynchronisation automatiquement pendant les heures creuses.*  
  


