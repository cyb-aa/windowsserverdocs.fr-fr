---
title: Recommandations relatives au réseau principal pour Windows Server
description: Cette rubrique fournit une vue d’ensemble du Guide du réseau de base, qui vous permet de planifier et de déployer les composants principaux requis pour un réseau pleinement fonctionnel et un nouveau domaine de Active Directory dans une nouvelle forêt avec Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 9b3ef3eb-4246-4e0e-8bf1-53224ca5f2f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 52f8b9e1446b5b3f3b1e7060cc737204771d1eae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356065"
---
# <a name="core-network-guidance-for-windows-server"></a>Recommandations relatives au réseau principal pour Windows Server

>S’applique à : Windows Server, Windows Server 2016

Cette rubrique fournit une vue d’ensemble de l’aide du réseau de base pour Windows Server&reg; 2016 et contient les sections suivantes.  
  
-   [Présentation du réseau Windows Server Core](#bkmk_intro)  
  
-   [Guide du réseau de base pour Windows Server](#bkmk_core)  
  
## <a name="bkmk_intro"></a>Présentation du réseau Windows Server Core

Un réseau de base est un ensemble de matériels, périphériques et logiciels réseau qui fournissent les services centraux répondant aux besoins en technologies de l’information de votre organisation.

Un réseau de base Windows Server présente de nombreux avantages, notamment :

- Des protocoles centraux pour la connectivité réseau entre les ordinateurs et d’autres périphériques compatibles avec le protocole TCP/IP (Transmission Control Protocol/Internet Protocol). TCP/IP est une suite de protocoles standard utilisée pour la connexion d’ordinateurs et la création de réseaux. TCP/IP est un logiciel de protocole réseau fourni avec les systèmes d’exploitation Microsoft&reg; Windows&reg; qui implémentent et prennent en charge la suite de protocoles TCP/IP.

- Adressage IP automatique par un serveur DHCP (Dynamic Host Configuration Protocol). La configuration manuelle d’adresses IP sur tous les ordinateurs de votre réseau prend du temps et est moins souple que l’attribution dynamique de baux d’adresses IP aux ordinateurs et autres périphériques par un serveur DHCP.

- Un service de résolution de noms (DNS, Domain Name System). Le service DNS permet aux utilisateurs, aux ordinateurs, aux applications et aux services de trouver les adresses IP des ordinateurs et périphériques du réseau d’après le nom de domaine complet (FQDN, Fully Qualified Domain Name) de l’ordinateur ou du périphérique.

- Une forêt, qui est constituée d’un ou de plusieurs domaines Active Directory partageant les mêmes définitions de classes et d’attributs (schéma), les mêmes informations de site et de réplication (configuration) et des fonctionnalités de recherche à l’échelle de la forêt (catalogue global).

- Un domaine racine de forêt, qui est le premier domaine de forêt créé dans une nouvelle forêt. Les groupes Administrateurs de l’entreprise et Administrateurs du schéma, qui sont des groupes d’administration s’appliquant à l’ensemble de la forêt, sont situés dans le domaine racine de la forêt. Par ailleurs, un domaine racine de forêt, comme les autres domaines, est une collection d’objets de type ordinateur, utilisateur et groupe qui sont définis par l’administrateur dans les services de domaine Active Directory (AD DS). Ces objets partagent une base de données d’annuaire commune et des stratégies de sécurité. Ils peuvent également partager des relations de sécurité avec d’autres domaines si vous ajoutez des domaines à mesure que votre organisation se développe. Le service d’annuaire stocke également des données d’annuaire et permet aux ordinateurs, applications et utilisateurs autorisés d’accéder aux données.

- Une base de données de comptes d’utilisateurs et d’ordinateurs. Le service d’annuaire offre une base de données de comptes d’utilisateurs centralisée qui vous permet de créer des comptes d’utilisateurs et d’ordinateurs pour les personnes et les ordinateurs qui sont autorisés à se connecter à votre réseau et à accéder aux ressources réseau, comme les applications, les bases de données, les fichiers et dossiers partagés, ainsi que les imprimantes.

Un réseau de base vous permet également de faire évoluer votre réseau à mesure que votre organisation se développe et que vos besoins en technologies de l’information évoluent. Par exemple, avec un réseau de base, vous pouvez ajouter des domaines, des sous-réseaux IP, des services d’accès à distance, des services sans fil et d’autres fonctionnalités et rôles de serveur fournis par Windows Server 2016.

## <a name="bkmk_core"></a>Guide du réseau de base pour Windows Server

Le Guide du réseau de base Windows Server 2016 fournit des instructions sur la planification et le déploiement des composants principaux requis pour un réseau pleinement fonctionnel et un nouveau Active Directory&reg; domaine dans une nouvelle forêt. Ce guide vous explique comment déployer des ordinateurs configurés avec les composants Windows Server suivants :

- Rôle serveur des services de domaine Active Directory (AD DS)

- Rôle serveur DNS (Domain Name System)

- Rôle serveur DHCP (Dynamic Host Configuration Protocol)

- Service de rôle NPS (Network Policy Server) du rôle serveur Services de stratégie et d’accès réseau

- Rôle serveur Serveur Web (IIS)

- Connexions TCP/IP (Transmission Control Protocol/Internet Protocol) version 4 sur les ordinateurs individuels

Ce guide est disponible à l’emplacement suivant.

- Le [Guide du réseau de base](../core-network-guide/Core-Network-Guide.md) dans la bibliothèque technique de Windows Server 2016.
  


