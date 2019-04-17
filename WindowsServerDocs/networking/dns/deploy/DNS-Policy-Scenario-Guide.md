---
title: Guide de scénario de stratégie DNS
description: Cette rubrique fait partie de la DNS stratégie scénario Guide pour Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7eab7aa403271992e9bc38f20ca0ddfa52bdd9cf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="dns-policy-scenario-guide"></a>Guide de scénario de stratégie DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Ce guide est conçu pour une utilisation par des administrateurs DNS, réseau et les systèmes.  
  
Une stratégie DNS est une nouvelle fonctionnalité de DNS dans Windows Server&reg; 2016. Vous pouvez utiliser ce guide pour découvrir comment utiliser une stratégie DNS pour contrôler la façon dont un serveur DNS traite les requêtes de résolution de nom en fonction des différents paramètres que vous définissez dans les stratégies.   
  
Ce guide contient des informations de vue d’ensemble de stratégie DNS, ainsi que les scénarios de stratégie DNS spécifiques qui fournissent des instructions sur la façon de configurer le comportement du serveur DNS à atteindre vos objectifs, y compris gestion géolocalisation en fonction du trafic pour les serveurs DNS principaux et secondaires, haute disponibilité des applications, DNS «split brain» et plus encore.  
  
Ce guide contient les sections suivantes.  
  
- [Vue d’ensemble des stratégies DNS](DNS-Policies-Overview.md)  
- [Utiliser une stratégie DNS pour la géolocalisation en fonction de gestion du trafic avec des serveurs principaux](primary-geo-location.md)  
- [Utiliser une stratégie DNS pour la géolocalisation gestion basée sur le trafic avec déploiements principaux / secondaires](primary-secondary-geo-location.md)  
- [Utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour](dns-tod-intelligent.md)
- [Les réponses DNS basées sur l’heure du jour avec un Azure Cloud serveur d’applications](dns-tod-azure-cloud-app-server.md)
- [Utiliser une stratégie DNS pour Split-Brain déploiement DNS](split-brain-DNS-deployment.md)
- [Utiliser une stratégie DNS pour Split-Brain DNS dans ActiveDirectory](dns-sb-with-ad.md)
- [Utiliser une stratégie DNS pour l’application de filtres sur les requêtes DNS](apply-filters-on-dns-queries.md)
- [Utiliser une stratégie DNS pour l’équilibrage de la charge de l’Application](app-lb.md)
- [Utiliser une stratégie DNS pour l’Application équilibrage de charge avec géolocalisation](app-lb-geo.md)

