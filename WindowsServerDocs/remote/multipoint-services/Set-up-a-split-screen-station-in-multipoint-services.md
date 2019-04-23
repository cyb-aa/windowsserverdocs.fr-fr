---
title: Configurer une station à écran partagé
description: Décrit comment configurer MultiPoint Services de sorte que deux utilisateurs peuvent partager un seul système
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d1d434-79b2-4e0a-835f-d94ed8ee6b21
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b2d01df6175eee99fd1374cd5af270cbcc764ca8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849940"
---
# <a name="set-up-a-split-screen-station"></a>Configurer une station à écran partagé
Vous pouvez configurer une station à écran partagé afin de deux utilisateurs peuvent utiliser simultanément le système.

Tout moniteur présente une résolution minimale 1200 x 720, lorsque connecté à une station qui prend en charge la fonctionnalité fractionné, peut être divisé en deux stations. Une fois une station divisée, le bureau que le moniteur a affiché se déplace vers la moitié gauche de l’écran, et une nouvelle station est affichée dans la partie droite de l’écran. Pour terminer la création de la nouvelle station, vous devez mapper un clavier, souris et concentrateur USB à la station. Une fois une station divisée, un utilisateur peut se connecter à la station de gauche tandis qu’un autre utilisateur se connecte à la station de droite.  
  
Stations à écran fractionné présentent plusieurs avantages :  
  
-   Vous pouvez réduire le coût et l’espace en proposant davantage d’utilisateurs sur un système MultiPoint Services.  
  
-   Deux utilisateurs peuvent collaborer ensemble, côte à côte sur un projet.  
  
-   Un utilisateur de tableau de bord MultiPoint permettre expliquer une procédure sur une station tandis que le stagiaire suit la démonstration sur l’autre station.  
  
L’illustration suivante montre un système MultiPoint Services avec une station d’écran de fractionnement (à droite).  
  
![Stations à écran partagé](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>Configuration requise pour une station à écran fractionné  
Pour créer une station à écran fractionné, le moniteur et une station doivent remplir ces conditions :  
  
-   Le moniteur doit avoir une résolution de 1200 x720 ou une version ultérieure.  
  
-   Si vous utilisez un client over-Ethernet USB zéro, vérifiez avec votre fournisseur de matériel pour déterminer si les stations à écran fractionné sont pris en charge. Nombre d’appareils client-over-Ethernet USB zéro présentent des limitations qui empêchent leur configuration en tant que stations à écran fractionné.  
  
## <a name="setting-up-a-split-screen-station"></a>Configuration d’une station à écran fractionné  
Utilisez les procédures suivantes pour ajouter un deuxième hub pour une station à écran fractionné et puis diviser la station dans MultiPoint Services. La dernière procédure explique comment retourner une station à écran partagé à une console unique.  
  
> [!NOTE]  
> Quand vous divisez une station, la session active sur la station est suspendue. L’utilisateur doit se connecter à la station pour reprendre le travail après que la division se produit.  
  
**Pour ajouter un deuxième hub avec clavier et souris :**  
  
1.  Connectez un concentrateur USB à un port USB ouvert sur l’ordinateur, comme illustré dans l’illustration suivante.  
  
    ![Image de serveur MultiPoint connexion par concentrateur USB](./media/WMSUSBHubConnection.gif)  
  
2.  Connecter un clavier et une souris au concentrateur USB.  
  
    ![Image de connexion de périphériques d’entrée de concentrateur USB](./media/WMSUSBDeviceConnection.gif)  
  
3.  Connecter les périphériques supplémentaires, par exemple un casque au concentrateur USB.  
  
4.  Si vous utilisez un concentrateur alimenté de façon externe, connecter le câble d’alimentation du concentrateur pour une prise de courant.  
  
**Pour diviser une station :**  
  
1.  Dans le gestionnaire MultiPoint, cliquez sur le **Stations** onglet.  
  
2.  Sous **Station**, cliquez sur le nom de la station que vous souhaitez fractionner.  
  
3.  Sous **sélectionné des tâches d’élément**, cliquez sur **diviser la station**.  
  
    L’écran d’origine se déplace vers la moitié gauche de l’analyse et écran d’un de la nouvelle station est créé dans la partie droite du moniteur.  
  
4.  Créer la nouvelle station en appuyant sur la lettre spécifiée sur le clavier qui vient d’être ajouté comme indiqué lorsque le **créer une Station de serveur MultiPoint** écran s’affiche dans la partie droite de l’analyse.  
  
Après qu’une station divisée, un utilisateur peut se connecter à la station de gauche lors d’un autre utilisateur se connecte à la station de droite.  
  
**Pour retourner une station divisée à une station unique :**  
  
1.  Dans le gestionnaire MultiPoint, cliquez sur le **Stations** onglet.  
  
2.  Sous **Station**, cliquez sur le nom de la station que vous souhaitez non scindée.  
  
3.  Sous **sélectionné des tâches d’élément**, cliquez sur **annuler le fractionnement de station**.