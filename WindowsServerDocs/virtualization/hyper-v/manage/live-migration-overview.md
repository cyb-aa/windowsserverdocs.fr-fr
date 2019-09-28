---
title: Vue d'ensemble de la migration dynamique
description: Donne une vue d’ensemble de la fonctionnalité de migration dynamique dans Windows Server 2016.
ms.prod: windows-server
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
author: johncslack
ms.author: joslack
ms.date: 06/27/2017
ms.openlocfilehash: ba239343728502c4928b86a2a0d4a3db5c36e7f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392544"
---
# <a name="live-migration-overview"></a>Vue d'ensemble de la migration dynamique

La migration dynamique est une fonctionnalité Hyper-V dans Windows Server.  Elle vous permet de déplacer de façon transparente des ordinateurs virtuels en cours d’exécution d’un ordinateur hôte Hyper-V à un autre sans temps mort.  Le principal avantage de la migration dynamique est la flexibilité ; les machines virtuelles en cours d’exécution ne sont pas liées à un seul ordinateur hôte.  Cela permet d’effectuer des actions telles que le drainage d’un hôte spécifique d’ordinateurs virtuels avant de le désactiver ou de le mettre à niveau.  Lorsqu’elle est associée au clustering de basculement Windows, la migration dynamique permet de créer des systèmes à tolérance de panne et à haute disponibilité. 

## <a name="related-technologies-and-documentation"></a>Technologies et documentation associées

La migration dynamique est souvent utilisée conjointement avec quelques technologies associées telles que le clustering de basculement et les System Center Virtual Machine Manager.  Si vous utilisez Migration dynamique à l’aide de ces technologies, vous trouverez ici des pointeurs vers la documentation la plus récente :
* [Clustering de basculement](../../../failover-clustering/failover-clustering-overview.md) (Windows Server 2016) 
* [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/) (System Center 2016) 

Si vous utilisez des versions antérieures de Windows Server, ou si vous avez besoin de détails sur les fonctionnalités introduites dans les versions antérieures de Windows Server, Voici des pointeurs vers la documentation historique : 
* [Migration dynamique](https://technet.microsoft.com/library/ee815293(v=ws.10).aspx) (Windows Server 2008 R2)  
* [Migration dynamique](https://technet.microsoft.com/library/hh831435(v=ws.11).aspx) (Windows Server 2012 R2) 
* [Clustering de basculement](https://technet.microsoft.com/library/hh831579(v=ws.11).aspx) (Windows Server 2012 R2)
* [Clustering de basculement](https://technet.microsoft.com/library/ff182338(v=ws.10).aspx) (Windows Server 2008 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/gg610610.aspx) (System Center 2012 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2)

## <a name="live-migration-in-windows-server-2016"></a>Migration dynamique dans Windows Server 2016

Dans Windows Server 2016, il y a moins de restrictions sur le déploiement d’une migration dynamique.  Il fonctionne désormais sans clustering de basculement.  Les autres fonctionnalités restent inchangées par rapport aux versions précédentes de Migration dynamique.  Pour plus d’informations sur la configuration et l’utilisation de la migration dynamique sans clustering de basculement : 
* [Configurer des hôtes pour la migration dynamique sans clustering de basculement](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [Utiliser la migration dynamique sans clustering de basculement pour déplacer un ordinateur virtuel](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)