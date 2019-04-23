---
title: Attacher des stations supplémentaires à votre serveur MultiPoint
description: Ajouter plusieurs stations à votre déploiement de MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d78ebf4e-0968-4014-9a42-9f75cc50cb52
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 57fc8ed6774c3266298ecd98e8f609ec01f63ef6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889260"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>Attacher des stations supplémentaires à MultiPoint Services
Dans votre environnement MultiPoint Services, vos utilisateurs utilisent des stations pour vous connecter à MultiPoint Services et effectuer leur travail. Les stations sont les points de terminaison d’utilisateur pour la connexion à l’ordinateur qui exécute Multipoint Services.  
  
MultiPoint services prend en charge trois types de station :  
  
-   Vidéo-connectés directement les stations  
  
-   USB zéro client connecté stations (et USB sur les stations de client connecté Ethernet zéro)  
  
-   Stations connectées au protocole RDP sur le réseau local  
  
Les classifications sont basées sur le matériel d’une station et le type de connexion qu’il utilise. Vous pouvez combiner des types de connexion pour vos stations. La seule exigence est que la station principale (que vous avez installé précédemment) doit être une station vidéo-connectés directement. Pour plus d’informations sur les configurations de station, voir [Stations MultiPoint](MultiPoint-services-Stations.md).  
  
Pour obtenir des instructions qui expliquent comment configurer chaque type de station, consultez les rubriques suivantes :  
  
-   [Configuration d’une station connectée direct-vidéo](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)  
  
-   [Configurer une clé USB zéro station client connecté](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)  
  
-   [Configuration d’une station de RDP sur le réseau local connecté](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)  
  
Pour une comparaison détaillée des types de station, voir [comparaison de type Station](multipoint-services-stations.md#BKMK_StationTypeComparison).  
  
> [!NOTE]  
> -   Les procédures permettant d’attacher des stations ne décrivent pas de la configuration intermédiaires hubs ou concentrateurs en aval. Pour plus d’informations sur l’emplacement où installer ces concentrateurs, consultez [Stations MultiPoint](MultiPoint-services-Stations.md).  
> -   Dans certains cas, vous devrez peut-être créer des bureaux virtuels, qui s’exécutent sur des machines virtuelles de station. Par exemple, vous utilisez les applications qui ne peut pas être installées sur Windows Server ou les applications qui s’exécuteront pas plusieurs instances sur le même ordinateur hôte. Pour plus d’informations, consultez [bureaux virtuels de créer de Windows 10 entreprise pour les stations](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md).  
  
> [!TIP]  
> Il est utile créer vos stations dans l’ordre de leurs emplacements physiques afin qu’ils sont identifiés de manière séquentielle dans MultiPoint Server. Si vous souhaitez ultérieurement modifier le nom d’une station, vous pouvez le faire dans le gestionnaire MultiPoint. Pour plus d’informations, consultez le remappage de toutes les stations dans MultiPoint Server aide et Support.