---
title: Guide de scénario de stratégie DNS
description: Cette rubrique fait partie du Guide de scénario de stratégie DNS pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fab127fcd524526a515752871c1a3db92047c46
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406230"
---
# <a name="dns-policy-scenario-guide"></a>Guide de scénario de stratégie DNS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Ce guide est destiné à être utilisé par les administrateurs DNS, réseau et système.  
  
La stratégie DNS est une nouvelle fonctionnalité de DNS dans Windows Server @ no__t-0 2016. Vous pouvez utiliser ce guide pour apprendre à utiliser la stratégie DNS afin de contrôler la façon dont un serveur DNS traite les requêtes de résolution de noms en fonction des différents paramètres que vous définissez dans les stratégies.   
  
Ce guide contient des informations de présentation de la stratégie DNS, ainsi que des scénarios de stratégie DNS spécifiques qui vous fournissent des instructions sur la façon de configurer le comportement du serveur DNS pour atteindre vos objectifs, notamment la gestion du trafic basé sur la géolocalisation pour les serveurs principaux et serveurs DNS secondaires, haute disponibilité de l’application, DNS split-brain, et bien plus encore.  
  
Ce guide contient les sections suivantes.  
  
- [Vue d’ensemble des stratégies DNS](DNS-Policies-Overview.md)  
- [Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec les serveurs principaux](primary-geo-location.md)  
- [Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des déploiements principaux secondaires](primary-secondary-geo-location.md)  
- [Utiliser une stratégie DNS pour les réponses DNS intelligentes en fonction de l’heure de la journée](dns-tod-intelligent.md)
- [Réponses DNS basées sur l’heure de la journée avec un serveur d’applications Cloud Azure](dns-tod-azure-cloud-app-server.md)
- [Utiliser une stratégie DNS pour le déploiement DNS split-brain](split-brain-DNS-deployment.md)
- [Utiliser la stratégie DNS pour le DNS split-brain dans Active Directory](dns-sb-with-ad.md)
- [Utiliser une stratégie DNS pour appliquer des filtres sur les requêtes DNS](apply-filters-on-dns-queries.md)
- [Utiliser une stratégie DNS pour l’équilibrage de charge d’application](app-lb.md)
- [Utiliser une stratégie DNS pour l’équilibrage de charge d’application avec reconnaissance de géolocalisation](app-lb-geo.md)

