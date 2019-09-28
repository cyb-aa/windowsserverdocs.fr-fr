---
title: Changer de mode
description: Découvrez comment basculer entre les modes station et console dans MultiPoint services
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f1b2324-c1b0-4b61-ab51-39af15e7792a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 181324bff3227f7b071b8ae32f756d365b059a74
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389241"
---
# <a name="switch-between-modes"></a>Changer de mode
Le gestionnaire MultiPoint comprend les modes suivants pour vous aider à exécuter différents types de gestion du système MultiPoint services :  
  
-   *Mode station*: Par défaut, le système MultiPoint services démarre en mode station. En mode Station, les stations de services MultiPoint se comportent comme si chacune d’elle est un ordinateur distinct exécutant Windows, et plusieurs utilisateurs peuvent utiliser le système en même temps. Vous et vos utilisateurs peuvent partager des fichiers et réaliser les tâches à effectuer.  
  
-   *Mode de console*: Lorsque le système MultiPoint services est en mode console, vous pouvez installer et mettre à jour les logiciels et pilotes, ou effectuer d’autres tâches de maintenance. Lorsque le système est en mode Console, aucune *station* n’est disponible pour une utilisation par d’autres utilisateurs de l’ordinateur. Ces stations ne sont pas affichées dans le gestionnaire MultiPoint. Toutes les analyses directement connectées au serveur sont traitées comme des affichages de ce système informatique.   
  
> [!NOTE]
> Vous pouvez mettre en œuvre et démarrer le système en mode Console en modifiant la valeur par défaut dans les paramètres du serveur.  
> ## <a name="to-switch-from-station-mode-to-console-mode"></a>Pour basculer du mode Station en mode Console  
  
1.  Ouvrez le gestionnaire MultiPoint en mode station, puis cliquez sur l’onglet dossier de **démarrage** .  
  
2.  Dans la colonne **Ordinateur**, cliquez sur l’ordinateur pour lequel vous souhaitez changer de mode.  
  
3.  Sous **tâches**nom d’ordinateur, cliquez sur **basculer en mode console**. L’ordinateur redémarre et toutes les stations sont indisponibles.  
  
## <a name="to-switch-from-console-mode-to-station-mode"></a>Pour basculer du mode Console en mode Station  
  
1.  Ouvrez le gestionnaire MultiPoint en mode console, puis cliquez sur l’onglet dossier de **démarrage** .  
  
2.  Dans la colonne **Ordinateur**, cliquez sur l’ordinateur pour lequel vous souhaitez changer de mode.  
  
3.  Sous **tâches**nom d’ordinateur, cliquez sur **basculer en mode station**. L’ordinateur redémarre et toutes les stations sont disponibles.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer des tâches système à l’aide du Gestionnaire MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)