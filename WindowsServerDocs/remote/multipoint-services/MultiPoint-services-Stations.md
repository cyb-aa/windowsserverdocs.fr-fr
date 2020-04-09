---
title: Stations MultiPoint
description: En savoir plus sur les stations dans MultiPoint services, y compris les différentes options pour les utilisateurs
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: f9f9d618-ccfe-41ea-a52c-00c3c7adb51a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 3bcdd2d3f7492b29ecf92c59714f1d93b910c9b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853422"
---
# <a name="multipoint--stations"></a>Stations MultiPoint
Dans un environnement de système MultiPoint services, les *stations* sont les points de terminaison utilisateur pour la connexion à l’ordinateur qui exécute multipoint services. Chaque station fournit à l’utilisateur une expérience Windows 10 indépendante. Les types de stations suivants sont pris en charge :  
  
-   Stations directes connectées à la vidéo  
  
-   Stations USB-zéro-connectées au client (y compris les clients Zero USB-sur-Ethernet)  
  
-   Stations de connexion RDP-sur-LAN (pour les ordinateurs clients riches ou clients légers)  
  
Les PC complets sur lesquels le connecteur MultiPoint est installé peuvent également être surveillés et contrôlés à l’aide du tableau de bord MultiPoint. Sur Windows 10, le connecteur MultiPoint peut être activé par le biais du panneau de configuration pour les fonctionnalités Windows. 

Multipoint Services prend en charge n’importe quelle combinaison de ces types de stations, mais il est recommandé d’utiliser une station connectée directement à la vidéo, qui peut servir de station principale. La raison de cette recommandation est de pouvoir anticiper les scénarios de prise en charge. Par exemple, pour interagir avec le BIOS du système avant que MultiPoint services s’exécute.  
  
## <a name="primary-stations-and-standard-stations"></a>Stations principales et stations standard  
Une station connectée à la vidéo directe est définie en tant que *station principale*. Les autres stations sont appelées *stations standard*.  
  
La station principale affiche les écrans de démarrage lorsque l’ordinateur est sous tension. Il permet d’accéder à la configuration du système et aux informations de dépannage disponibles uniquement au démarrage. La station principale doit être une station connectée à la vidéo. Après le démarrage, vous pouvez utiliser la station principale comme n’importe quelle autre station MultiPoint.  
  
## <a name="direct-video-connected-stations"></a>Stations directes connectées à la vidéo  
L’ordinateur qui exécute MultiPoint services peut contenir plusieurs cartes vidéo, chacune pouvant avoir un ou plusieurs ports vidéo. Cela vous permet de brancher des moniteurs pour plusieurs stations directement sur l’ordinateur. Les claviers et les souris sont connectés via des concentrateurs USB associés à chaque analyse. Ces hubs sont appelés hubs de *station*. D’autres périphériques, tels que les haut-parleurs, les casques ou les périphériques de stockage USB, peuvent également être connectés à un concentrateur de station, et ne sont disponibles que pour l’utilisateur de cette station.  
  
> [!IMPORTANT]  
> Il doit y avoir au moins une *station connectée à la vidéo directe* par serveur pour servir de station principale pour afficher le processus de démarrage lorsque l’ordinateur est sous tension.  
  
![Image de la disposition du système USB MultiPoint services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**Figure 1** Système MultiPoint services avec quatre stations connectées directement à la vidéo  
  
### <a name="ps2-stations"></a><a name="BKMK_PS2stations"></a>Stations PS/2  
Avec MultiPoint services, vous pouvez mapper le clavier et la souris PS/2 sur la carte mère à un moniteur connecté directement à la vidéo pour créer une station PS/2. Les données audio analogiques haute définition sur la carte mère sont les données audio associées à ce type de station. Cela ne s’applique pas aux ordinateurs où il n’y a pas de prises Jacks PS/2 sur la carte mère.  
  
## <a name="usb-zero-client-connected-stations"></a>Stations USB-zéro-connectées au client  
Les stations USB-Zero-Connected client utilisent un *client USB Zero* comme concentrateur de station. Les clients USB à zéro sont parfois appelés concentrateurs multifonctions avec vidéo. Il s’agit d’un concentrateur qui se connecte à l’ordinateur via un câble USB, et ces hubs prennent en charge un moniteur vidéo, une souris et un clavier (PS/2 ou USB), de l’audio et d’autres périphériques USB. Ce guide fait référence à ces hubs spécialisés comme des clients USB à zéro.  
  
Le diagramme suivant illustre un système MultiPoint Server avec une station principale (station connectée à la vidéo directe) et deux stations USB supplémentaires connectées à zéro client.  
  
![Stations USB connectées à zéro client](./media/WMS11_diagram7.gif)  
  
**Figure 2** Système MultiPoint services avec une station principale et deux stations USB connectées à zéro client  
  
### <a name="usb-over-ethernet-zero-clients"></a>Clients USB-sur-Ethernet Zero  
Les clients USB-sur-Ethernet Zero sont une variante des clients USB à zéro qui envoient des connexions USB sur réseau local au système MultiPoint services. Ces types de clients USB ne fonctionnent pas de la même façon que les autres clients USB à zéro, mais ils ne sont pas limités par la longueur maximale des câbles USB. Les clients USB sur Ethernet Zero ne sont pas des clients légers traditionnels et apparaissent en tant qu’appareils USB virtuels sur le système MultiPoint services. Lorsque vous utilisez ces appareils, reportez-vous au fabricant de l’appareil pour obtenir des recommandations spécifiques sur les performances et la planification des sites. La plupart des appareils possèdent un plug-in tiers pour le gestionnaire MultiPoint qui vous permet d’associer et de connecter des appareils au système MultiPoint services.  
  
## <a name="rdp-over-lan-connected-stations"></a>Stations connectées à RDP-sur-LAN  
Les clients légers et les ordinateurs de bureau, ordinateurs portables ou Tablet PC traditionnels peuvent se connecter à l’ordinateur exécutant MultiPoint services via le réseau local (LAN) à l’aide d’protocole RDP (Remote Desktop Protocol) (RDP) ou d’un protocole propriétaire et du protocole RDP (Remote Desktop Protocol) Moteur. Les connexions RDP offrent une expérience d’utilisateur final très similaire à toute autre station MultiPoint, mais utilise le matériel de l’ordinateur client local. En savoir plus sur les applications de bureau à distance disponibles pour Android, iOS, Mac et Windows dans [les clients Bureau à distance](../remote-desktop-services/clients/remote-desktop-clients.md). 
  
Les clients et les appareils qui exécutent Microsoft RemoteFX peuvent fournir une expérience multimédia riche en tirant parti des fonctionnalités matérielles du processeur et de la vidéo du client ou de l’ordinateur local pour fournir une vidéo haute définition sur le réseau.  
  
Si vous disposez de clients LAN existants, MultiPoint services peut offrir un moyen rapide et économique de mettre à niveau simultanément tous vos utilisateurs vers une expérience Windows 10.  
  
Du point de vue du déploiement et de l’administration, les différences suivantes existent quand vous utilisez des stations connectées via le réseau local :  
  
-   Non limité aux distances physiques des connexions USB  
  
-   Possibilité de réutiliser l’ancien matériel informatique en tant que station  
  
-   Plus facile à mettre à l’échelle sur un plus grand nombre de stations. Tout client sur votre réseau peut potentiellement être utilisé en tant que station à distance  
  
-   Aucune résolution des problèmes matériels via la console du gestionnaire MultiPoint  
  
-   Aucune fonctionnalité d’écran fractionné.  
  
    Pour plus d’informations, consultez [stations à écran fractionné](#split-screen-stations) plus loin dans cette rubrique.  
  
-   Aucun changement de nom de station ou configuration de l’ouverture de session automatique via la console du gestionnaire MultiPoint  
  
![Station connectée USB zéro client](./media/Diagram1.gif)  
  
**Figure 3** Système MultiPoint services avec des stations connectées à RDP-sur-LAN  
  
## <a name="additional-configuration-options"></a>Options de configuration supplémentaires  
  
### <a name="split-screen-stations"></a>Stations à écran partagé  
MultiPoint services offre une option d’affichage partagé sur les ordinateurs équipés de stations connectées directement à la vidéo ou de stations USB-zéro-client. Un écran fractionné offre la possibilité de créer une station supplémentaire par moniteur. Au lieu de demander deux moniteurs, vous pouvez utiliser une analyse avec deux configurations de concentrateur de station pour créer deux stations avec une seule analyse. Vous pouvez augmenter rapidement le nombre de stations disponibles sans acheter des analyses supplémentaires, des clients USB-zéro ou des cartes vidéo.  
  
Les avantages de l’utilisation d’une station à écran partagé peuvent être les suivants :  
  
-   Réduction des coûts et de l’espace en prenant en charge davantage d’utilisateurs sur un système MultiPoint services.  
  
-   Permettre à deux utilisateurs de collaborer côte à côte sur un projet.  
  
-   Permettre à un enseignant d’illustrer une procédure sur une station, tandis qu’un étudiant suit sur l’autre poste.  
  
Tout moniteur de station MultiPoint services qui a une résolution de 1024 x 768 ou supérieure peut être divisé en deux écrans de station. Pour une expérience utilisateur de l’écran de fractionnement optimale, il est recommandé d’utiliser un écran étendu avec une résolution minimale de 1440 x 900. Il est également recommandé de disposer d’un mini-clavier sans pavé numérique pour permettre l’ajustement des deux claviers devant l’écran.  
  
Pour créer des stations à écran fractionné, vous configurez une station connectée à un client ou une connexion directe. Ensuite, vous ajoutez un concentrateur de station supplémentaire en branchant un clavier et une souris à un concentrateur USB qui est connecté au serveur. Vous pouvez ensuite convertir la station en deux stations à l’aide du gestionnaire MultiPoint pour fractionner l’écran et mapper le nouveau concentrateur à la moitié du moniteur. La moitié gauche de l’écran devient une station et la moitié droite devient une deuxième station.  
  
Après le fractionnement d’une station, un utilisateur peut se connecter à la station de gauche et un autre utilisateur se connecte à la station de droite.  
  
![Stations à écran partagé](./media/WMS_diagram3.gif)  
  
**Figure 4** Système MultiPoint services avec des stations d’écran fractionnées  
  
## <a name="station-type-comparison"></a><a name="BKMK_StationTypeComparison"></a>Comparaison du type de station  
  
||Vidéo directe connectée|Client USB à connexion nulle|Connexion RDP-sur-LAN|  
|-|--------------------------|-----------------------------|----------------------------|  
|Performances vidéo|Recommandé pour des performances vidéo optimales||Utilisez des clients légers qui prennent en charge RemoteFX pour améliorer la qualité vidéo à une bande passante réseau inférieure|  
|Limitations physiques|Limité par longueur de câble vidéo et concentrateur USB et longueur de câble (longueur maximale recommandée de 15 mètres)|Limité par concentrateur USB et longueur de câble (15 mètres de longueur maximum recommandés)|Limité par la distribution LAN|  
|Nombre de stations autorisées |Limité par le nombre d’emplacements PCIe disponibles sur la carte mère multiplié par la carte vidéo|Le nombre total peut être limité par le fabricant du client Zero USB (pour plus d’informations, consultez la remarque qui suit ce tableau).|Limité par les ports disponibles sur le commutateur réseau|  
|Fractionner l’écran|Oui|Oui|Non|  
|État du périphérique de la station MultiPoint Manager, configuration de l’ouverture de session automatique, changement de nom de la station|Oui|Oui|Non|  
|Accès aux menus de démarrage du serveur|Oui|Non|Non|  
  
> [!NOTE]  
> Le nombre total de clients USB à connexion nulle qui sont connectés au serveur peut être limité par le fabricant ou par la capacité matérielle de l’ordinateur qui exécute MultiPoint services.
