---
title: Nouveautés dans Windows Server 2016 Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 420d3b043959b8b1201aad7a5b3210fd9bd6a0da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817752"
---
# <a name="whats-new-in-windows-server-2016-essentials"></a>Nouveautés dans Windows Server 2016 Essentials

> S’applique à : Windows Server 2016 Essentials

Voici les fonctionnalités nouvelles et améliorées de Windows Server 2016 Essentials.

## <a name="integration-with-azure-site-recovery-services"></a>[Intégration à Azure Site Recovery Services](azure-site-recovery-services-integration.md)

**Ce qu’il fait** --&reg;lorsqu’un ordinateur virtuel protégé échoue, ou si le serveur hôte sur lequel s’exécute l’ordinateur virtuel protégé échoue, le basculement avec les Services de Azure Site Recovery maintient la continuité de l’activité jusqu’à ce que la machine virtuelle ou le serveur hôte local soit réparé et disponible. 

**Fonctionnement--** les services Azure Site Recovery, proposés dans Microsoft Azure, permettent la réplication en temps réel de vos machines virtuelles vers un coffre de sauvegarde dans Azure. Si votre serveur ou site tombe en panne en raison d’une défaillance matérielle ou autre, vous pouvez basculer avec Azure Site Recovery Services afin que l’image de machine virtuelle stockée dans votre coffre de sauvegarde soit approvisionnée en tant que machine virtuelle en cours d’exécution dans Azure. Associé à un réseau virtuel Azure, les PC clients qui étaient précédemment connectés au serveur local se connectent de manière transparente au serveur s’exécutant dans Azure.     
                                                                                                                                                                                                                                                                                                               

## <a name="integration-with-azure-virtual-network"></a>[Intégration au réseau virtuel Azure](azure-virtual-network-integration.md)

Qu’est- **ce**que les organisations font pour Cloud Computing, elles déplacent rarement toutes leurs ressources en même temps. Au lieu de cela, ils déplacent des ressources dans le Cloud et les conservent localement. De cette façon, il est facile de déplacer une organisation vers le Cloud par étapes au fil du temps. L’intégration du réseau virtuel Azure fournit l’infrastructure réseau qui rend ce processus transparent et gérable.

**Fonctionnement--** Azure Virtual Networking est un service proposé dans Microsoft Azure qui permet aux organisations de créer un réseau privé virtuel (point à point) ou de site à site (S2S) qui rend les ressources qui s’exécutent dans Azure (telles que les machines virtuelles et le stockage) comme si elles se trouvent sur le réseau local pour un accès transparent aux ressources et aux applications.



## <a name="support-for-larger-deployments"></a>[Prise en charge des déploiements plus importants](support-for-larger-deployments.md) 

Certaines petites entreprises de plus grande taille nécessitent davantage de fonctionnalités et de capacités pour implémenter Windows Server Essentials de manière efficace. Windows Server 2016 Essentials offre une plus grande facilité de gestion des domaines, des utilisateurs et des appareils en ajoutant la prise en charge des déploiements plus importants avec :                                                                                                                                                                                                 

 - domaines multiples
 - plusieurs contrôleurs de domaine                                                                                                                                                                                                                                        
 - possibilité de spécifier un contrôleur de domaine désigné                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Voir aussi
--------

[Prise en main de Windows Server essentials](get-started.md) &copy;&reg;