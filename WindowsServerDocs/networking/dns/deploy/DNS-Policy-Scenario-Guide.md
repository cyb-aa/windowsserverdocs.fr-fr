---
title: Guide de scénario de stratégie DNS
description: Cette rubrique fait partie de DNS stratégie scénario Guide pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b3a1400a68e22fc4988c87c9222b66f718cf5fd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865640"
---
# <a name="dns-policy-scenario-guide"></a>Guide de scénario de stratégie DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Ce guide est conçu pour une utilisation par des administrateurs DNS, réseau et les systèmes.  
  
Une stratégie DNS est une nouvelle fonctionnalité pour DNS dans Windows Server&reg; 2016. Vous pouvez utiliser ce guide pour apprendre à utiliser une stratégie DNS pour contrôler la façon dont un serveur DNS traite les requêtes de résolution de nom en fonction de différents paramètres que vous définissez dans les stratégies.   
  
Ce guide contient des informations de vue d’ensemble de stratégie DNS, ainsi que des scénarios de stratégie DNS spécifiques qui fournissent des instructions sur la façon de configurer le comportement du serveur DNS à atteindre vos objectifs, y compris gestion du trafic en fonction de localisation géographique principal et les serveurs DNS secondaires, haute disponibilité des applications, DNS « split brain » et bien plus encore.  
  
Ce guide contient les sections suivantes.  
  
- [Vue d’ensemble des stratégies DNS](DNS-Policies-Overview.md)  
- [Utiliser une stratégie DNS pour l’emplacement géographique en fonction de gestion du trafic avec des serveurs principaux](primary-geo-location.md)  
- [Utiliser une stratégie DNS pour l’emplacement géographique en fonction de gestion du trafic avec des déploiements principaux / secondaires](primary-secondary-geo-location.md)  
- [Utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour](dns-tod-intelligent.md)
- [Réponses DNS basées sur l’heure du jour avec Azure Cloud du serveur d’applications](dns-tod-azure-cloud-app-server.md)
- [Utiliser une stratégie DNS pour le déploiement de DNS « split brain »](split-brain-DNS-deployment.md)
- [Utiliser une stratégie DNS pour DNS « split brain » dans Active Directory](dns-sb-with-ad.md)
- [Utiliser une stratégie DNS pour l’application des filtres sur les requêtes DNS](apply-filters-on-dns-queries.md)
- [Utiliser une stratégie DNS pour l’équilibrage de charge de l’Application](app-lb.md)
- [Utiliser une stratégie DNS pour l’Application équilibrage de charge avec géolocalisation](app-lb-geo.md)

