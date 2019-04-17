---
title: Vue d’ensemble du pare-feu de centre de données
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur le pare-feu de centre de données, qui est une couche réseau, 5-tuple (protocole, source et destination des numéros de port, les adresses IP source et de destination), du pare-feu avec état et mutualisé dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c9b9fb5b0fb9aa09ed783b2b66a8ad370a627c3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="datacenter-firewall-overview"></a>Vue d’ensemble du pare-feu de centre de données

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Pare-feu de centre de données est un nouveau service inclus avec Windows Server2016. Il est une couche réseau, 5-tuple (protocole, numéros de port source et de destination, adresses IP source et destination) avec état et multi-locataire. Lorsque déployé et proposées en tant que service par le fournisseur de services, les administrateurs clients peuvent installer et configurer des stratégies de pare-feu pour protéger leurs réseaux virtuels du trafic indésirable provenant d’Internet et les réseaux intranet.  
  
![Pare-feu de centre de données dans la pile réseau](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
L’administrateur de fournisseur de service ou l’administrateur client permettre gérer les stratégies de pare-feu de centre de données via le contrôleur de réseau et les API northbound.  
  
Le pare-feu de centre de données offre les avantages suivants pour les fournisseurs de services cloud:  
  
-   Une solution hautement évolutive et diagnosable pare-feu basé sur les logiciels qui peut être proposée aux clients  
  
-   Possibilité de déplacer des ordinateurs virtuels clients vers calcul différents hôtes sans interrompre les stratégies de pare-feu clients  
  
    -   Déployé en tant qu’un pare-feu d’agent hôte vSwitch port  
  
    -   Ordinateurs virtuels clients obtenir les stratégies affectées à leur pare-feu de l’agent hôte vSwitch  
  
    -   Règles de pare-feu sont configurés dans chaque port de commutateur virtuel, indépendamment de l’hôte réel de l’ordinateur virtuel en cours d’exécution  
  
-   Offre une protection pour les ordinateurs virtuels indépendants du système d’exploitation invité client pour les clients  
  
Le pare-feu de centre de données offre les avantages suivants pour les locataires:  
  
-   Possibilité de définir des règles de pare-feu pour protéger les charges de travail sur des réseaux virtuels accessible sur Internet  
  
-   Possibilité de définir des règles de pare-feu pour protéger le trafic entre les ordinateurs virtuels sur le même L2 sous-réseau virtuel, ainsi qu’entre les ordinateurs virtuels sur différents sous-réseaux virtuels L2  
  
-   Possibilité de définir des règles de pare-feu pour protéger et isoler le trafic réseau entre le client local réseaux et leurs réseaux virtuels sur le fournisseur de services  
  


