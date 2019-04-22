---
title: Configurer l’ordinateur physique et la station principale
description: Découvrez comment configurer votre premier système, la station principale, dans MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e83b126-ce9a-4cd7-a0bd-6627c9e0f81b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6569c4963d31b72216943bf29b71411e702caf84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812010"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>Configurer l’ordinateur physique et la station principale
Avant d’installer les Services MultiPoint, vous devez configurer la station principale pour votre système MultiPoint Services. Si vous utilisez un réseau local (LAN) connecter l’ordinateur au réseau local.  
  
Un *station* est un point de terminaison par lequel MultiPoint Services est accessible. Le *station principale* est la première station pour démarrer au démarrage de MultiPoint Services. Les administrateurs peuvent utiliser cela pour accéder aux menus de démarrage et les paramètres. La station principale fournit l’accès à la configuration système et les informations de dépannage qui sont uniquement disponibles lors du démarrage et avant le MultiPoint Services système est en cours d’exécution. Après le démarrage, vous pouvez utiliser la station principale comme vous le feriez pour n’importe quelle autre station.  
  
La station principale doit être une station vidéo-connectés directement. La procédure suivante décrit comment connecter le matériel nécessaire à votre ordinateur MultiPoint Services.  
  
Pour plus d’informations sur les stations, consultez [Stations MultiPoint](multipoint-services-stations.md). Pour faciliter vos sélections de matériel, consultez [en sélectionnant le matériel pour votre système MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md). Pour plus d’informations sur la connexion d’autres stations types à MultiPoint Services, consultez [attacher des stations supplémentaires à votre ordinateur MultiPoint Services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
> [!NOTE]  
> Pour créer une station connectée à la vidéo, vous devez utiliser un clavier Latin (par exemple, un clavier anglais ou espagnol).  
  
## <a name="to-set-up-your-primary-station"></a>Pour configurer votre station principale  
  
1.  Assurez-vous que l’ordinateur qui exécute MultiPoint Services est mis hors tension et débranché.  
  
2.  Connecter le cordon d’alimentation de l’analyse à une prise de courant et connectez le câble du moniteur au port vidéo sur l’ordinateur, comme illustré ci-après.  
  
    ![Image d’une connexion vidéo sur un système à concentrateur USB](./media/WMSVideoConnection.gif)  
  
3.  Si la station utilisera un clavier et souris USB, procédez comme suit :  
  
    1.  Connectez un concentrateur USB externe à un port USB ouvert sur l’ordinateur, comme illustré ci-après.  
  
        ![Image de connexion par concentrateur USB de Services MultiPoint](./media/WMSUSBHubConnection.gif)  
  
    2.  Connecter le clavier et souris USB au concentrateur USB.  
  
        ![Image de connexion de périphériques d’entrée de concentrateur USB](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > Si votre ordinateur MultiPoint Services ports PS/2, vous pouvez, si nécessaire, utilisez un clavier PS/2 et une souris connecté directement à l’ordinateur. Toutefois, cette configuration présente des limitations importantes. Les utilisateurs ne peuvent pas utiliser les périphériques audio, les Webcams et un périphérique flash sur des stations de PS/2.  
  
    3.  Si vous utilisez un concentrateur alimenté de façon externe, connecter le câble d’alimentation du concentrateur pour une prise de courant.  
  
        > [!IMPORTANT]  
        > Nous recommandons l’utilisation d’un concentrateur alimenté. Comportement anormal du système peut être dû à des conditions d’incomplète actuel.  
        >   
        > Les utilisateurs ne doivent pas joindre souris et claviers directement aux ports USB de l’ordinateur. Cette opération est susceptible de causer l’association incorrecte de plusieurs des claviers et des souris à la même station ou à aucune station du tout.  
  
        > [!NOTE]  
        > Le périphérique audio hôte sur la carte mère du système est uniquement disponible quand MultiPoint Services est en mode console. Pour garantir l’audio sans interruption pour une station qui utilise un concentrateur USB externe, vous devez utiliser un périphérique audio USB connecté au concentrateur.  
  
## <a name="to-connect-the-computer-to-the-lan"></a>Pour connecter l’ordinateur au réseau local  
  
-   Si vous avez un réseau local, connecter votre ordinateur à votre réseau avec un câble réseau.