---
title: Structure protégée et machines virtuelles dotées d’une protection maximale
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5c7ada81-2d97-41d4-87cf-1a7ccf06cd20
manager: dongill
author: rpsqrd
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c2c16574439569eeb1181977f23bae238f43865c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857430"
---
# <a name="guarded-fabric-and-shielded-vms"></a>Structure protégée et machines virtuelles dotées d’une protection maximale

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Un des objectifs plus importants de fournir un environnement hébergé consiste à garantir la sécurité des machines virtuelles en cours d’exécution dans l’environnement. En tant que fournisseur de services cloud ou administrateur d’un cloud privé d’entreprise, vous pouvez utiliser une structure protégée pour offrir un environnement plus sécurisé pour les ordinateurs virtuels. Une structure protégée se compose d’un Service Guardian hôte (HGS), généralement, un cluster de trois nœuds, d’un ou de plusieurs hôtes protégés et d’un ensemble d’ordinateurs virtuels.

> [!IMPORTANT]
> Vérifiez que vous avez installé la mise à jour cumulative la plus récente avant de déployer des machines virtuelles protégées en production.

## <a name="videos-blog-and-overview-topic-about-guarded-fabrics-and-shielded-vms"></a>Vidéos, blog et rubrique de présentation sur service Guardian infrastructures et des machines virtuelles protégées

- Vidéo : [Comment protéger votre structure de virtualisation contre les menaces internes avec Windows Server 2019](https://myignite.techcommunity.microsoft.com/sessions/64690)
- Vidéo : [Introduction aux Machines virtuelles protégées dans Windows Server 2016](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- Vidéo : [Plongez dans les machines virtuelles protégées avec Windows Server 2016 Hyper-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
- Vidéo : [Déploiement de machines virtuelles protégées et une infrastructure protégée avec Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)
- Blog : [Centre de données et le Blog de sécurité de Cloud privé](https://blogs.technet.microsoft.com/datacentersecurity/)
- Vue d'ensemble : [Structure protégée et vue d’ensemble des machines virtuelles protégée](Guarded-Fabric-and-Shielded-VMs.md)

## <a name="planning-topics"></a>Rubriques de planification

- [Guide de planification pour les hébergeurs](guarded-fabric-planning-for-hosters.md)
- [Guide de planification pour les locataires](guarded-fabric-shielded-vm-planning-for-tenants.md)

## <a name="deployment-topics"></a>Rubriques relatives au déploiement

- [Guide de déploiement](guarded-fabric-deploying-hgs-overview.md)
    - [Guide de démarrage rapide](guarded-fabric-deployment-overview.md)
    - [Déployer le service SGH](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)
    - [Déployer des hôtes service Guardian](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)
        - [Configuration de la structure DNS pour les hôtes qui deviendront des hôtes service Guardian](guarded-fabric-configuring-fabric-dns.md)
        - [Déployer un hôte service Guardian à l’aide du mode AD](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)
        - [Déployer un hôte service Guardian à l’aide du mode de module de plateforme sécurisée](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)
        - [Confirmer le diront hôtes service Guardian](guarded-fabric-confirm-hosts-can-attest-successfully.md)
        - [Machines virtuelles protégées - fournisseur de services déploie des hôtes service Guardian dans VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts)
    - [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
        - [Créer un modèle de machine virtuelle protégée](guarded-fabric-create-a-shielded-vm-template.md)
        - [Préparez un protection de machine virtuelle VHD d’assistance](guarded-fabric-vm-shielding-helper-vhd.md)
        - [Configurer Windows Azure Pack](guarded-fabric-hoster-sets-up-windows-azure-pack.md)
        - [Créer un fichier de données de protection](guarded-fabric-tenant-creates-shielding-data.md)
        - [Déployer une machine virtuelle protégée à l’aide de Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)
        - [Déployer une machine virtuelle protégée à l’aide de Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="operations-and-management-topic"></a>Opérations et la rubrique Gestion

- [Gérer le Service Guardian hôte](guarded-fabric-manage-hgs.md)
