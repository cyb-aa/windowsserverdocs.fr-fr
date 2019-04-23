---
title: Vue d'ensemble de la migration dynamique
description: Présente la fonctionnalité de migration en direct dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
author: johncslack
ms.author: joslack
ms.date: 06/27/2017
ms.openlocfilehash: 2bbe897ffb8b200a72fac5a662e518d4a4be1131
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887850"
---
# <a name="live-migration-overview"></a>Vue d'ensemble de la migration dynamique

Migration dynamique est une fonctionnalité de Hyper-V dans Windows Server.  Il vous permet de déplacer en toute transparence des Machines virtuelles en cours d’exécution d’un hôte Hyper-V vers un autre sans indisponibilité perceptible.  Le principal avantage de la migration dynamique est flexible ; les Machines virtuelles en cours d’exécution ne sont pas liés à un seul ordinateur hôte.  Ainsi, les actions, telles que le drainage d’un hôte spécifique de Machines virtuelles avant la désactivation ou la mise à niveau.  En association avec le Clustering de basculement Windows, migration dynamique permet la création de haute disponibilité et les pannes systèmes à tolérance de panne. 

## <a name="related-technologies-and-documentation"></a>Documentation et les Technologies associées

Migration dynamique est souvent utilisée conjointement avec quelques technologies associées telles que le Clustering de basculement et de System Center Virtual Machine Manager.  Si vous utilisez la Migration dynamique via ces technologies, ici sont des pointeurs vers leur documentation la plus récente :
* [Le Clustering de basculement](../../../failover-clustering/failover-clustering-overview.md) (Windows Server 2016) 
* [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/) (System Center 2016) 

Si vous utilisez des versions antérieures de Windows Server, ou que vous avez besoin de plus d’informations sur les fonctionnalités introduites dans les versions antérieures de Windows Server, ici sont des pointeurs vers la documentation de l’historique : 
* [Migration dynamique](https://technet.microsoft.com/library/ee815293(v=ws.10).aspx) (Windows Server 2008 R2)  
* [Migration dynamique](https://technet.microsoft.com/library/hh831435(v=ws.11).aspx) (Windows Server 2012 R2) 
* [Le Clustering de basculement](https://technet.microsoft.com/library/hh831579(v=ws.11).aspx) (Windows Server 2012 R2)
* [Le Clustering de basculement](https://technet.microsoft.com/library/ff182338(v=ws.10).aspx) (Windows Server 2008 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/gg610610.aspx) (System Center 2012 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2)

## <a name="live-migration-in-windows-server-2016"></a>Migration en direct dans Windows Server 2016

Dans Windows Server 2016, il existe moins de restrictions sur le déploiement de la migration dynamique.  Il fonctionne maintenant sans Clustering de basculement.  Autres fonctionnalités restent inchangées depuis les versions précédentes de la Migration en direct.  Pour plus d’informations sur la configuration et à l’aide de la migration dynamique sans Clustering de basculement : 
* [Configurer des hôtes pour la migration dynamique sans Clustering de basculement](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [Migration dynamique sans Clustering de basculement permet de déplacer un ordinateur virtuel](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)