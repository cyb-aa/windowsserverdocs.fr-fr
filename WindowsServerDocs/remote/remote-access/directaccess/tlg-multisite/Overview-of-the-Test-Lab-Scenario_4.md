---
title: Vue d’ensemble du scénario de laboratoire de test
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 46bcc22604a11a3fc3da764e149d900b1ada1dc8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860382"
---
# <a name="overview-of-the-test-lab-scenario"></a>Vue d’ensemble du scénario de laboratoire de test

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans ce scénario de laboratoire de test, DirectAccess est déployé avec :  
  
-   **DC1**: serveur configuré en tant que contrôleur de domaine, serveur DNS et serveur DHCP pour le domaine Corp.contoso.com.  
  
-   **2-DC1**: serveur configuré en tant que contrôleur de domaine et serveur DNS pour le domaine corp2.Corp.contoso.com.  
  
-   **Edge1 et 2-Edge1**-deux serveurs sur le réseau interne qui sont configurés en tant que serveurs d’accès à distance. Chaque serveur a deux cartes réseau ; l’une connectée au réseau interne et l’autre connectée au réseau externe.  
  
-   **App1 et 2-App1**-deux serveurs sur le réseau interne qui sont configurés en tant que serveurs Web et de fichiers.  
  
-   **App2**: un ordinateur sur le réseau interne configuré en tant que serveur Web et serveur de fichiers IPv4 uniquement. Cet ordinateur est utilisé pour mettre en surbrillance les fonctionnalités NAT64/DNS64.  
  
-   **ROUTEUR1**: serveur configuré pour assurer le routage entre les deux réseaux internes de l’entreprise.  
  
-   **INET1**: serveur configuré en tant que serveur DNS Internet et serveur DHCP.  
  
-   **NAT1**: ordinateur client configuré en tant qu’appareil NAT (Network Address Translator) à l’aide du partage de connexion Internet.  
  
-   **CLIENT1 et client2**-deux ordinateurs clients configurés en tant que clients DirectAccess qui seront utilisés pour tester la connectivité DirectAccess lors du déplacement entre le réseau interne, l’Internet simulé et un réseau privé. **Client2** est un client de&reg; Windows 7.  
  
Le laboratoire de test se compose de quatre sous-réseaux qui simulent les éléments suivants :  
  
-   Un réseau local nommé HomeNet (192.168.137.0/24) connecté à Internet par un NAT.  
  
-   Le réseau externe représenté par le sous-réseau Internet (131.107.0.0/24).  
  
-   Un réseau interne nommé corpnet (10.0.0.0/24 ; 2001 : DB8:1 ::/64), séparé d’Internet par le serveur d’accès à distance EDGE1.  
  
-   Un réseau interne nommé 2-Corpnet1 (10.2.0.0/24 ; 2001 : DB8:2 ::/64), séparé d’Internet par le serveur d’accès à distance 2-EDGE1.  
  
Les ordinateurs de chaque sous-réseau se connectent à l’aide d’un concentrateur ou d’un concentrateur physique ou virtuel, comme illustré dans la figure suivante.  
  
![Vue d’ensemble du laboratoire de test](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)  
  


