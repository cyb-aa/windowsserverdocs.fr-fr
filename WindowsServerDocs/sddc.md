---
title: Windows Server Software-Defined Datacenter
description: Vue d’ensemble de Windows Server SDDC
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: SDDC
ms.tgt_pltfrm: na
ms.topic: get-started article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/04/2019
ms.localizationpriority: medium
ms.openlocfilehash: 6490bd9a6cb7b305ba9746a357a8c909c7b84555
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950466"
---
# <a name="windows-server-software-defined-datacenter"></a>Windows Server Software-Defined Datacenter

>S'applique à : Windows Server 2019, Windows Server 2016

![](media/sddc/heading.png)

## <a name="what-is-windows-server-software-defined-datacenter"></a>Qu’est-ce que le centre de données à définition logicielle (SDDC) Windows Server ?

Le centre de données à définition logicielle (SDDC) est un terme courant du secteur qui fait généralement référence à un centre de données dont l’infrastructure est entièrement virtualisée. La virtualisation est la clé, ce qui signifie simplement que le matériel et les logiciels du centre de données s’étendent au-delà du ratio 1 à 1 classique. Avec un matériel émulant un hyperviseur logiciel, les systèmes d’exploitation et les applications peuvent être extraits du matériel physique et multipliés afin de former des pools de ressources élastiques de processeurs, de mémoire, d’E/S et de réseaux.
 
L’implémentation du SDDC de Microsoft comprend les technologies Windows Server détaillées dans cet article. Pour commencer, il y a l’hyperviseur Hyper-V qui fournit la plateforme de virtualisation sur laquelle reposent la mise en réseau et le stockage. Des technologies de sécurité, développées pour faire face aux exigences uniques d’une infrastructure virtualisée, bloquent et atténuent les menaces internes et externes. Avec l’intégration de PowerShell à Windows Server et l’ajout de [System Center](https://docs.microsoft.com/system-center/) et/ou [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview), vous pouvez programmer et automatiser le provisionnement, le déploiement, la configuration et la gestion.

Les technologies intégrées à Windows Server et System Center constituent les principales composantes de l’expérience Windows Server SDDC. Cependant, même s’il s’agit d’une plateforme virtualisée, elle nécessite néanmoins le matériel sous-jacent approprié. Les partenaires Microsoft qui participent aux programmes **Solutions WSSD (Windows Server Software-Defined)** et **Solutions Azure Stack HCI** peuvent aider votre entreprise à acquérir le matériel adéquat et à le rendre opérationnel dès le premier jour.

![](media/sddc/video.png) **[Regarder une vidéo pour en savoir plus sur le SDDC de Microsoft](https://mva.microsoft.com/training-courses/whats-new-in-windows-server-2016-16457?l=YcsJR6sXC_1006218965)**

![](media/sddc/poster-ico.png) **[Télécharger un fichier .pdf de cette page au format d’une affiche](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs/media/sddc/sddc_poster_0801417_ANSI-E.pdf)**

![](media/sddc/spacer1.png)<a href="https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs//media/sddc/sddc_poster_0801417_ANSI-E.pdf"><img src="media/sddc/poster.png"></a>

## <a name="azure-stack-hci-solutions"></a>Solutions Azure Stack HCI

Quand il s’agit de créer un centre de données à définition logicielle Windows Server, le choix d’une infrastructure matérielle adaptée constitue la première conditionnelle essentielle du succès. C’est la raison pour laquelle nous avons conclu un partenariat avec 15 partenaires pour créer des conceptions et des bonnes pratiques SDDC validées par Microsoft pour le déploiement.

Les partenaires Microsoft offrent toute une gamme de solutions qui fonctionnent avec Windows Server 2019 via le programme Azure Stack HCI et avec Windows Server 2016 via le programme à définition logicielle Windows Server (WSSD) pour fournir une infrastructure réseau et de stockage hautement performante et hyperconvergée. Les solutions hyperconvergées regroupent le calcul, le stockage et le réseau sur les serveurs et les composants standard du secteur pour améliorer l’intelligence et la gestion des centres de données.

![](media/sddc/learn.png) **[En savoir plus sur les solutions Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci)**

![](media/sddc/learn.png) **[En savoir plus sur les solutions WSSD](https://www.microsoft.com/cloud-platform/software-defined-datacenter)**

## <a name="windows-server-virtualized-technologies"></a>Technologies virtualisées Windows Server ##

Le reste de cette rubrique liste les technologies Windows Server SDDC et propose des liens vers la documentation de chacune d’elles. Les technologies sont listées dans le tableau ci-après :

![](media/sddc/table.png)

![](media/sddc/virtualize.png)

### <a name="windows-server-hyper-converged"></a>Windows Server, hyperconvergé

Les technologies de virtualisation Windows Server incluent des mises à jour vers Hyper-V, le commutateur virtuel Hyper-V, la Structure Service Guardian et les machines virtuelles dotées d’une protection maximale, qui améliorent la sécurité, la scalabilité et la fiabilité. Les mises à jour vers le clustering de basculement, le réseau et le stockage facilitent encore davantage le déploiement et la gestion de ces technologies quand elles sont utilisées avec Hyper-V.

![](media/sddc/spacer1.png)![](media/sddc/hyper-converged.png)

![](media/sddc/learn.png) **[En savoir plus sur Windows Server, hyperconvergé](https://docs.microsoft.com/windows-server/get-started/what-s-new-in-windows-server-2016#computevirtualizationvirtualizationmd)**

### <a name="hyper-v-hypervisor"></a>Hyperviseur Hyper-V

Hyper-V est une technologie de virtualisation basée sur un hyperviseur pour Windows. L’hyperviseur est essentiel à la virtualisation. Cette plateforme de virtualisation spécifique du processeur permet à plusieurs systèmes d’exploitation isolés de partager une seule plateforme matérielle.

![](media/sddc/spacer1.png)![](media/sddc/hypervisor.png)

![](media/sddc/learn.png) **[En savoir plus sur l’hyperviseur Hyper-V](https://www.microsoft.com/cloud-platform/server-virtualization)**

### <a name="guest-clustering-with-shared-vhdx"></a>Clustering invité avec VHDX partagé

![](media/sddc/virtualize-line.png)

Flexible, sécurisé et indépendant de la topologie de stockage sous-jacente, le VHDX partagé vous dispense de présenter le stockage physique sous-jacent à un système d’exploitation invité. Le nouveau VHDX partagé prend en charge le redimensionnement en ligne.

![](media/sddc/spacer1.png)![](media/sddc/cluster.png)

- Le VHDX partagé peut résider sur un volume partagé de cluster (CSV) de stockage de bloc ou sur un stockage basé sur des fichiers SMB.
- Protégé : le VHDX partagé prend en charge Réplica Hyper-V et la sauvegarde au niveau de l’hôte.

![](media/sddc/learn.png) **[En savoir plus sur le clustering invité avec le VHDX partagé](https://technet.microsoft.com/library/dn281956(v=ws.11).aspx)**

### <a name="hyper-v-replica"></a>Réplication Hyper-V

![](media/sddc/virtualize-line.png)

Réplication intégrée des machines virtuelles logicielles sur le réseau avec des certificats. N’est liée à aucun serveur, réseau ou stockage matériel de l’un des deux sites.

![](media/sddc/spacer1.png)![](media/sddc/replica.png)

Aucune autre technologie de réplication de machines virtuelles n’est nécessaire, ce qui permet de réduire les coûts.
- Gère automatiquement la migration dynamique.
- Configuration et gestion simples via le gestionnaire Hyper-V, PowerShell ou avec Azure Site Recovery.

![](media/sddc/learn.png) **[En savoir plus sur Réplica Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)**

![](media/sddc/networking.png)

### <a name="network-controller"></a>Contrôleur réseau

![](media/sddc/networking-line.png)

Il s’agit d’une fonction d’automatisation programmable et centralisée pour gérer, configurer, superviser et dépanner l’infrastructure réseau virtuelle et physique de votre centre de données.

![](media/sddc/spacer1.png)![](media/sddc/netcontroller.png)

Les administrateurs utilisent un outil de gestion qui interagit directement avec le contrôleur de réseau. Le contrôleur de réseau fournit des informations sur l’infrastructure réseau (infrastructure virtuelle et infrastructure physique comprises) à l’outil de gestion.

![](media/sddc/learn.png) **[En savoir plus sur le contrôleur de réseau](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-controller/network-controller)**

### <a name="datacenter-firewall"></a>Pare-feu de centre de données

![](media/sddc/networking-line.png)

Quand il est déployé et proposé en tant que service, les administrateurs clients peuvent installer et configurer des stratégies de pare-feu pour protéger les réseaux virtuels contre le trafic indésirable en provenance d’Internet et des réseaux intranet.

![](media/sddc/spacer1.png)![](media/sddc/firewall.png)

L’administrateur du fournisseur de services ou l’administrateur client peut gérer les stratégies de pare-feu de centre de données via le contrôleur de réseau.

![](media/sddc/learn.png) **[En savoir plus sur le pare-feu de centre de données](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-function-virtualization/datacenter-firewall-overview)**

### <a name="switch-embedded-teaming"></a>SET (Switch Embedded Teaming)

![](media/sddc/networking-line.png)

SET est une autre solution d’association de cartes réseau que vous pouvez utiliser dans des environnements qui incluent Hyper-V et la pile [SDN (Software-Defined Networking)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking).

![](media/sddc/spacer1.png)![](media/sddc/teaming.png)

![](media/sddc/learn.png) **[En savoir plus sur SET (Switch Embedded Teaming)](https://docs.microsoft.com/windows-server/networking/sdn/technologies/set-for-sdn)**

### <a name="software-load-balancing"></a>Équilibrage de charge logicielle

![](media/sddc/networking-line.png)

L’équilibrage de charge logicielle (SLB) permet à plusieurs serveurs d’héberger une même charge de travail, ce qui assure une disponibilité et une scalabilité de haut niveau. Effectuez le scale-out de vos capacités d’équilibrage de charge au moyen de machines virtuelles SLB sur les mêmes serveurs Hyper-V que ceux utilisés pour vos autres charges de travail de machines virtuelles. SLB prend en charge la création et la suppression rapides de points de terminaison d’équilibrage de charge pour les opérations de fournisseur de services cloud. Par ailleurs, il prend en charge des dizaines de gigaoctets par cluster, offre un modèle de provisionnement simple et peut facilement faire l’objet d’un scale-out ou d’un scale-in.

![](media/sddc/spacer1.png)![](media/sddc/balancer.png)

![](media/sddc/learn.png) **[En savoir plus sur l’équilibrage de charge logicielle](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn)**


![](media/sddc/storage.png)

### <a name="storage-spaces-direct"></a>Espaces de stockage directs

![](media/sddc/storage-line.png)

Les espaces de stockage direct reposent sur des serveurs standard dotés de disques locaux pour offrir un stockage à définition logicielle hautement disponible et scalable, pour un coût nettement inférieur à celui des baies SAN ou NAS traditionnelles. Son architecture simplifie considérablement l’approvisionnement et le déploiement.

![Sur chaque nœud, les lecteurs locaux sont mis en pool au niveau du cluster par les espaces de stockage direct, puis les machines virtuelles y accèdent via les volumes partagés de cluster](media/sddc/spacer1.png)![](media/sddc/ssd.png)

Les espaces de stockage direct introduisent le nouveau bus de stockage virtuel Software Storage Bus et tirent parti des nombreuses fonctionnalités de Windows Server que vous connaissez déjà, telles que le clustering de basculement, les volumes partagés de cluster, le protocole SMB3 (Server Message Block) et les espaces de stockage.

![](media/sddc/learn.png) **[En savoir plus sur les espaces de stockage direct](storage/storage-spaces/storage-spaces-direct-overview.md)**
### <a name="storage-quality-of-service"></a>Qualité de service de stockage ###

![](media/sddc/storage-line.png)

Supervisez et gérez de manière centralisée les performances de stockage pour les machines virtuelles en utilisant Hyper-V et les rôles Serveur de fichiers avec montée en puissance parallèle, en améliorant l’équité des ressources de stockage entre plusieurs machines virtuelles.

![](media/sddc/spacer1.png)![](media/sddc/qos.png)

La qualité de service de stockage est intégrée à la solution de stockage à définition logicielle Microsoft fournie par le serveur de fichiers avec montée en puissance parallèle et Hyper-V avec le protocole SMB3. Un nouveau gestionnaire de stratégie permet de superviser de manière centralisée les performances de stockage.

![](media/sddc/learn.png) **[En savoir plus sur la qualité de service de stockage](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview)**

### <a name="storage-replica"></a>Réplica de stockage


![](media/sddc/storage-line.png)

La préparation aux situations d’urgence et de reprise d’activité après sinistre garantit l’absence totale de perte de données, avec la possibilité de protéger de façon synchrone les données dans différents racks, étages, bâtiments, campus, villes et pays, avec une utilisation plus efficace de plusieurs centres de données.

![](media/sddc/spacer1.png)
![](media/sddc/storage-replica.png)

Réplication synchrone

1. L’application écrit des données
2. Les données du journal sont écrites et les données sont répliquées sur le site distant
3. Les données du journal sont écrites sur le site distant
4. Accusé de réception du site distant
5. Réception de l’écriture d’application confirmée

t & t1 : données vidées sur le volume, journaux toujours écrits en continu

![](media/sddc/learn.png) **[En savoir plus sur le réplica de stockage](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-overview)**

![](media/sddc/security.png)

### <a name="guarded-fabric"></a>Structure Service Guardian

![](media/sddc/security-line.png)

En tant que fournisseur de services cloud ou administrateur d’un cloud privé d’entreprise, vous pouvez utiliser une structure protégée pour offrir un environnement plus sécurisé pour les machines virtuelles. Une structure Service Guardian se compose d’un Service Guardian hôte (HGS) généralement constitué d’un cluster de trois nœuds, d’un ou plusieurs hôtes protégés et d’un ensemble de machines virtuelles dotées d’une protection maximale.

![](media/sddc/spacer1.png)![](media/sddc/guarded-fabric.png)

![](media/sddc/learn.png) **[En savoir plus sur la structure Service Guardian](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)**

### <a name="shielded-vms"></a>Machines virtuelles dotées d’une protection maximale

![](media/sddc/security-line.png)

Les données et l’état d’une machine virtuelle dotée d’une protection maximale sont protégés contre l’inspection, le vol et la falsification par des administrateurs de centre de données ou des programmes malveillants.

![](media/sddc/spacer1.png)![](media/sddc/shielded.png)

- Les machines virtuelles dotées d’une protection maximale s’exécutent uniquement dans les structures désignées comme étant propriétaires des machines virtuelles.
- Ces machines virtuelles étant chiffrées par BitLocker ou par d’autres moyens, seuls les propriétaires désignés peuvent les exécuter.
- Les machines virtuelles en cours d’exécution peuvent être converties en machines virtuelles protégées.

![](media/sddc/learn.png) **[En savoir plus sur les machines virtuelles dotées d’une protection maximale](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)**

### <a name="host-guardian-service"></a>Service Guardian hôte

![](media/sddc/security-line.png)

Le Service Guardian hôte contient les clés pour les infrastructures légitimes, ainsi que pour les machines virtuelles chiffrées.

![](media/sddc/spacer1.png)![](media/sddc/guardian.png)

![](media/sddc/learn.png) **[En savoir plus sur le Service Guardian hôte](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-hgs)**

### <a name="device-health-attestation"></a>Attestation d’intégrité de l’appareil

![](media/sddc/security-line.png)

L’attestation permet aux grandes entreprises d’élever le niveau de sécurité de leur organisation pour bénéficier d’une sécurité attestée et supervisée du matériel, avec un impact minime ou nul sur les coûts d’exploitation.


![](media/sddc/spacer1.png)![](media/sddc/attestation.png)


Le mode de matériel approuvé, présenté ci-dessus, offre le niveau d’assurance le plus élevé, avec l’approbation associée au matériel TPM v2.0 et la conformité à la stratégie d’intégrité du code pour la libération des clés.


![](media/sddc/learn.png) **[En savoir plus sur l’attestation d’intégrité des appareils](https://docs.microsoft.com/windows-server/security/device-health-attestation)**

![](media/sddc/management.png)

### <a name="powershell-desired-state-configuration"></a>PowerShell Desired State Configuration

![](media/sddc/management-line.png)

Windows PowerShell Desired State Configuration est une plateforme de gestion de configuration intégrée à Windows qui est basée sur des standards ouverts. DSC est suffisamment flexible pour fonctionner de manière fiable et cohérente à chaque étape du cycle de vie de déploiement (développement, test, préproduction, production), ainsi qu’au cours d’une montée en puissance parallèle.

![](media/sddc/spacer1.png)![](media/sddc/dsc.png)

DSC prend en charge les « déploiements continus », ce qui vous permet de déployer sans cesse des configurations sans rien perturber.

-  Pour assurer des déploiements plus rapides, les configurations DSC appliquent uniquement les paramètres dont les valeurs d’origine ont changé.
-  DSC peut être utilisé en local ou dans un environnement de cloud public ou privé.
-  Vous pouvez intégrer DSC à n’importe quelle solution Microsoft ou tierce, tant que vous exécutez un script PowerShell sur le système cible.

![](media/sddc/learn.png) **[En savoir plus sur PowerShell DSC](https://docs.microsoft.com/powershell/dsc/overview)**


### <a name="system-center-vmm"></a>System Center VMM

![](media/sddc/management-line.png)

Virtual Machine Manager, qui fait partie de la suite System Center, permet de configurer, gérer et transformer les centres de données traditionnels pour offrir une expérience de gestion unifiée pour les ressources locales, le fournisseur de services et le cloud Azure.

![](media/sddc/spacer1.png)![](media/sddc/vmm.png)

- Centre de données : configurer et gérer les composants d’un centre de données en tant qu’infrastructure unique dans VMM. 
- Hôte de virtualisation : VMM peut ajouter, provisionner et gérer des hôtes et des clusters de virtualisation Hyper-V et VMware.
- Réseaux : VMM assure la virtualisation de réseau et permet notamment de créer et gérer les réseaux virtuels et les passerelles réseau. 
- Stockage : VMM peut découvrir, classifier, provisionner, allouer et affecter un stockage local et distant.

![](media/sddc/learn.png) **[En savoir plus sur System Center VMM](https://docs.microsoft.com/system-center/vmm/)**

### <a name="windows-admin-center"></a>Windows Admin Center

![](media/sddc/management-line.png)

Windows Admin Center est un ensemble d’outils de gestion basé sur un navigateur et déployé localement qui permet d’administrer en local les serveurs Windows Server sans dépendance vis-à-vis d’Azure ou du cloud. Windows Admin Center offre aux administrateurs informatiques un contrôle total sur tous les aspects de leur infrastructure de serveurs. Il s’avère particulièrement utile pour la gestion des réseaux privés qui ne sont pas connectés à Internet.

![](media/sddc/spacer1.png)![](media/sddc/architecture.png)

La publication du serveur web dans DNS et la configuration du pare-feu d’entreprise vous permettent d’accéder à Windows Admin Center à partir de l’Internet public. Vous pouvez ainsi vous connecter à vos serveurs et les gérer de n’importe où avec Microsoft Edge ou Google Chrome.

![](media/sddc/learn.png) **[En savoir plus sur Windows Admin Center](manage/windows-admin-center/overview.md)**
