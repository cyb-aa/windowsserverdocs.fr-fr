---
title: Disques durs virtuels avec les fichiers d’échange doivent être exclus de la réplication
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8b6289a82c83f3dcfc0de299250ce19ee3782678
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850120"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Disques durs virtuels avec les fichiers d’échange doivent être exclus de la réplication

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Informations|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Fichiers d’échange doivent être exclus de la réplication, mais aucun disque n’ont été exclus.*  
  
## <a name="impact"></a>Impact  
*Fichiers de pagination rencontrer un volume élevé d’activité d’entrée/sortie, qui requièrent inutilement des ressources bien plus importantes à participer à la réplication. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Si vous ne le n'avez pas déjà fait, créez un disque dur virtuel distinct pour le fichier d’échange Windows. Si la réplication initiale a déjà été effectuée, utilisez le Gestionnaire Hyper-V pour supprimer la réplication. Ensuite, les configurer à nouveau la réplication et exclure le disque dur virtuel avec le fichier d’échange de la réplication.*  
  


