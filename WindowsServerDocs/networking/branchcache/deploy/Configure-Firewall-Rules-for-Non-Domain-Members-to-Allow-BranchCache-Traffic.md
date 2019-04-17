---
title: Configurer des règles de pare-feu pour les membres du domaine autoriser le trafic BranchCache
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e5f744141efc35bb493bcd95fad53eafbbc3f78d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>Configurer des règles de pare-feu pour les membres du domaine autoriser le trafic BranchCache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser les informations dans cette rubrique pour configurer les produits de pare-feu tiers et pour configurer manuellement un ordinateur client avec des règles de pare-feu qui permettent à BranchCache à s’exécuter en mode de cache distribué.  
  
> [!NOTE]  
> -   Si vous avez configuré les ordinateurs clients BranchCache à l’aide de la stratégie de groupe, les paramètres de stratégie de groupe remplacent toute configuration manuelle des ordinateurs clients sur lesquels les stratégies sont appliquées.  
> -   Si vous avez déployé BranchCache avec DirectAccess, vous pouvez utiliser les paramètres dans cette rubrique pour configurer des règles IPsec pour autoriser le trafic BranchCache.  
  
L’appartenance au groupe **administrateurs**, ou équivalent est le minimum requis pour effectuer ces modifications de configuration.  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD]: mise en cache de contenu et le protocole de récupération de la découverte d’homologues  
Les clients de cache distribué doivent autoriser le trafic MS-PCCRD entrant et sortant, qui est exécuté dans le protocole Web Services Dynamic Discovery (WS-Discovery).  
  
Paramètres de pare-feu doivent autoriser le trafic de multidiffusion en plus du trafic entrant et sortant. Vous pouvez utiliser les paramètres suivants pour configurer des exceptions de pare-feu pour le mode de cache distribué.  
  
Multidiffusion IPv4: 239.255.255.250  
  
Multidiffusion IPv6: FF02::C  
  
Le trafic entrant: port Local: 3702, port distant: éphémère  
  
Le trafic sortant: port Local: éphémère, port distant: 3702  
  
Programme: %systemroot%\system32\svchost.exe (Service BranchCache [PeerDistSvc])  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR]: mise en cache de contenu et la récupération de l’homologue: protocole de récupération  
Les clients de cache distribué doivent autoriser le trafic MS-PCCRR entrant et sortant, qui est exécutée dans le protocole HTTP 1.1 comme indiqué dans la demande de commentaires RFC2616.  
  
Paramètres de pare-feu doivent autoriser le trafic entrant et sortant. Vous pouvez utiliser les paramètres suivants pour configurer des exceptions de pare-feu pour le mode de cache distribué.  
  
Le trafic entrant: port Local: 80, port distant: éphémère  
  
Le trafic sortant: port Local: éphémère, port distant: 80  
  


