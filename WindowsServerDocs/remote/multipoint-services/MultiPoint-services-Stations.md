---
title: Stations MultiPoint
description: En savoir plus sur les stations MultiPoint Services, y compris les différentes options pour les utilisateurs
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9f9d618-ccfe-41ea-a52c-00c3c7adb51a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: e747826a7cd84521bc62e48abedf3092bf6d844c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855650"
---
# <a name="multipoint--stations"></a>Stations multiPoint
Dans un environnement de système MultiPoint Services, *stations* sont les points de terminaison d’utilisateur pour la connexion à l’ordinateur qui exécute MultiPoint Services. Chaque station fournit à l’utilisateur avec une expérience de Windows 10 indépendante. Les types de station suivants sont pris en charge :  
  
-   Vidéo-connectés directement les stations  
  
-   Stations zéro-client-connectés par USB (y compris les clients over-Ethernet USB zéro)  
  
-   Stations de RDP-over-connectés au réseau local (pour un client riche ou des ordinateurs clients légers)  
  
PC complètes qui ont installé le connecteur MultiPoint peuvent aussi être surveillées et contrôlées à l’aide du tableau de bord MultiPoint. Sur Windows 10, le connecteur MultiPoint peut être activé via le panneau de configuration pour les fonctionnalités de Windows. 

Multipoint Services prend en charge n’importe quelle combinaison de ces types de station, mais il est recommandé qu’un poste une station vidéo-connectés directement, qui peut servir à la station principale. La raison de cette recommandation est de pouvoir anticiper les scénarios de prise en charge. Par exemple, pour interagir avec le système BIOS de l’avant de MultiPoint Services est en cours d’exécution.  
  
## <a name="primary-stations-and-standard-stations"></a>Stations service principales et les stations standards  
Une station connectée direct-vidéo est définie comme le *station principale*. Les stations restantes sont appelées *stations standards*.  
  
La station principale affiche les écrans de démarrage lorsque l’ordinateur est allumé. Il fournit un accès à la configuration du système et les informations de dépannage qui sont disponibles uniquement lors du démarrage. La station principale doit être une station vidéo-connectés directement. Après le démarrage, vous pouvez utiliser la station principale comme n’importe quelle autre station MultiPoint.  
  
## <a name="direct-video-connected-stations"></a>Vidéo-connectés directement les stations  
L’ordinateur qui exécute MultiPoint Services peut contenir plusieurs cartes vidéo, chacun d’eux peut avoir un ou plusieurs ports vidéo. Cela vous permet de connecter des moniteurs pour plusieurs stations directement sur l’ordinateur. Clavier et souris sont connectés via les concentrateurs USB qui sont associés à chaque analyse. Ces concentrateurs sont appelés *concentrateurs de station*. Autres périphériques, tels que des haut-parleurs, un casque ou périphériques de stockage USB peuvent également être connectés à un concentrateur de station, et ils sont disponibles uniquement pour l’utilisateur de cette station.  
  
> [!IMPORTANT]  
> Vous devez avoir au moins un *station vidéo-connectés directement* par le serveur qui agira en tant que la station principale pour afficher le processus de démarrage lorsque l’ordinateur est allumé.  
  
![Image d’une disposition du système MultiPoint Services USB](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**Figure 1** MultiPoint services système avec quatre stations connectées direct-vidéo  
  
### <a name="BKMK_PS2stations"></a>Stations PS/2  
Avec MultiPoint Services, vous pouvez mapper le clavier PS/2 et la souris sur la carte mère vers un moniteur connecté vidéo direct pour créer une station PS/2. Audio analogique haute définition sur la carte mère est le fichier audio associé à ce type de station. Cela ne s’applique pas aux ordinateurs où il n’existe aucun osselets PS/2 sur la carte mère.  
  
## <a name="usb-zero-client-connected-stations"></a>Stations USB-zéro-client-connecté  
Utilisent des stations de zéro-client-connectés par USB un *USB zéro client* comme un concentrateur de station. Les clients USB zéro sont parfois appelés un concentrateur multifonction avec la vidéo. Il s’agit d’un concentrateur qui se connecte à l’ordinateur via un câble USB, et ces hubs généralement prendre en charge qu’une vidéo moniteur, une souris et du clavier (PS/2 ou USB), audio et d’autres périphériques USB. Ce guide fait référence à ces hubs spécialisés en tant que USB zéro les clients.  
  
Le diagramme suivant illustre un système MultiPoint server avec une station principale (vidéo direct connecté station) et deux autres USB zéro client les stations connectées.  
  
![Stations de connectée USB zéro client](./media/WMS11_diagram7.gif)  
  
**Figure 2** système MultiPoint Services avec une station principale et les deux les stations USB connectées zéro client  
  
### <a name="usb-over-ethernet-zero-clients"></a>Clients over-Ethernet USB zéro  
Les clients over-Ethernet USB zéro sont une variante de clients USB zéro qui envoient USB sur réseau local pour le système MultiPoint Services. Ces types de clients USB zéro fonctionnement est similaire à d’autres USB zéro clients, mais ne sont pas limitées par les valeurs maximales de la longueur du câble USB. Les clients over-Ethernet USB zéro ne sont pas des clients légers traditionnels, et ils apparaissent comme des périphériques USB virtuels sur le système MultiPoint Services. Lorsque vous utilisez ces appareils, consultez le fabricant du périphérique pour des performances spécifiques et des recommandations de planification de site. La plupart des périphériques ont un plug-in de tiers pour le gestionnaire MultiPoint qui vous permet d’associer et de connecter des appareils au système MultiPoint Services.  
  
## <a name="rdp-over-lan-connected-stations"></a>Stations connectées au protocole RDP sur le réseau local  
Les clients légers et bureau traditionnel, les portables et tablet PC, peut se connecter à l’ordinateur exécutant MultiPoint Services via le réseau local (LAN) en utilisant le protocole RDP (Remote Desktop) ou un protocole propriétaire et le protocole Bureau à distance Fournisseur. Les connexions RDP fournissent une expérience de l’utilisateur final est très similaire à n’importe quelle autre station MultiPoint, mais utilise du matériel de l’ordinateur client local. En savoir plus sur nos applications de bureau à distance disponibles pour Android, iOS, Mac et Windows dans [clients Bureau à distance](../remote-desktop-services/clients/remote-desktop-clients.md). 
  
Les clients et les appareils qui exécutent Microsoft RemoteFX peuvent fournir une expérience multimédia riche en tirant parti des capacités matérielles du processeur et de la vidéo de l’ordinateur ou un client léger local pour fournir une vidéo haute définition sur le réseau.  
  
Si vous avez des clients du réseau local existants MultiPoint Services peut fournir un moyen rapide et économique passer simultanément tous vos utilisateurs à une expérience de Windows 10.  
  
Déploiement et l’administration du point de vue, les différences suivantes existent lors de l’utilisation de stations RDP-over-connectés au réseau local :  
  
-   N’est pas limitée aux distances de connexion USB physiques  
  
-   Potentiel de réutiliser le matériel informatique ancien comme les stations  
  
-   Plus facile à mettre à l’échelle sur un nombre plus élevé de stations. N’importe quel client sur votre réseau peut potentiellement être utilisé en tant que station distante  
  
-   Aucun matériel de résolution des problèmes via la console du gestionnaire MultiPoint  
  
-   Aucune fonctionnalité fractionné.  
  
    Pour plus d’informations, consultez [Stations à écran fractionné](#a-namebkmksplitscreenstationsasplit-screen-stations) plus loin dans cette rubrique  
  
-   Aucun changement de nom de station ou d’une configuration ouverture de session automatique via la console du gestionnaire MultiPoint  
  
![Station connectée USB zéro client](./media/Diagram1.gif)  
  
**Figure 3** système MultiPoint Services avec les stations RDP-over-connectés au réseau local  
  
## <a name="additional-configuration-options"></a>Options de configuration supplémentaires  
  
### <a name="BKMK_SplitscreenStations"></a>Stations à écran fractionné  
MultiPoint Services offre une option d’écran de fractionnement sur les ordinateurs avec les stations vidéo-connectés directement ou stations zéro-client-connectés par USB. Un écran de fractionnement offre la possibilité de créer une station supplémentaire par moniteur. Au lieu de demander deux moniteurs, vous pouvez utiliser un moniteur avec deux paramétrages de concentrateur de station pour créer deux stations avec un moniteur. Vous pouvez rapidement augmenter le nombre de stations disponibles sans acheter de moniteurs supplémentaires, des clients de zéro USB ou des cartes vidéo.  
  
Avantages de l’utilisation d’une station à écran partagé :  
  
-   Réduire les coûts et espace en proposant davantage d’utilisateurs sur un système MultiPoint Services.  
  
-   Autorise les deux utilisateurs de collaborer côte à côte sur un projet.  
  
-   Autoriser un enseignant illustrer une procédure sur une station tandis que le stagiaire suit la démonstration sur l’autre station.  
  
Tous les Services MultiPoint station moniteur dont la résolution de 1024 x 768 ou ultérieur peut être fractionnée en deux écrans de station. Pour une expérience utilisateur d’écran fractionné optimale, un écran large avec une résolution de 1600 x 900 minimale est recommandé. Un clavier mini sans un pavé numérique est également recommandé d’autoriser les deux claviers tenir devant l’analyse.  
  
Pour créer des stations à écran fractionné, vous configurez une station de vidéo-connectés directement, ou zéro-client-connectés par USB. Puis vous ajoutez un concentrateur de station supplémentaires en connectant un clavier et une souris à un concentrateur USB qui est connecté au serveur. Vous pouvez ensuite convertir la station en deux stations à l’aide du gestionnaire MultiPoint pour fractionner l’écran et de mapper le nouveau concentrateur à la moitié de l’analyse. La moitié gauche de l’écran devient un poste et la moitié droite devient une deuxième console.  
  
Après qu’une station divisée, un utilisateur peut se connecter à la station de gauche lors d’un autre utilisateur se connecte à la station de droite.  
  
![Stations à écran partagé](./media/WMS_diagram3.gif)  
  
**Figure 4** système MultiPoint Services avec les stations à écran fractionné  
  
## <a name="BKMK_StationTypeComparison"></a>Comparaison de type station  
  
||Vidéo direct connecté|Connectés par USB zéro Client|RDP sur le réseau local connecté|  
|-|--------------------------|-----------------------------|----------------------------|  
|Performances vidéo|Recommandé pour de meilleures performances vidéo||Utiliser des clients légers qui prennent en charge de RemoteFX pour améliorer la qualité vidéo à faible bande passante réseau|  
|Limitations physiques|Limité par la longueur du câble vidéo et concentrateur USB et la longueur du câble (longueur maximale compteur 15 recommandé)|Limité par le concentrateur USB et la longueur du câble (longueur maximale compteur 15 recommandé)|Limité par la distribution de réseau local|  
|Nombre de stations autorisées |Limité par le nombre d’emplacements PCIe disponibles sur la carte mère fois les ports vidéo par la carte vidéo|Nombre total peut être limité par le fabricant USB zéro client (pour plus d’informations, voir la Remarque qui suit ce tableau.)|Limité par les ports disponibles sur le commutateur de réseau|  
|Fractionné|Oui|Oui|Non|  
|État du périphérique station Gestionnaire multiPoint, configuration de l’ouverture de session automatique, changement de nom de station|Oui|Oui|Non|  
|Accès aux menus de démarrage du serveur|Oui|Non|Non|  
  
> [!NOTE]  
> Le nombre total de clients USB zéro qui sont connectés au serveur peut être limité par le fabricant ou de la capacité matérielle de l’ordinateur qui exécute MultiPoint Services.