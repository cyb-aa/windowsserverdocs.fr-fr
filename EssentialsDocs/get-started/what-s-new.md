---
title: Nouveautés dans Windows Server 2016 Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7fed7e71f7ac163437fe5d32da7c867f93fbcf00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869970"
---
#<a name="whats-new-in-windows-server-2016-essentials"></a>Nouveautés dans Windows Server 2016 Essentials

> S'applique à : WindowsServer2016 Essentials

Voici les nouvelles fonctionnalités dans Windows Server 2016 Essentials et améliorations.

##<a name="integration-with-azure-site-recovery-servicesazure-site-recovery-services-integrationmd"></a>[Intégration avec Azure Site Recovery Services](azure-site-recovery-services-integration.md)

**Ce qu’il fait** --lorsqu’un ordinateur virtuel qui est protégé échoue, ou le serveur hôte que la machine virtuelle protégée s’exécute sur échoue, le basculement avec Azure Site Recovery Services gère la continuité des activités jusqu'à ce que la machine virtuelle de site sur ou serveur hôte est réparé et disponible. 

**Son fonctionnement** --Azure Site Recovery Services, proposé dans Microsoft Azure, permet une réplication en temps réel de vos machines virtuelles (VM) dans un coffre de sauvegarde dans Azure. Dans le cas où votre serveur ou un site tombe en panne en raison d’une défaillance matérielle ou autre, vous pouvez basculer avec Azure Site Recovery Services afin que l’image de machine virtuelle stockée dans votre coffre de sauvegarde sera déployé comme une machine virtuelle en cours d’exécution dans Azure. Associé à un réseau virtuel Azure, le client PC précédemment connecté au serveur local en toute transparence se connectera au serveur en cours d’exécution dans Azure.     
                                                                                                                                                                                                                                                                                                               

## <a name="integration-with-azure-virtual-networkazure-virtual-network-integrationmd"></a>[Intégration avec le réseau virtuel Azure](azure-virtual-network-integration.md)

**Ce qu’il fait**--à mesure que les organisations de parvenir au cloud computing, ils rarement déplacent toutes leurs ressources en même temps. Au lieu de cela, ils déplacement des ressources vers le nuage et conserver certaines en local. De cette façon, il est facile de déplacer une organisation dans le cloud par étapes au fil du temps. Intégration au réseau virtuel Azure fournit l’infrastructure de réseau qui rend ce processus transparent et facile à gérer.

**Son fonctionnement** --mise en réseau virtuel Azure est un service proposé sur Microsoft Azure qui permet aux organisations de créer un point à point (P2P) ou site à site (S2S) réseau privé virtuel qui rend les ressources qui sont en cours d’exécution dans Azure (par exemple) machines virtuelles et stockage) rechercher comme s’ils se trouvent sur le réseau local pour transparente des applications et des accès aux ressources.



##<a name="support-for-larger-deploymentssupport-for-larger-deploymentsmd"></a>[Prise en charge des déploiements plus importants](support-for-larger-deployments.md) 

Certaines petites entreprises supérieure avez besoin de davantage de fonctionnalités et de capacité à implémenter efficacement des Windows Server Essentials. Windows Server 2016 Essentials fournit apprécieront la gestion de domaines, les utilisateurs et les appareils en ajoutant la prise en charge des déploiements plus importants avec :                                                                                                                                                                                                 

 - plusieurs domaines
 - plusieurs contrôleurs de domaine                                                                                                                                                                                                                                        
 - possibilité de spécifier un contrôleur de domaine désigné                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Voir aussi
--------

[Prise en main Windows Server Essentials](get-started.md)
