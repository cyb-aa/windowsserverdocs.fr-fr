---
title: Compteurs de performance liées au réseau
description: Cette rubrique fait partie du guide de réglage de performances du sous-système de réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e5e8abbc19482bcd0dd5670065cde59d5be3169a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824350"
---
# <a name="network-related-performance-counters"></a>Compteurs de performance liées au réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique répertorie les compteurs qui sont pertinents pour la gestion des performances du réseau et contient les sections suivantes.  
  
-   [Utilisation des ressources](#bkmk_ru)  
  
-   [Problèmes potentiels de réseau](#bkmk_np)  
  
-   [Recevoir des performances de la fusion côté (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a> Utilisation des ressources  

Les compteurs de performances suivants sont pertinents pour l’utilisation des ressources réseau.  
  
-   IPv4, IPv6  
  
    -   Datagrammes reçus/s  
  
    -   Datagrammes envoyés/s  
  
-   TCPv4, TCPv6  
  
    -   Segments reçus/s  
  
    -   Segments envoyés/s  
  
    -   Segments retransmis/s  
  
-   Interface(*) du réseau, la carte réseau (\*)  
  
    -   Octets reçus/s  
  
    -   Octets envoyés/s  
  
    -   Paquets reçus/s  
  
    -   Paquets envoyés/s  
  
    -   Longueur de la file d'attente de sortie  
  
     Ce compteur est la longueur de la file d’attente des paquets de sortie \(dans les paquets\). S’il s’agit plu de 2, sont rencontrés. Vous devez trouver le goulot d’étranglement et éliminer si vous le pouvez. Étant donné que NDIS files d’attente les demandes, cette longueur doit toujours être 0.  
  
-   Informations de processeur  
  
    -   % Processor Time  
  
    -   Interruptions/s  
  
    -   Les appels DPC en file d’attente/s  
  
     Ce compteur est un taux moyen auquel les appels DPC ont été ajoutés à la file d’attente DPC du processeur logique. Chaque processeur logique a sa propre file d’attente DPC. Ce compteur mesure le taux auquel les appels DPC sont ajoutés à la file d’attente, pas le nombre de DPC dans la file d’attente. Il affiche la différence entre les valeurs qui ont été observées dans les deux derniers intervalles de temps, divisée par la durée de l’intervalle échantillon.  
  
##  <a name="bkmk_np"></a> Problèmes potentiels de réseau  

Les compteurs de performances suivants sont pertinentes aux éventuels problèmes de réseau.  
  
-   Interface(*) du réseau, la carte réseau (\*)  
  
    -   Paquets reçus et jetés  
  
    -   Paquets reçus, erreurs  
  
    -   Paquets sortants rejetés  
  
    -   Paquets sortants, erreurs  
  
-   WFPv4, WFPv6  
  
    -   Paquets ignorés/s

-   UDPv4, UDPv6

    -   Datagrammes reçus, erreurs  
  
-   TCPv4, TCPv6  
  
    -   Échecs de connexion  
  
    -   Connexions réinitialisées  
  
-   Stratégie de qualité de service réseau  
  
    -   Paquets abandonnés  
  
    -   Paquets supprimés/s  
  
-   Par Processor Network Interface Card Activity  
  
    -   Insuffisance de ressources de la réception des Indications/s  
  
    -   Insuffisance de ressources reçu des paquets/s  
  
-   Microsoft Winsock BSP  
  
    -   Datagrammes supprimées  
  
    -   Datagrammes ignorés/s  
  
    -   Connexions rejetées  
  
    -   Connexions rejetées/s  
  
##  <a name="bkmk_rsc"></a> Recevoir des performances de la fusion côté (RSC)  

Les compteurs de performances suivants sont pertinents pour les performances RSC.  
  
-   Carte réseau  
  
    -   Connexions TCP RSC Active  
  
    -   Taille de paquet moyenne TCP RSC  
  
    -   TCP RSC fusionnés paquets/s  
  
    -   TCP RSC Exceptions/s

Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances réseau sous-système](net-sub-performance-top.md).