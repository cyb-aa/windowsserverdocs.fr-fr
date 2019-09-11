---
title: Configurer l’ordinateur physique et la station principale
description: Découvrez comment configurer votre premier système, la station principale, dans MultiPoint services
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
ms.openlocfilehash: 01b405b679afa3815652b91bec63c786bd661cf7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871553"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>Configurer l’ordinateur physique et la station principale
Avant d’installer MultiPoint services, vous devez configurer la station principale pour votre système MultiPoint services. Si vous utilisez un réseau local (LAN), connectez l’ordinateur au réseau local.  
  
Une *station* est un point de terminaison par lequel les services multipoint sont accédés. La *station principale* est la première station à démarrer au démarrage de multipoint services. Les administrateurs peuvent l’utiliser pour accéder aux menus et aux paramètres de démarrage. La station principale fournit un accès à la configuration du système et des informations de dépannage qui ne sont disponibles qu’au démarrage et avant l’exécution du système MultiPoint services. Après le démarrage, vous pouvez utiliser la station principale comme n’importe quelle autre station.  
  
La station principale doit être une station connectée à la vidéo. La procédure suivante décrit comment connecter le matériel nécessaire à votre ordinateur MultiPoint services.  
  
Pour plus d’informations sur les stations, consultez [multipoint stations](multipoint-services-stations.md). Pour obtenir de l’aide pour effectuer des sélections matérielles, consultez [sélection du matériel pour votre système multipoint services](Selecting-Hardware-for-Your-MultiPoint-services-System.md). Pour plus d’informations sur la connexion d’autres types de stations à MultiPoint services, consultez [attacher des stations supplémentaires à votre ordinateur multipoint services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
> [!NOTE]  
> Pour créer une station connectée à la vidéo, vous devez utiliser un clavier latin (par exemple, un clavier en anglais ou en espagnol).  
  
## <a name="to-set-up-your-primary-station"></a>Pour configurer votre station principale  
  
1.  Assurez-vous que l’ordinateur qui exécute MultiPoint services est désactivé et débranché.  
  
2.  Connectez le cordon d’alimentation du moniteur à une prise d’alimentation et connectez le câble du moniteur au port d’affichage vidéo de l’ordinateur, comme indiqué ci-dessous.  
  
    ![Image d’une connexion vidéo sur un système à concentrateur USB](./media/WMSVideoConnection.gif)  
  
3.  Si la station utilise un clavier et une souris USB, procédez comme suit :  
  
    1.  Connectez un concentrateur USB externe à un port USB ouvert sur l’ordinateur, comme indiqué ci-dessous.  
  
        ![Image de la connexion du concentrateur USB MultiPoint services](./media/WMSUSBHubConnection.gif)  
  
    2.  Connectez le clavier et la souris USB au concentrateur USB.  
  
        ![Image de connexion de périphériques d’entrée de concentrateur USB](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > Si votre ordinateur MultiPoint services possède des ports PS/2, vous pouvez, si nécessaire, utiliser un clavier et une souris PS/2 branchés directement sur l’ordinateur. Toutefois, cette configuration présente des limitations importantes. Les utilisateurs ne peuvent pas utiliser les périphériques audio, les cames Web et les lecteurs flash sur les stations PS/2.  
  
    3.  Si vous utilisez un concentrateur alimenté en externe, connectez le câble d’alimentation du concentrateur à une prise d’alimentation.  
  
        > [!IMPORTANT]  
        > Nous vous recommandons fortement d’utiliser un concentrateur alimenté. Un comportement anormal du système peut être dû à des conditions en cours.  
        >   
        > Les utilisateurs ne doivent pas attacher de souris et de clavier directement aux ports USB de l’ordinateur. Cela risque d’entraîner une association incorrecte de plusieurs claviers et souris à la même station ou à aucune station du tout.  
  
        > [!NOTE]  
        > Le périphérique audio hôte sur la carte mère du système est disponible uniquement lorsque MultiPoint services est en mode console. Pour garantir l’audio ininterrompue pour une station qui utilise un concentrateur USB externe, vous devez utiliser un périphérique audio USB branché dans le concentrateur.  
  
## <a name="to-connect-the-computer-to-the-lan"></a>Pour connecter l’ordinateur au réseau local  
  
-   Si vous disposez d’un réseau local, connectez votre ordinateur à votre réseau avec un câble réseau.