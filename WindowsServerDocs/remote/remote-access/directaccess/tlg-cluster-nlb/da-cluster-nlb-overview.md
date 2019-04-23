---
title: Vue d’ensemble du scénario de laboratoire de test de cluster DirectAccess avec équilibrage de charge réseau
description: Cette rubrique fait partie du Guide de laboratoire de Test - décrire de DirectAccess dans un Cluster avec équilibrage de charge réseau Windows pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 118a07b761979d3c32a8f7cf4f149aed56a1a893
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863580"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>Vue d’ensemble du scénario de laboratoire de test de cluster DirectAccess avec équilibrage de charge réseau

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans ce scénario de laboratoire de test, DirectAccess est déployé avec :  
  
-   **DC1**- un serveur qui est configuré comme contrôleur de domaine, serveur de système DNS (Domain Name) et serveur de Configuration protocole DHCP (Dynamic Host).  
  
-   **EDGE1**- serveur sur le réseau interne qui est configuré en tant que le premier serveur d’accès à distance dans un cluster de serveurs d’accès à distance. Ce serveur dispose de deux cartes réseau ; l’une connectée au réseau interne, et l’autre connectée au réseau externe.  
  
-   **EDGE2**- serveur sur le réseau interne qui est configuré en tant que le second serveur d’accès à distance dans un cluster de serveurs d’accès à distance. Ce serveur dispose de deux cartes réseau ; l’une connectée au réseau interne, et l’autre connectée au réseau externe.  
  
-   **App1**- serveur sur le réseau interne qui est configuré comme un serveur web et de fichier et comme une autorité de certification racine entreprise (CA)  
  
-   **APP2**- un ordinateur sur le réseau interne qui est configuré comme un serveur IPv4 uniquement et fichiers web. Cet ordinateur est utilisé pour mettre en évidence les fonctionnalités NAT64/DNS64.  
  
-   **INET1**- un serveur qui est configuré comme un serveur Internet DNS et DHCP.  
  
-   **NAT1**- un ordinateur client qui est configuré comme un périphérique network address translator (NAT), à l’aide du partage de connexion Internet.  
  
-   **CLIENT1**- un ordinateur client qui est configuré comme un client DirectAccess qui sera utilisé pour tester la connectivité de DirectAccess lors du déplacement entre le réseau interne, l’Internet simulé et un réseau domestique.  
  
Le laboratoire de test se compose de trois sous-réseaux qui simulent les éléments suivants :  
  
-   Un réseau domestique nommé réseau domestique (192.168.137.0/24) connecté à Internet par un NAT.  
  
-   Le réseau externe représenté par le sous-réseau Internet (131.107.0.0/24).  
  
-   Un réseau interne nommé Corpnet (10.0.0.0/24 ; 2001:db8:1 :: / 64) séparés à partir d’Internet par le serveur d’accès à distance.  
  
Ordinateurs sur chaque sous-réseau se connectent à l’aide d’un concentrateur physique ou virtuel ou un commutateur, comme indiqué dans l’illustration suivante.  
  
![Vue d’ensemble du laboratoire de test](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)  
  


