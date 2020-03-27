---
title: Configurer les règles de pare-feu pour les membres n'appartenant pas au domaine afin d'autoriser le trafic BranchCache
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4a86d37fe8744127a91b7fb89e4f34d4a0a021fa
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318494"
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>Configurer les règles de pare-feu pour les membres n'appartenant pas au domaine afin d'autoriser le trafic BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser les informations de cette rubrique pour configurer des produits de pare-feu tiers et configurer manuellement un ordinateur client avec des règles de pare-feu qui autorisent l’exécution de BranchCache en mode de cache distribué.  
  
> [!NOTE]  
> -   Si vous avez configuré des ordinateurs clients BranchCache à l’aide de stratégie de groupe, les paramètres stratégie de groupe remplacent toute configuration manuelle des ordinateurs clients auxquels les stratégies sont appliquées.  
> -   Si vous avez déployé BranchCache avec DirectAccess, vous pouvez utiliser les paramètres dans cette rubrique pour configurer des règles IPsec pour autoriser le trafic BranchCache.  
  
Pour apporter ces modifications de configuration, il est nécessaire au minimum d'appartenir au groupe **Administrateurs** ou à un groupe équivalent.  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD] : Protocole de découverte de la mise en cache et de la récupération du contenu homologue  
Les clients de cache distribué doivent autoriser le trafic MS-PCCRD entrant et sortant, via le protocole WS-Discovery (Web Services Dynamic Discovery).  
  
Les paramètres de pare-feu doivent autoriser le trafic de multidiffusion en plus du trafic entrant et sortant. Vous pouvez utiliser les paramètres suivants pour configurer des exceptions de pare-feu pour le mode de cache distribué.  
  
Multidiffusion IPv4:239.255.255.250  
  
Multidiffusion IPv6 : FF02 :: C  
  
Trafic entrant : port local : 3702, port distant : éphémère  
  
Trafic sortant : port local : éphémère, port distant : 3702  
  
Programme :%systemroot%\system32\svchost.exe (service BranchCache [PeerDistSvc])  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR] : mise en cache et récupération de contenu homologue : Protocole de récupération  
Les clients de cache distribué doivent autoriser le trafic MS-PCCRR entrant et sortant, qui est transmis dans le protocole HTTP 1,1 comme indiqué dans RFC (Request for Comments) 2616.  
  
Les paramètres de pare-feu doivent autoriser le trafic entrant et sortant. Vous pouvez utiliser les paramètres suivants pour configurer des exceptions de pare-feu pour le mode de cache distribué.  
  
Trafic entrant : port local : 80, port distant : éphémère  
  
Trafic sortant : port local : éphémère, port distant : 80  
  


