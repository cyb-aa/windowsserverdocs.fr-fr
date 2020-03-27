---
title: Présentation du pare-feu de centre de données
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur le pare-feu de centre de code, qui est une couche réseau, 5 tuples (protocole, numéros de port source et de destination, adresses IP source et de destination), pare-feu avec état et multi-locataires dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 141057d2ee3e648f589d255ea04fdef179cedf3c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317035"
---
# <a name="datacenter-firewall-overview"></a>Présentation du pare-feu de centre de données

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le pare-feu de centre de centres est un nouveau service inclus avec Windows Server 2016. Il s’agit d’une couche réseau, 5-Tuple (protocole, numéros de port source et de destination, adresses IP source et de destination), pare-feu avec état et multi-locataire. Une fois déployé et proposé en tant que service par le fournisseur de services, les administrateurs clients peuvent installer et configurer des stratégies de pare-feu pour protéger leurs réseaux virtuels contre le trafic indésirable provenant des réseaux Internet et intranet.  
  
![Pare-feu de centre de la pile réseau](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
L’administrateur du fournisseur de services ou l’administrateur client peut gérer les stratégies de pare-feu de centre de l’aide du contrôleur de réseau et des API Northbound.  
  
Le pare-feu de centre de code offre les avantages suivants pour les fournisseurs de services Cloud :  
  
-   Une solution de pare-feu logicielle hautement évolutive, gérable et pouvant être diagnostiquée qui peut être proposée aux locataires  
  
-   Liberté de déplacer les machines virtuelles clientes vers différents hôtes de calcul sans rompre les stratégies de pare-feu de locataire  
  
    -   Déployé en tant que pare-feu de l’agent hôte du port vSwitch  
  
    -   Les machines virtuelles clientes obtiennent les stratégies affectées à leur pare-feu de l’agent hôte vSwitch  
  
    -   Les règles de pare-feu sont configurées dans chaque port vSwitch, indépendamment de l’hôte réel qui exécute l’ordinateur virtuel.  
  
-   Offre une protection aux machines virtuelles clientes indépendamment du système d’exploitation invité du locataire.  
  
Le pare-feu de centre de code offre les avantages suivants pour les locataires :  
  
-   Possibilité de définir des règles de pare-feu pour protéger les charges de travail accessibles sur Internet sur des réseaux virtuels  
  
-   Possibilité de définir des règles de pare-feu pour protéger le trafic entre les machines virtuelles sur le même sous-réseau virtuel L2 et entre les machines virtuelles sur différents sous-réseaux virtuels L2  
  
-   Possibilité de définir des règles de pare-feu pour protéger et isoler le trafic réseau entre les réseaux locaux de locataire et leurs réseaux virtuels au niveau du fournisseur de services  
  


