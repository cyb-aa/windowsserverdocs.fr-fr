---
title: Guide de réseau de base pour Windows Server
description: Cette rubrique fournit une vue d’ensemble du Guide réseau de base, qui vous permet de planifier et déployer les principaux composants requis pour un réseau pleinement fonctionnel et un nouveau domaine ActiveDirectory dans une nouvelle forêt avec Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 63e4cf8c5bf56ef5131e835163a5fcb5dfd98b55
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide-for-windows-server"></a>Guide de réseau de base pour Windows Server

>S’applique à: Windows Server, Windows Server2016

Cette rubrique fournit une vue d’ensemble de base réseau Guide for Windows Server&reg; 2016 et contient les sections suivantes.  
  
-   [Introduction au réseau WindowsServerCore](#bkmk_intro)  
  
-   [Guide de réseau de base pour Windows Server](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Introduction au réseau WindowsServerCore

Un réseau de base est une collection de matériels de réseau, périphériques et logiciels qui fournit les services centraux répondant pour les technologies de l’information de votre organisation (IT) aux besoins.

Un réseau de base Windows Server vous offre de nombreux avantages, notamment les suivantes.

- Protocoles centraux pour la connectivité réseau entre les ordinateurs et autres appareils compatibles TCP/IP Transmission Control Protocol/Internet Protocol (). TCP/IP est une suite de protocoles standard pour la connexion des ordinateurs et de création de réseaux. TCP/IP est un logiciel de protocole réseau fourni avec Microsoft&reg; Windows&reg; suite du protocole de systèmes d’exploitation qui implémente et prend en charge le protocole TCP/IP.

- Dynamic Host Configuration Protocol (DHCP) server l’adressage IP automatique. Configuration manuelle d’adresses IP sur tous les ordinateurs sur votre réseau prend du temps et est moins souple que dynamiquement aux ordinateurs et autres périphériques avec des baux d’adresses IP à partir d’un serveur DHCP.

- Service de résolution de nom du système DNS (Domain Name). DNS permet aux utilisateurs, ordinateurs, applications et services trouver les adresses IP des ordinateurs et périphériques sur le réseau à l’aide du nom de domaine complet de l’ordinateur ou le périphérique.

- Une forêt, qui est un ou plusieurs domaines ActiveDirectory qui partagent les mêmes définitions de classes et d’attributs (schéma), informations de site et de réplication (configuration) et des fonctionnalités de recherche de la forêt (catalogue global).

- Un domaine racine de forêt, qui est le premier domaine créé dans une nouvelle forêt. Les groupes Administrateurs de l’entreprise et administrateurs du schéma, qui sont des groupes d’administration de forêt, sont trouvent dans le domaine racine de forêt. En outre, un domaine racine de forêt, comme avec d’autres domaines, est une collection d’objets ordinateur, utilisateur et groupe qui sont définies par l’administrateur dans les Services de domaine ActiveDirectory (ADDS). Ces objets partagent une stratégies de sécurité et de la base de données Active courants. Ils peuvent également partager des relations de sécurité avec d’autres domaines si vous ajoutez les domaines que votre organisation se développe. Également, le service d’annuaire stocke les données d’annuaire et permet à des ordinateurs autorisés, applications et les utilisateurs à accéder aux données.

- Une base de données de compte utilisateur et ordinateur. Le service d’annuaire fournit une base de données de comptes utilisateur centralisée qui permet de créer des comptes d’utilisateur et ordinateur pour les personnes et les ordinateurs qui sont autorisés à se connecter à votre réseau et le réseau de l’accès aux ressources, telles que les applications, les bases de données, les fichiers partagés et les dossiers et les imprimantes.

Un réseau de base vous permet également de faire évoluer votre réseau de votre organisation se développe et modifier des besoins. Par exemple, avec un réseau de base, vous pouvez ajouter des domaines, sous-réseaux IP, services d’accès à distance, services sans fil et autres fonctionnalités et rôles serveur fournis par Windows Server2016.

## <a name="bkmk_core"></a>Guide de réseau de base pour Windows Server

Le Guide du réseau Windows Server2016 Core fournit des instructions sur la façon de planifier et déployer les principaux composants requis pour un réseau pleinement fonctionnel et un nouveau répertoire Active&reg; domaine dans une nouvelle forêt. À l’aide de ce guide, vous pouvez déployer des ordinateurs configurés avec les composants du serveur Windows suivants:

- Le rôle de serveur Services de domaine ActiveDirectory (ADDS)

- Le rôle de serveur de système DNS (Domain Name)

- Le rôle de serveur DHCP Dynamic Host Configuration Protocol)

- Le service de rôle serveur NPS (Network Policy) du rôle de serveur de stratégie réseau et Services d’accès

- Le rôle serveur serveur Web (IIS)

- Transmission Control Protocol/Internet Protocol version4 (TCP/IP) connexions sur des serveurs individuels

Ce guide est disponible à l’emplacement suivant.

- Le [Guide réseau de base](../core-network-guide/Core-Network-Guide.md) dans la bibliothèque technique de Windows Server2016.
  


