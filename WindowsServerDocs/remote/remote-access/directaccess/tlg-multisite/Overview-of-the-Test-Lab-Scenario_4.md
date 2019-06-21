---
title: Vue d’ensemble du scénario de laboratoire de test
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b067d5f247f5dc13ea294d83a76f267c5a57ffe7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283296"
---
# <a name="overview-of-the-test-lab-scenario"></a>Vue d’ensemble du scénario de laboratoire de test

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans ce scénario de laboratoire de test, DirectAccess est déployé avec :  
  
-   **DC1**- un serveur qui est configuré comme contrôleur de domaine, serveur DNS et le serveur DHCP pour le domaine corp.contoso.com.  
  
-   **2-DC1**- un serveur qui est configuré comme un contrôleur de domaine et le serveur DNS pour le domaine corp2.corp.contoso.com.  
  
-   **EDGE1 et 2-EDGE1**-deux serveurs qui sont configurés en tant que serveurs d’accès à distance sur le réseau interne. Chaque serveur possède deux cartes réseau ; l’une connectée au réseau interne, et l’autre connectée au réseau externe.  
  
-   **App1 et 2-APP1**-deux serveurs sur le réseau interne qui sont configurés comme serveurs de fichiers et web.  
  
-   **APP2**- un ordinateur sur le réseau interne qui est configuré comme un serveur IPv4 uniquement et fichiers web. Cet ordinateur est utilisé pour mettre en évidence les fonctionnalités NAT64/DNS64.  
  
-   **ROUTEUR1**- un serveur qui est configuré pour fournir le routage entre les deux réseaux internes d’entreprise.  
  
-   **INET1**- un serveur qui est configuré comme un serveur Internet DNS et DHCP.  
  
-   **NAT1**- un ordinateur client qui est configuré comme un périphérique network address translator (NAT), à l’aide du partage de connexion Internet.  
  
-   **CLIENT1 et CLIENT2**-deux ordinateurs clients qui sont configurés en tant que clients DirectAccess qui seront utilisés pour tester la connectivité de DirectAccess lors du déplacement entre le réseau interne, l’Internet simulé et un réseau domestique. **CLIENT2** est Windows 7&reg; client.  
  
Le laboratoire de test se compose de quatre sous-réseaux qui simulent les éléments suivants :  
  
-   Un réseau domestique nommé réseau domestique (192.168.137.0/24) connecté à Internet par un NAT.  
  
-   Le réseau externe représenté par le sous-réseau Internet (131.107.0.0/24).  
  
-   Un réseau interne nommé Corpnet (10.0.0.0/24 ; 2001:db8:1 :: / 64) séparés à partir d’Internet par le serveur d’accès à distance EDGE1.  
  
-   Un réseau interne nommé Corpnet1-2 (10.2.0.0/24 ; 2001:db8:2 :: / 64) séparés à partir d’Internet par le serveur d’accès à distance de 2-EDGE1.  
  
Ordinateurs sur chaque sous-réseau se connectent à l’aide d’un concentrateur physique ou virtuel ou un commutateur, comme indiqué dans l’illustration suivante.  
  
![Vue d’ensemble du laboratoire de test](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)  
  


