---
title: Installer une station
description: Découvrez comment configurer un une station MultiPoint services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dce05b6c-795e-43b2-9920-026550b873c5
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 2bba32f27ae01052a693d78f152d4487a04bd9bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880680"
---
# <a name="set-up-a-station"></a>Installer une station
Une *station* MultiPoint Services est généralement composée d’un *concentrateur de station*, d’une souris, d’un clavier et d’un moniteur vidéo. Cette rubrique décrit la connexion des périphériques matériels au concentrateur de station pour créer une station MultiPoint Services.  
  
Le concentrateur de station est un périphérique matériel qui permet de connecter des périphériques à un ordinateur dans un système MultiPoint Services. MultiPoint Services prend en charge deux types de concentrateurs de station :  
  
-   **Concentrateur USB :** Un concentrateur USB multiport générique d’extension qui est conforme aux spécifications USB 2.0 ou version ultérieure. Ces concentrateurs disposent généralement de deux ou quatre ports USB, ou plus, qui permettent de connecter plusieurs périphériques USB à un seul port USB sur l’ordinateur. Concentrateurs USB sont généralement des périphériques qui peuvent être de façon externe ou alimenté par bus. Les concentrateurs utilisés comme concentrateurs de station avec MultiPoint Services devraient posséder au moins quatre ports.  
  
    > [!IMPORTANT]  
    > Si vous envisagez de connecter des périphériques USB autres qu’un clavier et une souris au concentrateur, nous vous conseillons d’utiliser un concentrateur alimenté de façon externe pour de meilleures performances.  
  
-   **Concentrateur multifonction :** Concentrateur d’extension connecté à l’ordinateur via un port USB et qui permet la connexion d’une variété de périphériques non-USB au concentrateur, notamment un moniteur vidéo. Concentrateurs multifonctions sont produits par les fabricants de matériel spécifique et peuvent nécessiter l’installation d’un pilote spécifique à l’appareil.  
  
Pour ajouter une station au système MultiPoint Services, commencez par vérifier que les ports de connexion sont en nombre suffisant pour le matériel de la station que vous souhaitez utiliser. En outre, vous devez sécuriser le nombre approprié de *licences d’accès client (CAL)* pour votre système MultiPoint Services.  
  
## <a name="setting-up-station-hardware"></a>Installation du matériel de la station  
Les procédures de cette section décrivent la connexion du matériel de la station MultiPoint Services aux différents types de concentrateurs de station.  
  
### <a name="to-set-up-a-station-with-a-usb-hub"></a>Pour installer une station avec un concentrateur USB  
  
1.  Avant de vous connecter à une nouvelle station, *terminez* toutes les *sessions* utilisateur et arrêtez l’ordinateur ainsi que les autres périphériques sous tension du système MultiPoint Services.  
  
2.  Connectez le câble du nouveau moniteur vidéo au port vidéo de l’ordinateur, comme illustré ci-après :  
  
    ![Image d’une connexion vidéo sur un système à concentrateur USB](./media/WMSVideoConnection.gif)  
  
3.  Connectez le nouveau concentrateur USB à un port USB ouvert de l’ordinateur :  
  
    ![Image d’une connexion par concentrateur USB MultiPoint Server](./media/WMSUSBHubConnection.gif)  
  
4.  Connectez un clavier et une souris au concentrateur USB :  
  
    ![Image de connexion de périphériques d’entrée de concentrateur USB](./media/WMSUSBDeviceConnection.gif)  
  
5.  Branchez le cordon d’alimentation du moniteur vidéo sur une prise.  
  
6.  Mettez l’ordinateur sous tension.  
  
7.  MultiPoint Services démarre. Suivez les instructions qui s’affichent sur le moniteur vidéo de la nouvelle station pour associer les périphériques à cette dernière.  
  
### <a name="to-set-up-a-station-with-a-multifunction-hub"></a>Pour installer une station avec un concentrateur multifonction  
  
1.  Avant de vous connecter à une nouvelle station, terminez toutes les sessions utilisateur et arrêtez l’ordinateur ainsi que les autres périphériques sous tension du système MultiPoint Services.  
  
2.  Connectez le câble du nouveau moniteur vidéo au port vidéo DVI ou VGA du concentrateur multifonction, comme illustré ci-après :  
  
    ![Image d’un concentrateur multifonction et d’une connexion vidéo](./media/WMSMultifunctionHubVideoConnection.gif)  
  
3.  Connectez le nouveau concentrateur multifunction à un port USB ouvert de l’ordinateur :  
  
    ![Image d’une connexion par concentrateur multifonction](./media/WMSMultifunctionHubConnection.gif)  
  
4.  Connectez un clavier et une souris aux connecteurs PS2 ou USB du concentrateur multifonction :  
  
    ![Image d’un concentrateur multifonction et de connecteurs PS2](./media/WMSMultifunctionHubPS2Connection.gif)  
  
5.  Branchez le cordon d’alimentation du moniteur vidéo sur une prise.  
  
6.  Mettez l’ordinateur sous tension.  
  
7.  MultiPoint Services démarre. Si vous y êtes invité, suivez les instructions qui s’affichent sur le moniteur vidéo de la nouvelle station pour *associer* les périphériques à cette dernière.  
  
## <a name="see-also"></a>Voir aussi  
[Terminer une Session utilisateur](End-a-User-Session.md)  
[Redémarrer ou arrêter](Restart-or-Shut-Down.md)  
[Gérer le matériel de la Station](Manage-Station-Hardware.md)  
[Utiliser des périphériques USB](Work-with-USB-Devices.md)