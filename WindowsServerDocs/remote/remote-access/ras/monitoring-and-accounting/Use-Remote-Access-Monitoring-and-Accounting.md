---
title: Utiliser l'analyse et la gestion de comptes de l'accès à distance
description: Cette rubrique fait partie du Guide d’analyse et de gestion de l’accès à distance dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 92519b49-0df4-43c1-9717-f13570644212
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 03d367f2f981ca3ed649f1ca4d5eca23967c9cfc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860512"
---
# <a name="use-remote-access-monitoring-and-accounting"></a>Utiliser l'analyse et la gestion de comptes de l'accès à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

L'analyse d'accès à distance rend compte de l'activité et de l'état des utilisateurs distants quant aux connexions DirectAccess et VPN. Elle relève le nombre et la durée des connexions clientes (parmi d'autres statistiques) et analyse l'état de fonctionnement du serveur. Une console d'analyse facile à utiliser propose un aperçu de toute votre infrastructure d'accès à distance. Vous avez la possibilité d'afficher l'analyse de configurations de serveur unique, de cluster et multisites.  
  
**Remarque :** Windows Server 2012 associe DirectAccess et le service Routage et accès distant (RRAS) dans un rôle Accès à distance unique.  
  
> [!NOTE]  
> Outre cette rubrique, les rubriques suivantes sur la surveillance de l'accès à distance sont disponibles.  
>   
> -   [Analyser la charge existante sur le serveur d’accès à distance](Monitor-the-existing-load-on-the-Remote-Access-server.md)  
> -   [Analyser l’état de distribution de la configuration du serveur d’accès à distance](Monitor-the-configuration-distribution-status-of-the-Remote-Access-server.md)  
> -   [Surveiller l’état des opérations du serveur d’accès à distance et de ses composants](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md)  
> -   [Identifier et résoudre les problèmes de fonctionnement du serveur d’accès à distance](Identify-and-resolve-Remote-Access-server-operations-problems.md)  
> -   [Analyser l’activité et l’état des clients distants connectés](Monitor-connected-remote-clients-for-activity-and-status.md)  
> -   [Générer un rapport d’utilisation pour les clients distants à l’aide de données d’historique](Generate-a-usage-report-for-remote-clients-using-historical-data.md)  

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
  
-   **Connexions du trafic du tunnel de l'ordinateur**: Ce tunnel est établi par l'ordinateur, dans le contexte du système, pour accéder aux serveurs requis pour la résolution de noms, l'authentification, les mises à jour correctives, etc.  
  
-   **Connexions du trafic du tunnel de l'utilisateur**: Ce tunnel est établi par le compte d'utilisateur sur l'ordinateur, dans le contexte d'un utilisateur, quand ce dernier essaie d'accéder à une ressource sur le réseau d'entreprise. Selon les conditions requises du déploiement, un utilisateur peut avoir à fournir des informations d'identification fortes (par exemple, à l'aide d'une carte à puce ou en fournissant un mot de passe à usage unique) pour accéder aux ressources du réseau d'entreprise.  
  
Pour DirectAccess, une connexion est spécifiquement identifiée par l'adresse IP du client distant. Par exemple, si un tunnel d'ordinateur est ouvert pour un ordinateur client et qu'un utilisateur s'est connecté à partir de cet ordinateur, ils utilisent la même connexion. Quand l'utilisateur se déconnecte, puis se reconnecte tandis que le tunnel d'ordinateur est toujours actif, la connexion reste la même.  
  


