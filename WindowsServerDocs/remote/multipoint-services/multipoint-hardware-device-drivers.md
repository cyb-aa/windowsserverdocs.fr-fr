---
title: Collecter les pilotes matériels et de périphériques nécessaires à l’installation
description: Informations sur les pilotes que vous devez installer pour MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cf5fdbe-b871-4360-b003-d65ac43b491e
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: a9d902e2599cdcd69e156d1fabec87a067b1d8ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833420"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>Collecter les pilotes matériels et de périphériques nécessaires à l’installation
Avant de commencer à déployer votre système MultiPoint Services, vous devez :  
  
-   **Composants matériels du serveur** -installer toutes les cartes vidéo supplémentaires ou autres composants système pour l’instant.  
  
-   **Composants matériels pour les stations** : pour plus d’informations sur les stations de planification pour votre environnement, consultez [en sélectionnant le matériel pour votre système MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md).
-   **Les pilotes les plus récents pour vos cartes vidéo** -si votre fabricant OEM ou un périphérique n’a pas fourni ces, vous devez les télécharger à partir du site Web du fabricant du périphérique.  
  
-   **Les derniers pilotes du client USB zéro** -si vous utilisez les stations USB zéro client, vous devez installer les derniers pilotes du client USB zéro.  
  
    > [!IMPORTANT]  
    > Pour une installation de MultiPoint Services, vous devez installer la version 64 bits de tous les pilotes.  
  
> [!TIP]  
> Si vous installez MultiPoint Services sur un ordinateur avec une autre version de Windows déjà installé, vous devez déterminer la marque de la carte vidéo et le modèle dans le Gestionnaire de périphériques avant de démarrer l’installation de Windows Server et assurez-vous de que vous pouvez obtenir les pilotes qui sont disponible pour Windows Server 2016. Ouvrez le Gestionnaire de périphériques, ouvrez **gestion de l’ordinateur** à partir de la **Démarrer** écran. Puis, dans l’arborescence de la console, cliquez sur **Device Manager**.