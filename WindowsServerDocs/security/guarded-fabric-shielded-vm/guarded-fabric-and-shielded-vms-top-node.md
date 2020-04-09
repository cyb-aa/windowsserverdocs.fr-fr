---
title: Structure protégée et machines virtuelles dotées d’une protection maximale
ms.prod: windows-server
ms.topic: article
ms.assetid: 5c7ada81-2d97-41d4-87cf-1a7ccf06cd20
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9e76b3081438ae38c6b83b7cdd179d47b1e21a70
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856912"
---
# <a name="guarded-fabric-and-shielded-vms"></a>Structure protégée et machines virtuelles dotées d’une protection maximale

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

L’un des objectifs les plus importants de la fourniture d’un environnement hébergé est de garantir la sécurité des ordinateurs virtuels en cours d’exécution dans l’environnement. En tant que fournisseur de services cloud ou administrateur d’un cloud privé d’entreprise, vous pouvez utiliser une structure protégée pour offrir un environnement plus sécurisé pour les ordinateurs virtuels. Une structure protégée se compose d’un Service Guardian hôte (HGS), généralement, un cluster de trois nœuds, d’un ou de plusieurs hôtes protégés et d’un ensemble d’ordinateurs virtuels.

> [!IMPORTANT]
> Vérifiez que vous avez installé la dernière mise à jour cumulative avant de déployer des machines virtuelles protégées en production.

## <a name="videos-blog-and-overview-topic-about-guarded-fabrics-and-shielded-vms"></a>Vidéos, blog et rubrique de présentation sur les infrastructures protégées et les machines virtuelles protégées

- Vidéo : [Comment protéger votre structure de virtualisation contre les menaces internes avec Windows Server 2019](https://myignite.techcommunity.microsoft.com/sessions/64690)
- Vidéo : [Présentation des machines virtuelles protégées dans Windows Server 2016](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- Vidéo : [Explorez les machines virtuelles protégées avec Windows Server 2016 Hyper-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
- Vidéo : [déploiement de machines virtuelles protégées et d’une infrastructure protégée avec Windows Server 2016](https://mva.microsoft.com/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)
- Blog : [blog sur la sécurité du Cloud privé et du centre de](https://blogs.technet.microsoft.com/datacentersecurity/) connaissances
- Vue d’ensemble : [vue d’ensemble de l’infrastructure protégée et des machines virtuelles](Guarded-Fabric-and-Shielded-VMs.md) protégées

## <a name="planning-topics"></a>Rubriques de planification

- [Guide de planification pour les hébergeurs](guarded-fabric-planning-for-hosters.md)
- [Guide de planification pour les locataires](guarded-fabric-shielded-vm-planning-for-tenants.md)

## <a name="deployment-topics"></a>Rubriques relatives au déploiement

- [Guide de déploiement](guarded-fabric-deploying-hgs-overview.md)
    - [Démarrage rapide](guarded-fabric-deployment-overview.md)
    - [Déployer SGH](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)
    - [Déployer des hôtes protégés](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)
        - [Configuration du DNS de l’infrastructure pour les hôtes qui deviendront des hôtes service Guardian](guarded-fabric-configuring-fabric-dns.md)
        - [Déployer un hôte service Guardian à l’aide du mode AD](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)
        - [Déployer un hôte service Guardian en mode TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)
        - [Confirmer que les hôtes protégés peuvent attester](guarded-fabric-confirm-hosts-can-attest-successfully.md)
        - [Machines virtuelles protégées-le fournisseur de services d’hébergement déploie des hôtes service Guardian dans VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts)
    - [Déployer des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
        - [Créer un modèle de machine virtuelle protégée](guarded-fabric-create-a-shielded-vm-template.md)
        - [Préparer un disque dur virtuel d’assistance de protection de machine virtuelle](guarded-fabric-vm-shielding-helper-vhd.md)
        - [Configurer Windows Azure Pack](guarded-fabric-hoster-sets-up-windows-azure-pack.md)
        - [Créer un fichier de données de protection](guarded-fabric-tenant-creates-shielding-data.md)
        - [Déployer une machine virtuelle protégée à l’aide de Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)
        - [Déployer une machine virtuelle protégée à l’aide de Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="operations-and-management-topic"></a>Rubrique sur les opérations et la gestion

- [Gestion du service Guardian hôte](guarded-fabric-manage-hgs.md)
