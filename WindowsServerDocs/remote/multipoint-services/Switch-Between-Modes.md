---
title: Changer de mode
description: Découvrez comment basculer entre le mode station et console dans MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a9c00d65ad1c59e91f4011ab932bf9f921957c59
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446148"
---
# <a name="switch-between-modes"></a>Changer de mode
Le gestionnaire multiPoint inclut les modes suivants pour vous aider à effectuer différents types de gestion du système MultiPoint Services :  
  
-   *Mode station*: Par défaut, le système MultiPoint Services démarre en mode station. En mode Station, les stations de services MultiPoint se comportent comme si chacune d’elle est un ordinateur distinct exécutant Windows, et plusieurs utilisateurs peuvent utiliser le système en même temps. Vous et vos utilisateurs peuvent partager des fichiers et réaliser les tâches à effectuer.  
  
-   *Mode console*: Lorsque le système MultiPoint Services est en mode console, vous pouvez installer et mettre à jour des logiciels et des pilotes ou effectuer d’autres tâches de maintenance. Lorsque le système est en mode Console, aucune *station* n’est disponible pour une utilisation par d’autres utilisateurs de l’ordinateur. Ces stations ne sont pas affichées dans le gestionnaire MultiPoint. Tous les moniteurs connectés directement au serveur sont traités comme affichages de cet ordinateur.   
  
> [!NOTE]
> Vous pouvez mettre en œuvre et démarrer le système en mode Console en modifiant la valeur par défaut dans les paramètres du serveur.  
> ## <a name="to-switch-from-station-mode-to-console-mode"></a>Pour basculer du mode Station en mode Console  
  
1.  Ouvrez le gestionnaire MultiPoint en mode station, puis cliquez sur le **accueil** onglet.  
  
2.  Dans la colonne **Ordinateur**, cliquez sur l’ordinateur pour lequel vous souhaitez changer de mode.  
  
3.  Sous *nom de l’ordinateur* **tâches**, cliquez sur **passer en mode console**. L’ordinateur redémarre et toutes les stations sont indisponibles.  
  
## <a name="to-switch-from-console-mode-to-station-mode"></a>Pour basculer du mode Console en mode Station  
  
1.  Ouvrez le gestionnaire MultiPoint en mode console, puis cliquez sur le **accueil** onglet.  
  
2.  Dans la colonne **Ordinateur**, cliquez sur l’ordinateur pour lequel vous souhaitez changer de mode.  
  
3.  Sous *nom de l’ordinateur* **tâches**, cliquez sur **passer en mode station**. L’ordinateur redémarre et toutes les stations sont disponibles.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer des tâches système à l’aide du Gestionnaire MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)