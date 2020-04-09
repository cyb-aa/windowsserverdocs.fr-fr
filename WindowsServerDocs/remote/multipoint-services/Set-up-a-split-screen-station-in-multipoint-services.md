---
title: Configurer une station à écran partagé
description: Décrit comment configurer MultiPoint services pour que deux utilisateurs puissent partager un même système
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 35d1d434-79b2-4e0a-835f-d94ed8ee6b21
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 006422614b87cd331dd157218ac910b6f7af1602
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853912"
---
# <a name="set-up-a-split-screen-station"></a>Configurer une station à écran partagé
Vous pouvez configurer une station à écran partagé pour que deux utilisateurs puissent utiliser simultanément le système.

Toute analyse qui a une résolution de 1200 x720 minimum, lorsqu’elle est connectée à une station qui prend en charge la fonctionnalité de fractionnement, peut être divisée en deux stations. Après le fractionnement d’une station, le bureau affiché par le moniteur se déplace vers la moitié gauche de l’écran, et une nouvelle station s’affiche à droite de la moitié de l’écran. Pour terminer la création de la station, vous devez mapper un clavier, une souris et un concentrateur USB sur la station. Une fois une station divisée, un utilisateur peut se connecter à la station de gauche tandis qu’un autre utilisateur se connecte à la station de droite.  
  
Les stations à écran partagé présentent plusieurs avantages :  
  
-   Vous pouvez réduire les coûts et l’espace en prenant en charge davantage d’utilisateurs sur un système MultiPoint services.  
  
-   Deux utilisateurs peuvent collaborer ensemble, côte à côte, sur un projet.  
  
-   Un utilisateur du tableau de bord MultiPoint peut démontrer une procédure sur une station tandis qu’un étudiant suit l’autre station.  
  
L’illustration suivante montre un système MultiPoint services avec une station à écran partagé (à droite).  
  
![Stations à écran partagé](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>Configuration requise pour une station à écran partagé  
Pour créer une station à écran partagé, le moniteur et la station doivent remplir les conditions suivantes :  
  
-   Le moniteur doit avoir une résolution de 1200 x720 ou supérieure.  
  
-   Si vous utilisez un client Zero USB sur Ethernet, contactez votre fournisseur de matériel pour savoir si les stations à écran fractionné sont prises en charge. De nombreux périphériques clients USB sur Ethernet zéro présentent des limites qui empêchent leur configuration en tant que stations à écran fractionné.  
  
## <a name="setting-up-a-split-screen-station"></a>Configuration d’une station à écran partagé  
Utilisez les procédures suivantes pour ajouter un second Hub pour une station à écran partagé, puis Fractionnez la station dans MultiPoint services. La dernière procédure explique comment retourner une station à écran partagé sur une seule station.  
  
> [!NOTE]  
> Quand vous divisez une station, la session active sur la station est suspendue. L’utilisateur doit se reconnecter à la station pour reprendre son travail une fois le fractionnement effectué.  
  
**Pour ajouter un second Hub à l’aide du clavier et de la souris :**  
  
1.  Connectez un concentrateur USB à un port USB ouvert sur l’ordinateur, comme indiqué dans l’illustration suivante.  
  
    ![Image de la connexion du concentrateur USB MultiPoint Server](./media/WMSUSBHubConnection.gif)  
  
2.  Connectez un clavier et une souris au concentrateur USB.  
  
    ![Image de connexion de périphériques d’entrée de concentrateur USB](./media/WMSUSBDeviceConnection.gif)  
  
3.  Connectez tous les autres périphériques, tels que les écouteurs, au concentrateur USB.  
  
4.  Si vous utilisez un concentrateur alimenté en externe, connectez le câble d’alimentation du concentrateur à une prise d’alimentation.  
  
**Pour diviser une station :**  
  
1.  Dans le gestionnaire MultiPoint, cliquez sur l’onglet **stations** .  
  
2.  Sous **station**, cliquez sur le nom de la station que vous souhaitez fractionner.  
  
3.  Sous **tâches de l’élément sélectionné**, cliquez sur **partager la station**.  
  
    L’écran d’origine passe à la moitié gauche du moniteur et l’écran d’une nouvelle station est créé sur la moitié droite du même moniteur.  
  
4.  Créez la nouvelle station en appuyant sur la lettre spécifiée sur le clavier que vous venez d’ajouter, comme indiqué lorsque l’écran **créer une station multipoint Server** s’affiche à droite de la moitié du moniteur.  
  
Après le fractionnement d’une station, un utilisateur peut se connecter à la station de gauche et un autre utilisateur se connecte à la station de droite.  
  
**Pour replacer une station partagée sur une seule station :**  
  
1.  Dans le gestionnaire MultiPoint, cliquez sur l’onglet **stations** .  
  
2.  Sous **station**, cliquez sur le nom de la station que vous souhaitez défractionner.  
  
3.  Sous **tâches de l’élément sélectionné**, cliquez sur **déscinder la station**.