---
title: Attacher des stations supplémentaires à votre serveur MultiPoint
description: Ajouter d’autres stations à votre déploiement MultiPoint services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: d78ebf4e-0968-4014-9a42-9f75cc50cb52
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: f82cfb982d36e9d66ff5f951f65030f2f1d77539
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858732"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>Attacher des stations supplémentaires à MultiPoint services
Dans votre environnement MultiPoint services, vos utilisateurs utilisent des stations pour se connecter à MultiPoint services et effectuer leur travail. Les stations sont les points de terminaison utilisateur pour la connexion à l’ordinateur qui exécute multipoint services.  
  
MultiPoint Services prend en charge trois types de station :  
  
-   Stations directes connectées à la vidéo  
  
-   Stations USB connectées à un client (et stations connectées à zéro client USB sur Ethernet)  
  
-   Stations connectées à RDP-sur-LAN  
  
Les classifications sont basées sur le matériel d’une station et le type de connexion qu’elle utilise. Vous pouvez mélanger et faire correspondre les types de connexion pour vos stations. La seule exigence est que la station principale (que vous avez installée précédemment) doive être une station connectée directement à la vidéo. Pour plus d’informations sur les configurations de station, consultez [multipoint stations](MultiPoint-services-Stations.md).  
  
Pour obtenir des instructions qui expliquent comment configurer chaque type de station, consultez les rubriques suivantes :  
  
-   [Configurer une station connectée directement au port vidéo](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)  
  
-   [Configurer une station connectée à un client zéro USB](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)  
  
-   [Configurer une station connectée au réseau LAN via RDP](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)  
  
Pour une comparaison détaillée des types de stations, consultez Comparaison des types de [stations](multipoint-services-stations.md#BKMK_StationTypeComparison).  
  
> [!NOTE]  
> -   Les procédures d’attachement de stations ne décrivent pas comment configurer des concentrateurs intermédiaires ou des hubs en aval. Pour plus d’informations sur l’emplacement d’installation de ces hubs, consultez [multipoint stations](MultiPoint-services-Stations.md).  
> -   Dans certains cas, vous devrez peut-être créer des bureaux virtuels de station, qui s’exécutent sur des machines virtuelles. Par exemple, vous utilisez des applications qui ne peuvent pas être installées sur Windows Server ou des applications qui n’exécuteront pas plusieurs instances sur le même ordinateur hôte. Pour plus d’informations, consultez [créer des bureaux virtuels Windows 10 entreprise pour les stations](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md).  
  
> [!TIP]  
> Il est utile de créer vos stations dans l’ordre de leur emplacement physique afin qu’elles soient identifiées de façon séquentielle dans MultiPoint Server. Si vous souhaitez modifier le nom d’une station ultérieurement, vous pouvez le faire dans le gestionnaire MultiPoint. Pour plus d’informations, consultez remapper toutes les stations dans le centre d’aide et de support MultiPoint Server.