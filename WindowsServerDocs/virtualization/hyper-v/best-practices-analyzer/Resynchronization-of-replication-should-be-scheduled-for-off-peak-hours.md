---
title: La resynchronisation de la réplication doit être planifiée pendant les heures creuses
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ae630f93ef50ebc977de2bffcefa3b27b03622ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861792"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>La resynchronisation de la réplication doit être planifiée pendant les heures creuses

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*La resynchronisation de la réplication pour les machines virtuelles principales n’est pas planifiée pendant les heures creuses.*  
  
## <a name="impact"></a>Impact  
*Plus un ordinateur virtuel est dans un État nécessitant une resynchronisation, plus les fichiers journaux de réplication augmentent et plus les modifications non répliquées se produisent sur les machines virtuelles principales. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez le Gestionnaire Hyper-V pour modifier les paramètres de réplication de la machine virtuelle afin d’effectuer la resynchronisation automatiquement pendant les heures creuses.*  
  


