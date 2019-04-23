---
title: Présentation du pare-feu de centre de données
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur le pare-feu de centre de données, qui est une couche réseau, de 5-tuple (numéros de port de protocole, source et destination, adresses IP source et destination), de pare-feu avec état, mutualisée dans Windows Server 2016.
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
ms.openlocfilehash: f1de50dc61639f4985c9d28fdde6072af650f42e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890830"
---
# <a name="datacenter-firewall-overview"></a>Présentation du pare-feu de centre de données

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Pare-feu de centre de données est un nouveau service inclus avec Windows Server 2016. Il est une couche réseau, de 5-tuple (protocole, numéros de port source et de destination, adresses IP source et destination), de pare-feu avec état et mutualisé. Moment du déploiement et proposés en tant que service par le fournisseur de services, les administrateurs clients peuvent installer et configurer des stratégies de pare-feu pour vous aider à protéger leurs réseaux virtuels à partir de trafic indésirable provenant d’Internet et les réseaux intranet.  
  
![Pare-feu de centre de données dans la pile réseau](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
L’administrateur du fournisseur de service ou l’administrateur client permettre gérer les stratégies de pare-feu de centre de données via le contrôleur de réseau et les API northbound.  
  
Le pare-feu de centre de données offre les avantages suivants pour les fournisseurs de services cloud :  
  
-   Une solution hautement évolutive, gérable et diagnosable pare-feu basé sur logiciel qui peut être proposée aux locataires  
  
-   Possibilité de déplacer des machines virtuelles du locataire aux hôtes de calcul différents sans rompre les stratégies de pare-feu de client  
  
    -   Déployé comme un pare-feu de l’agent de commutateur virtuel port hôte  
  
    -   Machines virtuelles du locataire obtenir les stratégies affectées à leur pare-feu de l’agent hôte vSwitch  
  
    -   Règles de pare-feu sont configurées dans chaque port de commutateur virtuel, indépendamment de l’hôte réel de la machine virtuelle en cours d’exécution  
  
-   Offre une protection de machines virtuelles indépendants du système d’exploitation invité locataire du locataire  
  
Le pare-feu de centre de données offre les avantages suivants pour les clients :  
  
-   Possibilité de définir des règles de pare-feu pour vous aider à protéger sur les charges de travail sur les réseaux virtuels Internet  
  
-   Possibilité de définir des règles de pare-feu pour protéger le trafic entre les machines virtuelles sur le même L2 sous-réseau virtuel ainsi qu’entre des machines virtuelles sur différents sous-réseaux virtuels L2  
  
-   Possibilité de définir des règles de pare-feu pour protéger et isoler le trafic réseau entre le client en local les réseaux et leurs réseaux virtuels au niveau du fournisseur de service  
  


