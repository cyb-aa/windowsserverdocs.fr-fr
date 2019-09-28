---
title: Mettre à jour et installer des pilotes de périphériques si nécessaire
description: Découvrez comment vérifier et mettre à jour les pilotes de périphérique dans MultiPoint services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 766e2175a16cd20a68730870c8980ed9c9204a3c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394875"
---
# <a name="update-and-install-device-drivers-if-needed"></a>Mettre à jour et installer des pilotes de périphériques si nécessaire
Si vous utilisez des clients ou des périphériques USB nuls qui requièrent des pilotes, vous devez installer les pilotes pour l’instant. Il est également judicieux de vérifier **Device Manager** pour toutes les alertes de pilote et d’installer des pilotes pour ces appareils.  
  
En règle générale, les pilotes les plus récents sont requis pour les types d’appareils suivants :  
  
-   Clients USB à zéro  
  
-   Clients USB-sur-Ethernet Zero  
  
-   Contrôleurs de disque  
  
-   Cartes réseau  
  
-   Contrôleurs audio  
  
-   Contrôleurs hôtes USB

-   Cartes graphiques


## <a name="to-check-for-driver-alerts-in-device-manager"></a>Pour rechercher les alertes de pilote dans Device Manager  
  
1.  Ouvrez l’écran d’accueil.  
  
2.  Tapez **gestion**de l’ordinateur, puis cliquez sur gestion de l' **ordinateur** dans les résultats.  
  
3.  Dans l’arborescence de la console Gestion de l’ordinateur, cliquez sur **Device Manager**.  
  
4.  Dans les périphériques système sur la droite, recherchez les alertes de pilote qui peuvent affecter MultiPoint Server.  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>Pour installer des pilotes de périphérique dans le gestionnaire MultiPoint  
  
1.  Pour ouvrir le gestionnaire MultiPoint, recherchez « gestionnaire MultiPoint », puis cliquez sur **Gestionnaire multipoint** dans les résultats.  
  
2.  Dans le gestionnaire MultiPoint, cliquez sur l’onglet dossier de **démarrage** , puis cliquez sur **basculer en mode console**.  
  
3.  Pour installer un pilote de périphérique, double-cliquez sur le fichier de pilote et suivez les instructions pour installer le pilote.  
  
4.  Répétez l’étape précédente pour installer tous les pilotes requis.  
  
    > [!NOTE]  
    > Si une installation requiert un redémarrage de l’ordinateur, vous devrez revenir en mode console avant d’installer le pilote suivant. MultiPoint Server démarre toujours en mode station. Pour basculer en mode console, accédez à l’onglet dossier de **démarrage** dans le gestionnaire multipoint, puis cliquez sur **basculer en mode console**.