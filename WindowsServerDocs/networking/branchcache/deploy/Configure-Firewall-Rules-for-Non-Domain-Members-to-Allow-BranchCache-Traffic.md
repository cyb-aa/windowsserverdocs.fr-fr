---
title: Configurer les règles de pare-feu pour les membres n'appartenant pas au domaine afin d'autoriser le trafic BranchCache
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 288865f0237969e0bed7e105f8d539759984275e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834770"
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>Configurer les règles de pare-feu pour les membres n'appartenant pas au domaine afin d'autoriser le trafic BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser les informations dans cette rubrique pour configurer les produits de pare-feu tiers et configurer manuellement un ordinateur client avec des règles de pare-feu qui permettent de BranchCache en mode de cache distribué.  
  
> [!NOTE]  
> -   Si vous avez configuré les ordinateurs clients BranchCache à l’aide de la stratégie de groupe, les paramètres de stratégie de groupe remplacent toute configuration manuelle des ordinateurs clients auxquels les stratégies sont appliquées.  
> -   Si vous avez déployé BranchCache avec DirectAccess, vous pouvez utiliser les paramètres dans cette rubrique pour configurer des règles IPsec pour autoriser le trafic BranchCache.  
  
Pour apporter ces modifications de configuration, il est nécessaire au minimum d'appartenir au groupe **Administrateurs** ou à un groupe équivalent.  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD] : La mise en cache de contenu homologue et le protocole de découverte de récupération  
Les clients de cache distribué doivent autoriser le trafic MS-PCCRD entrant et sortant, via le protocole WS-Discovery (Web Services Dynamic Discovery).  
  
Paramètres de pare-feu doivent autoriser le trafic de multidiffusion en plus de trafic entrant et sortant. Vous pouvez utiliser les paramètres suivants pour configurer des exceptions de pare-feu pour le mode de cache distribué.  
  
Multidiffusion IPv4 : 239.255.255.250  
  
Multidiffusion IPv6 : FF02::C  
  
Trafic entrant : Port local : 3702, port distant : éphémères  
  
Trafic sortant : Port local : éphémère, port distant : 3702  
  
Programme : %systemroot%\system32\svchost.exe (Service BranchCache [PeerDistSvc])  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR] : La mise en cache de contenu homologue et la récupération : Protocole de récupération  
Les clients de cache distribué doivent autoriser le trafic MS-PCCRR entrant et sortant, qui est exécutée dans le protocole HTTP 1.1 comme indiqué dans la demande de commentaires RFC 2616.  
  
Paramètres de pare-feu doivent autoriser le trafic entrant et sortant. Vous pouvez utiliser les paramètres suivants pour configurer des exceptions de pare-feu pour le mode de cache distribué.  
  
Trafic entrant : Port local : 80, port distant : éphémères  
  
Trafic sortant : Port local : éphémère, port distant : 80  
  


