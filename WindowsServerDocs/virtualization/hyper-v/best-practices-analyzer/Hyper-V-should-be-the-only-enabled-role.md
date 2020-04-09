---
title: Hyper-V doit être le seul rôle activé
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 40dae4baad782d3be4fc6ca8ba2bc4e506131dbf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861912"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>Hyper-V doit être le seul rôle activé

>S’applique à Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Les rôles autres que Hyper-V sont activés sur ce serveur.*  
  
Dans la plupart des cas, il n’est pas recommandé d’installer d’autres rôles sur un serveur exécutant le rôle Hyper-V. Serveur hôte de virtualisation des services Bureau à distance service de rôle est une exception, car il fait partie du rôle Services Bureau à distance et nécessite l’installation d’Hyper-V sur le même serveur.  
  
## <a name="impact"></a>Impact  
  
*Le rôle Hyper-V doit être le seul rôle activé sur un serveur.*  
  
Cette meilleure pratique permet de conserver le système d’exploitation hôte sans les rôles, les fonctionnalités et les applications qui ne sont pas nécessaires pour exécuter Hyper-V. Conformément à cette meilleure pratique et à l’exécution d’Hyper-V sur nano Server, vous pouvez réduire le nombre de mises à jour dont vous avez besoin, car seuls nano Server, les composants du service Hyper-V et l’hyperviseur Windows sont soumis aux mises à jour logicielles.  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez Gestionnaire de serveur pour supprimer tous les rôles à l’exception d’Hyper-V.*  
  
Gestionnaire de serveur comprend l’Assistant Suppression de rôles. Cet Assistant vous permet de supprimer plusieurs rôles à la fois. Avant de supprimer des rôles, l’Assistant Suppression de rôles vérifie la présence de dépendances afin de réduire le risque de suppression de logiciels basés sur d’autres rôles. Si des dépendances sont trouvées, l’Assistant vous invite à approuver la suppression d’autres rôles, services de rôle ou logiciels requis par les rôles installés.   
  
Pour utiliser Gestionnaire de serveur, vous devez être connecté à l’ordinateur en tant qu’administrateur.  
  
#### <a name="to-remove-a-role"></a>Pour supprimer un rôle  
  
1.  Ouvrez Gestionnaire de serveur à l’aide de raccourcis dans le menu **Démarrer** , dans la barre des tâches Windows ou dans outils d’administration.  
2.   Dans la zone **Résumé des rôles** de la fenêtre principale de gestionnaire de serveur, cliquez sur supprimer des **rôles**. Suivez les instructions de l’Assistant pour supprimer le rôle.   
  
  
  


