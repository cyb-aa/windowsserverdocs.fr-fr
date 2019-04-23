---
title: Hyper-V doit être le seul rôle est activé
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bd03554396696a43b4821aff0f4ed893933484c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886460"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>Hyper-V doit être le seul rôle est activé

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Rôles autres que Hyper-V sont activés sur ce serveur.*  
  
Dans la plupart des cas, il n’est pas judicieux d’installer d’autres rôles sur un serveur exécutant le rôle Hyper-V. Service de rôle hôte de virtualisation Bureau à distance est une exception, car il fait partie du rôle Services Bureau à distance et requiert Hyper-V doit être installé sur le même serveur.  
  
## <a name="impact"></a>Impact  
  
*Le rôle Hyper-V doit être le seul rôle activé sur un serveur.*  
  
Cette meilleure pratique permet de conserver le système d’exploitation hôte libre de rôles, les fonctionnalités et les applications qui ne sont pas nécessaires pour exécuter Hyper-V. L’application de cette meilleure pratique et exécutant Hyper-V sur Nano Server permet de réduire le nombre de mises à jour que vous avez besoin, car seuls Nano Server, les composants de service Hyper-V et l’hyperviseur Windows seraient susceptibles d’être mises à jour logicielles.  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le Gestionnaire de serveur pour supprimer tous les rôles à l’exception de Hyper-V.*  
  
Le Gestionnaire de serveur inclut l’Assistant Suppression de rôles. Cet Assistant vous permet de supprimer plusieurs rôles à la fois. Avant de supprimer des rôles, l’Assistant Suppression de rôles vérifie les dépendances pour réduire le risque de suppression du logiciel qui dépendent des autres rôles. Si la dépendance n’est trouvée, l’Assistant vous invite à approuver la suppression d’autres rôles, les services de rôle ou les logiciels requis par les rôles installés.   
  
Pour utiliser le Gestionnaire de serveur, vous devez être connecté à l’ordinateur en tant qu’administrateur.  
  
#### <a name="to-remove-a-role"></a>Pour supprimer un rôle  
  
1.  Ouvrez le Gestionnaire de serveur à l’aide des raccourcis sur le **Démarrer** menu, dans la barre des tâches Windows, ou dans les outils d’administration.  
2.   Dans le **résumé des rôles** zone de la fenêtre principale du Gestionnaire de serveur, cliquez sur **supprimer des rôles**. Suivez les instructions dans l’Assistant pour supprimer le rôle.   
  
  
  


