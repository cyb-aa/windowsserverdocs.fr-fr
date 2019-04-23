---
title: Variables qui affectent les performances du système MultiPoint Services
description: Informations sur les performances de MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f3e8875-1b5e-4789-b16c-d06d6e31f38e
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 7a06fcc763283114dc12ad106aa7ec146502dbd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867580"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>Variables qui affectent les performances du système MultiPoint Services
Il existe plusieurs variables qui peuvent affecter les performances globales de votre système MultiPoint Services. Vous souhaiterez ces lorsque vous concevez votre système.  
  
## <a name="usage"></a>Utilisation  
  
-   **Applications** le type et le nombre d’applications qui s’exécutent en même temps, en particulier graphique\-lourds ou la mémoire des applications intensives affectera les performances globales de votre système. Pour plus d’informations, consultez [Applications et le contenu Internet](hardware-and-performance-recommendations.md#applications-and-internet-content).  
  
-   **Utilisation d’Internet** envisager si vos utilisateurs verront un contenu multimédia ou des pages web qui utilisent des vidéos de plein écran. Ce type de contenu peut surcharger le système si trop d’utilisateurs affichez simultanément.  
  
    > [!NOTE]  
    > La fonctionnalité de projection dans MultiPoint Services, ce qui permet aux enseignants projeter leurs écrans sur leurs analyses étudiant, n’est pas conçue pour projeter la vidéo. La fonctionnalité de projection est destinée à des fins de démonstration, par exemple affichage d’une procédure.  
  
-   **Les appareils à haut débit** si trop d’utilisateurs simultanément utilisent un appareil à haut débit, comme une caméra web ou un lecteur de DVD, cela affecte les performances globales du système.  
  
## <a name="configuration"></a>Configuration  
  
-   **Processeur, de GPU et de RAM** consultez [optimiser les performances de système MultiPoint Services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance) dans ce guide pour obtenir des recommandations processeur, de GPU et de RAM.  
-   **La bande passante réseau** pour RDP sur le réseau local est important, en particulier si la vidéo est en cours d’exécution dans la session utilisateur stations connectées, la bande passante réseau et la capacité du client (par exemple, un client léger, postes de travail PC ou ordinateurs portables). Si vous n’utilisez pas les clients over-Ethernet USB zéro, la bande passante réseau doit également être un facteur important. Données vidéo pour tous les appareils sont envoyées via la même connexion Ethernet, vous pouvez donc à prendre en compte la configuration d’un réseau Gigabit Ethernet distinct lors de l’utilisation de ces appareils.  
-   **RemoteFX** pour les stations de RDP sur le réseau local connecté, vous pourrez peut-être utiliser RemoteFX pour améliorer considérablement la distribution de contenu multimédia de haute définition.  
-   **Résolution d’affichage** si vous avez une utilisation élevée de vidéo plein écran, vous souhaiterez envisager de réduire la résolution d’écran pour optimiser les performances.  
-   **Nombre de clients USB zéro** le nombre total de clients USB zéro sur un concentrateur racine unique sur le serveur affecte directement les performances vidéo. Pour plus d’informations, consultez [disposition pour les Stations USB zéro Client connecté](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations). Le nombre de stations over-Ethernet USB zéro client sont prises en charge peut être légèrement inférieur au nombre de clients USB zéro.  
-   **La bande passante USB** prendre en compte la bande passante USB lorsque vous concevez votre système.  Cela est particulièrement important pour les clients USB zéro, ce qui envoient des données vidéo sur la connexion USB. Pour optimiser la bande passante, réduire le nombre d’appareils qui sont connectés à un seul port USB sur le serveur. Cela s’applique aux concentrateurs intermédiaires et de stations de connecter en chaîne. Pour plus d’informations, consultez [concentrateurs de Station](MultiPoint-services-Site-Planning.md#station-hubs) et [intermédiaire hubs](MultiPoint-services-Site-Planning.md#intermediate-hubs).  
  
-   **Type USB** à l’aide de USB 3.0 au lieu de USB 2.0 augmente la bande passante disponible entre le serveur et le concentrateur intermédiaire si vous vous connectez plus de trois clients USB zéro pour le concentrateur ou si vous utilisez des périphériques USB de bande passante élevée.  
  
-   **Stations** le nombre total de stations affecte les performances. Si vous avez des besoins vidéo, de traitement ou de graphique, vous souhaiterez limiter le nombre total de stations. Pour plus d’informations, consultez [performances d’optimiser le système MultiPoint Services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance).