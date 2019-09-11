---
title: Configurer une station à connexion vidéo directe dans MultiPoint services
description: Découvrez comment créer une station connectée à la vidéo directe dans MultiPoint services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82ba3517-9743-4cde-8eea-63a17edb016f
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: eda8d5eee0635370873adec5b1fde2d65fc9fd9c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871604"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>Configurer une station à connexion vidéo directe dans MultiPoint services
Sur une station connectée à une vidéo directe, le moniteur est connecté directement à un port vidéo sur l’ordinateur MultiPoint Server. Un clavier et une souris sont ensuite connectés à un concentrateur USB et sont associés à l’analyse.  
  
L’illustration suivante montre un environnement de serveur MultiPoint qui possède un seul ordinateur MultiPoint Server et quatre stations connectées directement à la vidéo. Pour plus d’informations, consultez la page sur les [stations de serveur multipoint](MultiPoint-services-Stations.md).  
  
**Système MultiPoint services avec quatre connexions vidéo directes**  
  
![Image de la disposition du système USB MultiPoint services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> Pour configurer une station connectée à une vidéo directe, vous devez utiliser un clavier latin (par exemple, un clavier en anglais ou en espagnol).  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>Pour configurer une station connectée à une vidéo directe  
  
1.  Connectez le câble du moniteur au port d’affichage vidéo de l’ordinateur, comme indiqué ci-dessous.  
  
    ![Image d’une connexion vidéo sur un système à concentrateur USB](./media/WMSVideoConnection.gif) 
  
2.  Branchez le cordon d’alimentation du moniteur vidéo sur une prise.  
  
3.  Connectez un concentrateur USB à un port USB ouvert sur l’ordinateur, comme indiqué ci-dessous.  
  
    ![Image de la connexion du concentrateur USB MultiPoint services](./media/WMSUSBHubConnection.gif)  
  
4.  Connectez un clavier et une souris au concentrateur de station USB.  
  
    ![Image de connexion de périphériques d’entrée de concentrateur USB](./media/WMSUSBDeviceConnection.gif)  
  
5.  Connectez les autres périphériques, tels que les casques, au concentrateur USB.  
  
6.  Si vous utilisez un concentrateur alimenté en externe, connectez le câble d’alimentation du concentrateur à une prise d’alimentation.  
  
    > [!IMPORTANT]  
    > Nous vous recommandons fortement d’utiliser un concentrateur alimenté. Un comportement anormal du système peut être dû à des conditions en cours.  
    >   
    > Les utilisateurs ne doivent pas attacher de souris et de clavier directement aux ports USB de l’ordinateur. Cela risque d’entraîner une association incorrecte de plusieurs claviers et souris à la même station ou à aucune station du tout.  
  
7.  Suivez les instructions qui s’affichent sur l’écran pour créer la station.  
  
Si vous ajoutez plusieurs stations directement connectées à votre environnement MultiPoint services, la station principale peut changer. Vous pouvez facilement déterminer quelle station connectée à la vidéo directe est votre station principale.  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>Pour savoir quelle station connectée à la vidéo directe est la station principale  
  
1.  Activez toutes les analyses qui sont connectées directement aux cartes d’affichage de l’ordinateur (cartes vidéo).  
  
2.  Démarrez ou redémarrez l’ordinateur MultiPoint services et consultez le moniteur qui affiche les écrans de démarrage. Cette station est la station principale.  
  
    > [!NOTE]  
    > Dans certains cas, les informations de démarrage du BIOS s’affichent simultanément sur plusieurs analyses. Dans ce cas, l’une des analyses peut être considérée comme la « station principale » dans le but d’accéder au BIOS.