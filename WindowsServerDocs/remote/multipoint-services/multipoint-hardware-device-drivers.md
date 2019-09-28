---
title: Collecter les pilotes matériels et de périphériques nécessaires à l’installation
description: Informations sur les pilotes que vous devez installer pour MultiPoint services
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: cfbb8c8b68768c72b869df539c93f05e7e01d256
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394700"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>Collecter les pilotes matériels et de périphériques nécessaires à l’installation
Avant de commencer à déployer votre système MultiPoint services, vous aurez besoin des éléments suivants :  
  
-   **Composants matériels pour le serveur** : installez des cartes vidéo supplémentaires ou d’autres composants du système pour le moment.  
  
-   **Composants matériels pour les stations** : pour plus d’informations sur la planification de stations pour votre environnement, consultez [sélection de matériel pour votre système multipoint services](Selecting-Hardware-for-Your-MultiPoint-services-System.md).
-   **Les pilotes les plus récents pour vos cartes vidéo** : si le fabricant de votre appareil ou de votre OEM n’en a pas fourni, vous devrez les télécharger sur le site Web du fabricant de l’appareil.  
  
-   **Les derniers pilotes clients USB à zéro** : Si vous utilisez des stations clientes sans câble USB, vous devez installer les derniers pilotes client USB à zéro.  
  
    > [!IMPORTANT]  
    > Pour une installation de MultiPoint services, vous devez installer la version 64 bits de tous les pilotes.  
  
> [!TIP]  
> Si vous installez MultiPoint services sur un ordinateur sur lequel une autre version de Windows est déjà installée, vous devez déterminer la marque et le modèle de la carte vidéo dans Device Manager avant de démarrer l’installation de Windows Server et vous assurer que vous pouvez obtenir des pilotes qui sont disponible pour Windows Server 2016. Ouvrez Device Manager, ouvrez **gestion** de l’ordinateur à partir de l’écran d' **Accueil** . Puis, dans l’arborescence de la console, cliquez sur **Device Manager**.