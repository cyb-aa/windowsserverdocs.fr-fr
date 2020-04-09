---
title: Compteurs de performance liées au réseau
description: Cette rubrique fait partie du Guide d’optimisation des performances du sous-système réseau pour Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.openlocfilehash: e3350d476b9bfe8ef38cab610a225dcffe0bd02a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862192"
---
# <a name="network-related-performance-counters"></a>Compteurs de performance liées au réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique répertorie les compteurs pertinents pour la gestion des performances réseau et contient les sections suivantes.  
  
-   [Utilisation des ressources](#bkmk_ru)  
  
-   [Problèmes potentiels liés au réseau](#bkmk_np)  
  
-   [Performances de la fusion côté réception (RSC)](#bkmk_rsc)  
  
##  <a name="resource-utilization"></a><a name="bkmk_ru"></a>Utilisation des ressources  

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
  
    Ce compteur correspond à la longueur de la file d’attente de paquets en sortie \(dans les paquets\). Si la longueur est supérieure à 2, des retards se produisent. Vous devez trouver le goulot d’étranglement et l’éliminer si possible. Étant donné que NDIS met en file d’attente les requêtes, cette longueur doit toujours être 0.  
  
- Informations sur le processeur  
  
  - % de temps processeur  
  
  - Interruptions/s  
  
  - DPC mis en file d’attente/s  
  
    Ce compteur est une vitesse moyenne à laquelle les appels DPC ont été ajoutés à la file d’attente DPC du processeur logique. Chaque processeur logique a sa propre file d’attente DPC. Ce compteur mesure la vitesse à laquelle les DPC sont ajoutés à la file d’attente, et non le nombre de DPC dans la file d’attente. Il affiche la différence entre les valeurs observées dans les deux derniers échantillons, divisée par la durée de l’intervalle échantillon.  
  
##  <a name="potential-network-problems"></a><a name="bkmk_np"></a>Problèmes potentiels liés au réseau  

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
  
##  <a name="receive-side-coalescing-rsc-performance"></a><a name="bkmk_rsc"></a>Performances de la fusion côté réception (RSC)  

Les compteurs de performances suivants sont pertinents pour les performances de RSC.  
  
-   Carte réseau(*)  
  
    -   Connexions TCP actives RSC  
  
    -   Taille moyenne des paquets TCP RSC  
  
    -   Paquets TCP RSC fusionnés/s  
  
    -   Exceptions TCP RSC/s

Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances du sous-système réseau](net-sub-performance-top.md).