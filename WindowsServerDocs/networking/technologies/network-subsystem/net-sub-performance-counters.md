---
title: Compteurs de Performance liés au réseau
description: Cette rubrique fait partie du guide de réglage des performances sous-système réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33551dfd4f76bc13ba69863b782ddae279e0ad16
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-related-performance-counters"></a>Compteurs de Performance liés au réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique répertorie les compteurs qui sont pertinentes pour la gestion des performances du réseau et contient les sections suivantes.  
  
-   [Utilisation des ressources](#bkmk_ru)  
  
-   [Problèmes de réseau](#bkmk_np)  
  
-   [Recevoir des performances de fusion côté (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a>Utilisation des ressources  

Les compteurs de performances suivantes concernent l’utilisation des ressources réseau.  
  
-   IPv4, IPv6  
  
    -   Datagrammes reçus/s  
  
    -   Les datagrammes envoyés/s  
  
-   TCPv4, TCPv6  
  
    -   Segments reçus/s  
  
    -   Segments envoyés/s  
  
    -   Segments diffusé/s  
  
-   Réseau Interface(*), Adapter(\*) du réseau  
  
    -   Octets reçus/s  
  
    -   Octets envoyés/s  
  
    -   Les paquets reçus/s  
  
    -   Paquets envoyés/s  
  
    -   Longueur de file d’attente de sortie  
  
     Ce compteur est la longueur de la sortie paquet file d’attente \(in packets\). S’il s’agit plu de 2, délais se produisent. Vous devez trouver le goulot d’étranglement et éliminer si vous le pouvez. Étant donné que les demandes en file d’attente NDIS, cette longueur doit toujours être 0.  
  
-   Informations de processeur  
  
    -   % Temps processeur  
  
    -   Interruptions processeur/s  
  
    -   Les appels DPC en file d’attente/s  
  
     Ce compteur est un taux moyen auquel les appels DPC ont été ajoutées à la file du processeur logique. Chaque processeur logique a sa propre file DPC. Ce compteur mesure la vitesse à laquelle les appels DPC sont ajoutés à la file d’attente, et non le nombre d’appels DPC dans la file d’attente. Il affiche la différence entre les valeurs qui ont été observés dans les deux derniers échantillons, divisées par la durée de l’intervalle échantillon.  
  
##  <a name="bkmk_np"></a>Problèmes de réseau  

Les compteurs de performances suivantes sont pertinentes d’éventuels problèmes réseau.  
  
-   Réseau Interface(*), Adapter(\*) du réseau  
  
    -   Les paquets reçus et rejetés  
  
    -   Les paquets reçus des erreurs  
  
    -   Paquets sortants rejetés  
  
    -   Paquets sortants, erreurs  
  
-   WFPv4, WFPv6  
  
    -   Paquets ignorés/s

-   UDPv4, UDPv6

    -   Erreurs reçues de datagrammes  
  
-   TCPv4, TCPv6  
  
    -   Échecs de connexion  
  
    -   Réinitialisation de connexions  
  
-   Stratégie de qualité de service réseau  
  
    -   Paquets perdus  
  
    -   Paquets ignorés/s  
  
-   Par l’activité de carte l’Interface réseau du processeur  
  
    -   Ressources insuffisantes recevoir des Indications/s  
  
    -   Ressources insuffisantes reçu des paquets/s  
  
-   BSP Microsoft Winsock  
  
    -   Datagrammes abandonnées  
  
    -   Datagrammes ignorés/s  
  
    -   Connexions rejetées  
  
    -   Connexions rejetées/s  
  
##  <a name="bkmk_rsc"></a>Recevoir des performances de fusion côté (RSC)  

Les compteurs de performances suivantes sont pertinentes pour les performances RSC.  
  
-   Adapter(*) réseau  
  
    -   Connexions TCP RSC Active  
  
    -   Taille de paquet moyenne RSC TCP  
  
    -   TCP RSC fusionné paquets/s  
  
    -   Exceptions RSC TCP/s

Pour obtenir des liens vers toutes les rubriques de ce guide, voir [réglage des performances réseau sous-système](net-sub-performance-top.md).