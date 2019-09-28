---
title: Compteurs de performance liées au réseau
description: Cette rubrique fait partie du Guide d’optimisation des performances du sous-système réseau pour Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ebff972d670f3fd0b8d12959d161bce03ac487e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401846"
---
# <a name="network-related-performance-counters"></a>Compteurs de performance liées au réseau

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique répertorie les compteurs pertinents pour la gestion des performances réseau et contient les sections suivantes.  
  
-   [Utilisation des ressources](#bkmk_ru)  
  
-   [Problèmes potentiels liés au réseau](#bkmk_np)  
  
-   [Performances de la fusion côté réception (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a>Utilisation des ressources  

Les compteurs de performances suivants sont pertinents pour l’utilisation des ressources réseau.  
  
- IPv4, IPv6  
  
  -   Datagrammes reçus/s  
  
  -   Datagrammes envoyés/s  
  
- TCPv4, TCPv6  
  
  -   Segments reçus/s  
  
  -   Segments envoyés/s  
  
  -   Segments retransmis/s  
  
- Interface réseau (*), carte réseau (\*)  
  
  - Octets reçus/s  
  
  - Octets envoyés/s  
  
  - Paquets reçus/s  
  
  - Paquets envoyés/s  
  
  - Longueur de la file d'attente de sortie  
  
    Ce compteur est la longueur de la file d’attente de paquets en sortie @no__t les paquets 0in-no__t-1. Si la longueur est supérieure à 2, des retards se produisent. Vous devez trouver le goulot d’étranglement et l’éliminer si possible. Étant donné que NDIS met en file d’attente les requêtes, cette longueur doit toujours être 0.  
  
- Informations sur le processeur  
  
  - % Processor Time  
  
  - Interruptions/s  
  
  - DPC mis en file d’attente/s  
  
    Ce compteur est une vitesse moyenne à laquelle les appels DPC ont été ajoutés à la file d’attente DPC du processeur logique. Chaque processeur logique a sa propre file d’attente DPC. Ce compteur mesure la vitesse à laquelle les DPC sont ajoutés à la file d’attente, et non le nombre de DPC dans la file d’attente. Il affiche la différence entre les valeurs observées dans les deux derniers échantillons, divisée par la durée de l’intervalle échantillon.  
  
##  <a name="bkmk_np"></a>Problèmes potentiels liés au réseau  

Les compteurs de performances suivants sont pertinents pour les problèmes réseau potentiels.  
  
-   Interface réseau (*), carte réseau (\*)  
  
    -   Paquets reçus ignorés  
  
    -   Paquets reçus, Erreurs  
  
    -   Paquets sortants rejetés  
  
    -   Paquets sortants, Erreurs  
  
-   WFPv4, WFPv6  
  
    -   Paquets ignorés/s

-   UDPv4, UDPv6

    -   Datagrammes reçus, Erreurs  
  
-   TCPv4, TCPv6  
  
    -   Échecs de connexion  
  
    -   Connexions réinitialisées  
  
-   Stratégie QoS réseau  
  
    -   Paquets supprimés  
  
    -   Paquets ignorés/s  
  
-   Activité de la carte d’interface réseau par processeur  
  
    -   Indications sur la réception des ressources faibles/s  
  
    -   Paquets reçus de ressources faibles/s  
  
-   BSP Microsoft Winsock  
  
    -   Datagrammes abandonnés  
  
    -   Datagrammes ignorés/s  
  
    -   Connexions rejetées  
  
    -   Connexions rejetées/s  
  
##  <a name="bkmk_rsc"></a>Performances de la fusion côté réception (RSC)  

Les compteurs de performances suivants sont pertinents pour les performances de RSC.  
  
-   Carte réseau(*)  
  
    -   Connexions TCP actives RSC  
  
    -   Taille moyenne des paquets TCP RSC  
  
    -   Paquets TCP RSC fusionnés/s  
  
    -   Exceptions TCP RSC/s

Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances du sous-système réseau](net-sub-performance-top.md).