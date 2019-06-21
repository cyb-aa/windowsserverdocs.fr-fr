---
title: Gérer les clients DirectAccess à distance
description: Cette rubrique fait partie du guide les Clients DirectAccess de gérer à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 36255d80-a13e-4af7-a5c0-ab4c8f302622
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c4ebab1cb444df9c756d66ded24e1c851023d17a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281167"
---
# <a name="manage-directaccess-clients-remotely"></a>Gérer les clients DirectAccess à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

L'analyse d'accès à distance rend compte de l'activité et de l'état des utilisateurs distants quant aux connexions DirectAccess et VPN. Elle relève le nombre et la durée des connexions clientes (parmi d'autres statistiques) et analyse l'état de fonctionnement du serveur. Une console d'analyse facile à utiliser propose un aperçu de toute votre infrastructure d'accès à distance. Vous avez la possibilité d'afficher l'analyse de configurations de serveur unique, de cluster et multisites.  
  
**Remarque :** Windows Server 2016 combine DirectAccess et le Service d’accès à distance (RAS) dans un seul rôle accès à distance.  
  
## <a name="in-this-guide"></a>Dans ce guide  
Ce document contient des instructions pour exploiter les fonctionnalités d'analyse de l'accès à distance à l'aide de la console de gestion DirectAccess et des applets de commande Windows PowerShell correspondantes, fournies dans le cadre du rôle serveur Accès à distance.  
  
Les scénarios d'analyse et de gestion de comptes suivants sont expliqués :  
  
1.  Analyser la charge existante sur le serveur d'accès à distance  
  
2.  Analyser l'état de distribution de la configuration du serveur d'accès à distance  
  
3.  Analyser l'état de fonctionnement du serveur d'accès à distance et de ses composants  
  
4.  Identifier et résoudre les problèmes de fonctionnement du serveur d'accès à distance  
  
5.  Analyser l'activité et l'état des clients distants connectés  
  
6.  Générer un rapport d'utilisation pour les clients distants à l'aide de données d'historique  
  
### <a name="understand-monitoring-and-accounting"></a>Comprendre l'analyse et la gestion de compte  
Avant de démarrer des tâches d'analyse et de gestion de comptes pour les clients distants, vous devez comprendre la différence entre les deux.  
  
-   L'**analyse** présente les utilisateurs actifs et connectés à un instant donné.  
  
-   La **gestion de comptes** conserve un historique des utilisateurs qui se sont connectés au réseau d'entreprise, ainsi que leurs informations d'utilisation (à des fins de conformité et d'audit).  
  
L'analyse des clients distants se base sur les connexions. Deux types de connexions de tunnel sont établis par les clients DirectAccess :  
  
-   **Connexions du trafic du tunnel de l'ordinateur** : Ce tunnel est établi par l'ordinateur, dans le contexte du système, pour accéder aux serveurs requis pour la résolution de noms, l'authentification, les mises à jour correctives, etc.  
  
-   **Connexions du trafic du tunnel de l'utilisateur** : Ce tunnel est établi par le compte d'utilisateur sur l'ordinateur, dans le contexte d'un utilisateur, quand ce dernier essaie d'accéder à une ressource sur le réseau d'entreprise. Selon les conditions requises du déploiement, un utilisateur peut avoir à fournir des informations d'identification fortes (par exemple, à l'aide d'une carte à puce ou en fournissant un mot de passe à usage unique) pour accéder aux ressources du réseau d'entreprise.  
  
Pour DirectAccess, une connexion est spécifiquement identifiée par l'adresse IP du client distant. Par exemple, si un tunnel d'ordinateur est ouvert pour un ordinateur client et qu'un utilisateur s'est connecté à partir de cet ordinateur, ils utilisent la même connexion. Quand l'utilisateur se déconnecte, puis se reconnecte tandis que le tunnel d'ordinateur est toujours actif, la connexion reste la même.  
  


