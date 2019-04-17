---
title: "Nouveautés dans Windows Server 2016 Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
#<a name="whats-new-in-windows-server-2016-essentials"></a>Nouveautés dans Windows Server 2016 Essentials

> S’applique à: Windows Server2016Essentials

Voici les nouvelles fonctionnalités dans Windows Server 2016 Essentials et améliorations.

##[<a name="integration-with-azure-site-recovery-services"></a>Intégration à Azure Site Recovery Services](azure-site-recovery-services-integration.md)

**Action** --lorsqu’un ordinateur virtuel qui est protégé échoue, ou le serveur hôte que la machine virtuelle protégée s’exécute sur échoue, basculer avec Azure Site Recovery Services conserve la continuité d’activité jusqu'à ce que l’ordinateur virtuel local sur serveur hôte est réparé et disponible. 

**Fonctionnement** --Azure Site Recovery Services proposés dans Microsoft Azure, permet une réplication en temps réel de vos ordinateurs virtuels (VM) à un coffre de sauvegarde dans Azure. Dans le cas où votre serveur ou un site tombe en panne en raison d’une panne matérielle ou autre, vous pouvez basculement avec Azure Site Recovery Services afin que l’image d’ordinateur virtuel stocké dans le coffre de sauvegarde est approvisionné en tant qu’un ordinateur virtuel en cours d’exécution dans Azure. Combiné à un réseau virtuel Azure, client PC précédemment connecté au serveur local en toute transparence se connectera au serveur en cours d’exécution dans Azure.     
                                                                                                                                                                                                                                                                                                               

## [<a name="integration-with-azure-virtual-network"></a>Intégration avec le réseau virtuel Azure](azure-virtual-network-integration.md)

**Action**--lorsque les entreprises mettent leur moyen de cloud computing, ils rarement déplacent toutes leurs ressources à la fois. Au lieu de cela, ils déplacement des ressources vers le nuage et conserver certaines localement. De cette façon, il est facile de transférer une organisation dans le nuage en étapes au fil du temps. Intégration de réseau virtuelle Azure fournit l’infrastructure réseau qui rend ce processus transparente et facile à gérer.

**Fonctionnement** : un réseau virtuel Azure est un service proposé dans Microsoft Azure qui permet aux organisations de créer un point à point (P2P) ou de site (S2S) réseau privé virtuel qui rend les ressources qui sont en cours d’exécution dans Azure (par exemple, les ordinateurs virtuels et stockage) rechercher comme s’ils se trouvent sur le réseau local pour un accès transparent applications et des ressources.



##[<a name="support-for-larger-deployments"></a>Prise en charge des déploiements plus importants](support-for-larger-deployments.md) 

Certaines grandes entreprises de petite besoin de davantage de fonctionnalités et la capacité à mettre en œuvre efficacement de Windows Server Essentials. Windows Server 2016 Essentials fournit la facilité de gestion accrue des domaines, les utilisateurs et périphériques en ajoutant la prise en charge des déploiements plus importants avec:                                                                                                                                                                                                 

 - plusieurs domaines
 - plusieurs contrôleurs de domaine                                                                                                                                                                                                                                        
 - possibilité de spécifier un contrôleur de domaine désigné                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Voir aussi
--------

[Prise en main WindowsServerEssentials](get-started.md)
