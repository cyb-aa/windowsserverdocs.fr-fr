---
title: Mettre à jour et installer des pilotes de périphériques si nécessaire
description: Découvrez comment vérifier et mettre à jour des pilotes de périphérique dans MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 66477634e06df217656876b084ae37be8cb311c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829130"
---
# <a name="update-and-install-device-drivers-if-needed"></a>Mettre à jour et installer des pilotes de périphériques si nécessaire
Si vous utilisez des clients USB zéro ou des périphériques qui nécessitent des pilotes, vous devez installer les pilotes pour l’instant. Il est judicieux également vérifier **Device Manager** aux alertes de pilote et installer les pilotes pour ces appareils.  
  
En règle générale, les pilotes les plus récents sont requises pour les types d’appareils suivants :  
  
-   Clients USB zéro  
  
-   Clients over-Ethernet USB zéro  
  
-   Contrôleurs de disque  
  
-   Cartes réseau  
  
-   Contrôleurs audio  
  
-   Contrôleurs hôtes USB

-   Cartes graphiques


## <a name="to-check-for-driver-alerts-in-device-manager"></a>Pour vérifier les alertes de pilote dans le Gestionnaire de périphériques  
  
1.  Ouvrez l’écran d’accueil.  
  
2.  Type **gestion de l’ordinateur**, puis cliquez sur **gestion de l’ordinateur** dans les résultats.  
  
3.  Dans l’arborescence de la console Gestion de l’ordinateur, cliquez sur **Device Manager**.  
  
4.  Dans les périphériques système sur la droite, recherchez les alertes de pilote qui pourraient affecter MultiPoint Server.  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>Pour installer des pilotes de périphérique dans le gestionnaire MultiPoint  
  
1.  Pour ouvrir le gestionnaire MultiPoint, recherchez « Gestionnaire MultiPoint », puis cliquez sur **Gestionnaire MultiPoint** dans les résultats.  
  
2.  Dans le gestionnaire MultiPoint, cliquez sur le **accueil** onglet, puis cliquez sur **passer en mode console**.  
  
3.  Pour installer un pilote de périphérique, double-cliquez sur le fichier de pilote et suivez les instructions pour installer le pilote.  
  
4.  Répétez l’étape précédente pour installer tous les pilotes requis.  
  
    > [!NOTE]  
    > Si une installation requiert un redémarrage de l’ordinateur, vous devrez passer au mode de console avant d’installer le pilote suivant. Serveur multiPoint démarre toujours en mode station. Pour passer en mode console atteindre le **accueil** onglet dans le gestionnaire MultiPoint et cliquez sur **passer en mode console**.