---
title: Configuration d’une station vidéo-connectés directement dans MultiPoint Services
description: Découvrez comment créer une station vidéo-connectés directement dans MultiPoint Services
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
ms.openlocfilehash: 58197164c91ab6b69b0ef331c025287f593f94c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850740"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>Configuration d’une station vidéo-connectés directement dans MultiPoint Services
Sur une station connectée à la vidéo directe, le moniteur n’est connecté directement à un port vidéo sur l’ordinateur MultiPoint Server. Un clavier et une souris sont alors connectés à un concentrateur USB et sont associés à l’analyse.  
  
L’illustration suivante montre un environnement MultiPoint Server qui a un seul ordinateur MultiPoint Server et quatre stations vidéo-connectés directement. Pour plus d’informations, consultez [MultiPoint Server Stations](MultiPoint-services-Stations.md).  
  
**Système multiPoint Services avec quatre connexions vidéo directes**  
  
![Image d’une disposition du système MultiPoint Services USB](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> Pour configurer une station vidéo-connectés directement, vous devez utiliser un clavier Latin (par exemple, un clavier de langue anglais ou espagnol).  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>Pour configurer une station connectée à la vidéo directe  
  
1.  Connectez le câble du moniteur au port vidéo sur l’ordinateur, comme illustré ci-après.  
  
    ![Image d’une connexion vidéo sur un système à concentrateur USB](./media/WMSVideoConnection.gif) 
  
2.  Branchez le cordon d’alimentation du moniteur vidéo sur une prise.  
  
3.  Connectez un concentrateur USB à un port USB ouvert sur l’ordinateur, comme illustré ci-après.  
  
    ![Image de connexion par concentrateur USB de Services MultiPoint](./media/WMSUSBHubConnection.gif)  
  
4.  Connecter un clavier et une souris au concentrateur de station USB.  
  
    ![Image de connexion de périphériques d’entrée de concentrateur USB](./media/WMSUSBDeviceConnection.gif)  
  
5.  Connecter les périphériques supplémentaires, par exemple un casque, au concentrateur USB.  
  
6.  Si vous utilisez un concentrateur alimenté de façon externe, connecter le câble d’alimentation du concentrateur pour une prise de courant.  
  
    > [!IMPORTANT]  
    > Nous recommandons l’utilisation d’un concentrateur alimenté. Comportement anormal du système peut être dû à des conditions d’incomplète actuel.  
    >   
    > Les utilisateurs ne doivent pas joindre souris et claviers directement aux ports USB de l’ordinateur. Cette opération est susceptible de causer l’association incorrecte de plusieurs des claviers et des souris à la même station ou à aucune station du tout.  
  
7.  Suivez les instructions qui s’affichent sur le moniteur pour créer la station.  
  
Si vous ajoutez plus d’une station de vidéo connectée directe à votre environnement MultiPoint Services, la station principale peut changer. Vous pouvez découvrir facilement lequel diriger vidéo station connectée est votre station principale.  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>Pour savoir lequel diriger station connectée à la vidéo est la station principale  
  
1.  Activer toutes les analyses qui sont connectés directement à des cartes graphiques de l’ordinateur (cartes vidéo).  
  
2.  Démarrer (ou redémarrer) l’ordinateur MultiPoint Services et voir quel moniteur affiche les écrans de démarrage. Cette station est la station principale.  
  
    > [!NOTE]  
    > Dans certains cas, les informations de démarrage du BIOS s’affichent simultanément sur plusieurs moniteurs. Dans ce cas, toutes les analyses peuvent être considéré comme la « station principale » pour les besoins de l’accès au BIOS.