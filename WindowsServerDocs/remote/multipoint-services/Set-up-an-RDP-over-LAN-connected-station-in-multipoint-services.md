---
title: Configuration d’une station de RDP sur le réseau local connecté dans MultiPoint Services
description: Découvrez comment configurer un système RDP sur le réseau local dans MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e1a025-c2fb-4708-a3ff-c44c223a3224
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 8d2f1644918f1a581c1bcab181cd084e12c6b576
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843700"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>Configuration d’une station de RDP sur le réseau local connecté dans MultiPoint Services
Une station de RDP sur le réseau local connecté est un client léger, un bureau traditionnel ou un ordinateur portable qui se connecte à MultiPoint Services sur un réseau local (LAN) en utilisant le protocole RDP (Remote Desktop). Pour plus d’informations sur cela et d’autres types de station, voir [Stations MultiPoint](MultiPoint-services-Stations.md).  
  
## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>Pour configurer une station MultiPoint à l’aide d’un ordinateur ou un client léger sur un réseau local  
  
1.  Allumez l’ordinateur qui exécute MultiPoint Services.  
  
2.  Assurez-vous que l’ordinateur MultiPoint Server est connecté au réseau local par un commutateur, routeur ou un autre périphérique réseau et qu’il a une adresse IP appropriée. (Une adresse IP qui commence par 169.254 (une adresse APIPA) peut indiquer un problème avec la connexion de réseau local ou que le serveur DHCP n’est pas accessible ou ne fonctionne pas correctement.)  
  
3.  Connecter l’ordinateur client ou un client léger au réseau local.  
  
4.  Allumez l’ordinateur client ou un client léger.  
  
5.  Sur l’ordinateur client ou un client léger, démarrez connexion Bureau à distance ou une application équivalente et entrez le nom ou l’adresse IP de l’ordinateur qui exécute MultiPoint Services.

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>Configurer un appareil Windows 10 pour la gestion à distance à l’aide des Services de connecteur
N’importe quel PC ou un ordinateur portable exécutant Windows 10 peut être géré à distance tant que :
- les Services de connecteur ont été activées.  
- l’ordinateur a été ajoutée sur les ordinateurs gérés sur le serveur MultiPoint.  

Sur un ordinateur exécutant Windows 10, procédez comme suit pour activer le connecteur MultiPoint :

1. Dans la zone de recherche, tapez « Windows d’activer ou désactiver des fonctionnalités » et sélectionnez le résultat de recherche correcte. 

2. Dans la liste des fonctionnalités activer **connecteur MultiPoint**. Cela active les services de connecteur MultiPoint qui sont nécessaires pour gérer l’appareil. 

Sur le serveur MultiPoint :
1. Ouvrez le gestionnaire MultiPoint et sélectionnez **ajouter ou supprimer des ordinateurs personnels** ou **Add ou remove MultiPoint Services**.

2. Sélectionnez les ordinateurs distants que vous souhaitez gérer et cliquez sur **OK**.  Vous êtes invité pour les informations d’identification d’administrateur sur les ordinateurs distants.  Après cela, vous verrez les ordinateurs distants sous l’onglet Accueil du gestionnaire MultiPoint.

Lorsque set installer le Gestionnaire de tableau de bord peut surveiller correctement les utilisateurs qui travaillent sur l’appareil géré.

> [!IMPORTANT]  
> Lors de la surveillance gérés Windows 10 périphériques administratrive les utilisateurs ne peuvent être analysées à l’exception du serveur de paramètres ont été modifiés en conséquence. Consultez [modifier les paramètres du serveur](Edit-Server-Settings.md)