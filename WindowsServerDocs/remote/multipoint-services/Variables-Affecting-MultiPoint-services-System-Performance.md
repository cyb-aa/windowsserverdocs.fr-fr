---
title: Variables qui affectent les performances du système MultiPoint Services
description: Informations sur les performances de MultiPoint services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f3e8875-1b5e-4789-b16c-d06d6e31f38e
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: cba973e3b0a89c26f886a67154c27831adb2c8cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394830"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>Variables qui affectent les performances du système MultiPoint Services
Il existe de nombreuses variables qui peuvent affecter les performances globales de votre système MultiPoint services. Vous souhaiterez peut-être les prendre en compte lors de la conception de votre système.  
  
## <a name="usage"></a>Utilisation  
  
-   **Applications** Le type et le nombre d’applications qui s’exécutent en même temps\-, en particulier les applications graphiques lourdes ou gourmandes en mémoire, affectent les performances globales de votre système. Pour plus d’informations, consultez [applications et contenu Internet](hardware-and-performance-recommendations.md#applications-and-internet-content).  
  
-   **Utilisation d’Internet** Déterminez si vos utilisateurs visualisent du contenu multimédia ou des pages Web qui utilisent des vidéos en mode plein mouvement. Ce type de contenu peut surcharger le système si un trop grand nombre d’utilisateurs sont affichés simultanément.  
  
    > [!NOTE]  
    > La fonctionnalité de projection dans MultiPoint services, qui permet aux enseignants de projeter leurs écrans sur leurs moniteurs d’étudiant, n’est pas conçue pour projeter une vidéo en plein mouvement. La fonctionnalité de projection est conçue à des fins de démonstration, telles que l’émission d’une procédure.  
  
-   **Périphériques à haut débit** Si un trop grand nombre d’utilisateurs utilisent simultanément un appareil à haut débit, tel qu’un lecteur de DVD ou de caméra Web, cela affecte les performances globales du système.  
  
## <a name="configuration"></a>Configuration  
  
-   **Processeur, GPU et RAM** Pour obtenir des recommandations en matière de processeur, de GPU et de RAM, consultez [optimiser les performances du système multipoint services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance) dans ce guide.  
-   **Bande passante réseau** Pour les stations connectées au réseau local, la bande passante réseau et la capacité du client (par exemple, un client léger, un ordinateur de bureau ou un ordinateur portable) sont importantes, en particulier si la vidéo est en cours d’exécution dans la session de l’utilisateur. Si vous utilisez des clients de zéro USB sur Ethernet, la bande passante réseau doit également être prise en compte. Les données vidéo de tous les appareils sont envoyées via la même connexion Ethernet. vous pouvez donc envisager de configurer un réseau Gigabit Ethernet distinct lors de l’utilisation de ces appareils.  
-   **RemoteFX** Pour les stations connectées à RDP-sur-LAN, vous pouvez utiliser RemoteFX pour améliorer de façon considérable la distribution de contenu multimédia haute définition.  
-   **Résolution d’affichage** Si vous utilisez beaucoup de vidéo en plein écran, vous souhaiterez peut-être réduire la résolution de l’analyse pour optimiser les performances.  
-   **Nombre de clients USB à zéro** Le nombre total de clients USB à zéro sur un concentrateur racine unique sur le serveur aura une incidence directe sur les performances de la vidéo. Pour plus d’informations, consultez [disposition pour les stations connectées à un client USB sans client](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations). Le nombre de stations clientes Zero USB prises en charge peut être légèrement inférieur au nombre de clients USB à zéro.  
-   **Bande passante USB** Tenez compte de la bande passante USB lors de la conception de votre système.  Ceci est particulièrement important pour les clients USB à zéro, qui envoient des données vidéo via la connexion USB. Pour optimiser la bande passante, réduisez le nombre d’appareils connectés à un port USB unique sur le serveur. Cela s’applique aux stations connectés en chaîne et aux concentrateurs intermédiaires. Pour plus d’informations, consultez [hubs de station](MultiPoint-services-Site-Planning.md#station-hubs) et [concentrateurs intermédiaires](MultiPoint-services-Site-Planning.md#intermediate-hubs).  
  
-   **Type USB** L’utilisation de USB 3,0 au lieu de USB 2,0 augmente la bande passante disponible entre le serveur et le concentrateur intermédiaire si vous connectez plus de trois clients USB à zéro au concentrateur ou si vous utilisez des périphériques USB à bande passante élevée.  
  
-   **Stations** de Le nombre total de stations affecte les performances. Si vous avez des besoins en graphiques, en traitement ou en vidéo lourds, vous souhaiterez peut-être limiter le nombre total de stations. Pour plus d’informations, consultez [optimiser les performances du système multipoint services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance).