---
title: Centre de données défini par logiciel (SDDC) WindowsServer
description: Présentation du centre de données défini par logiciel (SDDC) WindowsServer
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: SDDC
ms.tgt_pltfrm: na
ms.topic: get-started article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/28/2017
ms.localizationpriority: medium
ms.openlocfilehash: 4c8c530568d7f336ae2bd4981c02093fe580d9b7
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121458"
---
# Centre de données défini par logiciel (SDDC) WindowsServer

>S’applique à WindowsServer2016

![](media/sddc/heading.png)

## Qu’est-ce que le centre de données défini par logiciel (SDDC) WindowsServer? ##

Le centre de données défini par logiciel (SDDC) est un terme courant lié au secteur d’activité qui fait généralement référence à un centre de données dont l’infrastructure est entièrement virtualisée. La virtualisation est l’élément clé, et cela signifie simplement que le matériel et les logiciels du centre de données s’étendent au-delà du ratio 1à1 classique. Avec un matériel émulant un hyperviseur logiciel, les systèmes d’exploitation et les applications peuvent être extraits du matériel physique et multipliés afin de former des pools de ressources élastiques de processeurs, de mémoire, d’E/S et de réseaux.
 
L’implémentation du SDDC de Microsoft est constituée des technologies WindowsServer détaillées dans cet article. Elle commence avec l’hyperviseur Hyper-V qui fournit la plateforme de virtualisation sur laquelle reposent la mise en réseau et le stockage. Des technologies de sécurité, développées pour relever les défis uniques que présente une infrastructure virtualisée, réduisent les menaces internes et externes. Avec l’intégration de PowerShell à WindowsServer et l’ajout de [SystemCenter](https://docs.microsoft.com/system-center/) et/ou [OperationsManagementSuite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview), vous pouvez programmer et automatiser la configuration, le déploiement, la configuration et la gestion.

Les technologies intégrées à WindowsServer et SystemCenter constituent les principaux blocs de construction de l’expérience SDDC WindowsServer. Toutefois, même s’il s’agit d’une plateforme virtualisée, elle requiert néanmoins le matériel sous-jacent approprié. Les partenaires Microsoft qui participent au programme **Solutions WSSD (WindowsServerSoftware-Defined)** peuvent aider votre entreprise à acquérir le matériel approprié et à le rendre opérationnel dès le premier jour.

![](media/sddc/video.png)**[Regarder une vidéo pour en savoir plus sur le SDDC de Microsoft](https://mva.microsoft.com/en-US/training-courses/whats-new-in-windows-server-2016-16457?l=YcsJR6sXC_1006218965)**

![](media/sddc/poster-ico.png)**[Télécharger un fichier .pdf de cette page sous forme de poster](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs/media/sddc/sddc_poster_0801417_ANSI-E.pdf)**


![](media/sddc/spacer1.png)<a href="https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs//media/sddc/sddc_poster_0801417_ANSI-E.pdf"><img src="media/sddc/poster.png"></a>


## Solutions de centre de données défini par logiciel WindowsServer (WSSD) ##
Lors de la création de votre centre de données défini par logiciel WindowsServer, le choix de l’infrastructure matérielle appropriée constitue la première étape essentielle pour la réussite. C’est pourquoi nous avons conclu un partenariat avec **DataON**, **Fujitsu**, **Lenovo**, **QCT**, **SuperMicro**, **HewlettPackardEnterprise** et **DellEMC** afin de créer des conceptions SDDC validées par Microsoft et les meilleures pratiques en matière de déploiement. Les partenaires Microsoft offrent une gamme de solutions de centre de données défini par logiciel WindowsServer (WSSD) qui fonctionnent avec WindowsServer2016 afin d’offrir une infrastructure réseau et de stockage hautement performante et hyperconvergée. Les solutions hyperconvergées regroupent le calcul, le stockage et la mise en réseau sur des serveurs et des composants standard de l’industrie pour une connaissance et un contrôle améliorés du centre de données.



![](media/sddc/learn.png)**[En savoir plus sur les solutions WSSD](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)**

## Technologies virtuelles WindowsServer ##

Le reste de cette rubrique répertorie les technologies SDDC WindowsServer et fournit des liens vers la documentation de chacune d’elles. Les technologies sont répertoriées dans le tableau ci-après:

![](media/sddc/table.png)

![](media/sddc/virtualize.png)

### WindowsServer, hyperconvergé ###

Les technologies de virtualisation WindowsServer incluent des mises à jour de Hyper-V, du commutateur virtuel Hyper-V, de Guarded Fabric et des ordinateurs virtuels protégés, améliorant de ce fait la sécurité, l’extensibilité et la fiabilité. Les mises à jour du clustering de basculement, de la mise en réseau et du stockage facilitent encore davantage le déploiement et la gestion de ces technologies lors de l’utilisation de Hyper-V.

![](media/sddc/spacer1.png)![](media/sddc/hyper-converged.png)

![](media/sddc/learn.png)**[En savoir plus sur WindowsServer, hyperconvergé](https://docs.microsoft.com/windows-server/get-started/what-s-new-in-windows-server-2016#computevirtualizationvirtualizationmd)**
 
### Hyperviseur Hyper-V ###

Hyper-V est une technologie de virtualisation basée sur un hyperviseur pour Windows. L’hyperviseur est essentiel à la virtualisation. Cette plateforme de virtualisation spécifique du processeur permet à plusieurs systèmes d’exploitation isolés de partager une seule plateforme matérielle.

![](media/sddc/spacer1.png)![](media/sddc/hypervisor.png)

![](media/sddc/learn.png)**[En savoir plus sur l’hyperviseur Hyper-V](https://www.microsoft.com/en-us/cloud-platform/server-virtualization)**

### Clustering invité avec VHDX partagé ###

![](media/sddc/virtualize-line.png)

Flexible, sécurisé et indépendant de la topologie de stockage sous-jacente, le stockage VHDX partagé supprime l’obligation de présenter le stockage physique sous-jacent à un système d’exploitation invité. Le nouveau VHDX partagé prend en charge la fonctionnalité de redimensionnement en ligne.

![](media/sddc/spacer1.png)![](media/sddc/cluster.png)

- Le VHDX partagé peut résider sur un volume partagé de cluster sur un stockage par blocs, ou sur le stockage basé sur des fichiersSMB.
- Protégé: VHDX partagé prend en charge RéplicaHyper-V et la sauvegarde au niveau de l’hôte.

![](media/sddc/learn.png)**[En savoir plus sur le clustering invité avec VHDX partagé](https://technet.microsoft.com/library/dn281956(v=ws.11).aspx)**

### RéplicaHyper-V ###

![](media/sddc/virtualize-line.png)

Réplication d’ordinateur virtuel intégrée, basée sur les logiciels, sur le réseau avec des certificats. N’est liée à aucun serveur, réseau ou matériel de stockage sur l’un des sites.

![](media/sddc/spacer1.png)![](media/sddc/replica.png)

Aucune autre technologie de réplication d’ordinateur virtuel n’est requise, ce qui permet de réduire les coûts.
- Gère automatiquement la migration dynamique.
- Configuration et gestion simples, par le biais du gestionnaire Hyper-V, de PowerShell ou d’AzureSiteRecovery.

![](media/sddc/learn.png)**[En savoir plus sur RéplicaHyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)**

![](media/sddc/networking.png)

### Contrôleur de réseau ###

![](media/sddc/networking-line.png)

Il s’agit d’une fonction d’automatisation programmable et centralisée pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données.

![](media/sddc/spacer1.png)![](media/sddc/netcontroller.png)

Les administrateurs utilisent un outil de gestion qui interagit directement avec le contrôleur de réseau. Le contrôleur de réseau fournit des informations sur l’infrastructure réseau, notamment l’infrastructure virtuelle et physique, à l’outil de gestion.

![](media/sddc/learn.png)**[En savoir plus sur le contrôleur de réseau](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-controller/network-controller)**

### Pare-feu de centre de données ###

![](media/sddc/networking-line.png)

Lorsqu’il est déployé et proposé en tant que service, les administrateurs clients peuvent installer et configurer des stratégies de pare-feu pour protéger les réseaux virtuels du trafic indésirable d’Internet et des réseaux intranet.

![](media/sddc/spacer1.png)![](media/sddc/firewall.png)

L’administrateur du fournisseur de services ou l’administrateur client peut gérer les stratégies de pare-feu de centre de données via le contrôleur de réseau.

![](media/sddc/learn.png)**[En savoir plus sur le pare-feu de centre de données](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-function-virtualization/datacenter-firewall-overview)**

### SET (Switch Embedded Teaming) ###

![](media/sddc/networking-line.png)

SET est une autre solution d’association de cartes réseau que vous pouvez utiliser dans des environnements qui incluent Hyper-V et la pile [de mise en réseau SDN](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking).

![](media/sddc/spacer1.png)![](media/sddc/teaming.png)

![](media/sddc/learn.png)**[En savoir plus sur SET (Switch Embedded Teaming)](https://docs.microsoft.com/windows-server/networking/sdn/technologies/set-for-sdn)**

### Équilibrage de la charge logicielle ###

![](media/sddc/networking-line.png)

La fonctionnalité d’équilibrage de charge logicielle vous permet d’activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité. Faites monter en puissance les fonctionnalités d’équilibrage de la charge à l’aide d’ordinateurs virtuels d’équilibrage de la charge logicielle sur les mêmes serveurs Hyper-V que ceux utilisés pour vos autres charges de travail d’ordinateurs virtuels. L’équilibrage de la charge logicielle prend en charge la création et la suppression rapides de points de terminaison d’équilibrage de charge pour les opérations de fournisseur de services cloud. Il prend en charge des dizaines de gigaoctets par cluster, fournit un modèle de configuration simple, et peut facilement monter en puissance ou être réduit.

![](media/sddc/spacer1.png)![](media/sddc/balancer.png)

![](media/sddc/learn.png)**[En savoir plus sur l’équilibrage de la charge logicielle](https://docs.microsoft.com/windows-server/networking/sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn)**


![](media/sddc/storage.png)

### Espaces de stockage direct ###

![](media/sddc/storage-line.png)

Les espaces de stockage direct tirent parti de serveurs standard utilisant des disques locaux pour fournir un stockage à définition logicielle hautement disponible et évolutif, pour un coût nettement inférieur à celui des baies SAN ou NAS traditionnelles. Son architecture simplifie considérablement la configuration et le déploiement.

![Sur chaque nœud, les lecteurs connectés localement sont mis en pool au niveau du cluster par les espaces de stockage direct, puis les ordinateurs virtuels y accèdent via les volumes partagés de cluster](media/sddc/spacer1.png)![](media/sddc/ssd.png)

Les espaces de stockage direct introduisent le nouveau bus de stockage virtuel SoftwareStorageBus et tirent parti des nombreuses fonctionnalités de WindowsServer que vous connaissez déjà, telles que le clustering de basculement, les volumes partagés de cluster, le protocole SMB3 (Server Message Block) et les espaces de stockage.

![](media/sddc/learn.png)**[En savoir plus sur les espaces de stockage direct](storage/storage-spaces/storage-spaces-direct-overview.md)**
### Qualité de service de stockage ###


![](media/sddc/storage-line.png)

Elle offre un moyen de surveiller et de gérer de manière centralisée les performances de stockage pour les ordinateurs virtuels utilisant des rôles Hyper-V et de serveur de fichiers avec montée en puissance parallèle, améliorant l’équité des ressources de stockage entre plusieurs ordinateurs virtuels.

![](media/sddc/spacer1.png)![](media/sddc/qos.png)

La qualité de service de stockage est intégrée à la solution de stockage défini par logiciel Microsoft et fournie par le serveur de fichiers avec montée en puissance parallèle et Hyper-V à l’aide du protocoleSMB3. Un nouveau gestionnaire de stratégie permet de contrôler de manière centralisée les performances de stockage.

![](media/sddc/learn.png)**[En savoir plus sur la qualité de service de stockage](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview)**

### Réplica de stockage ###


![](media/sddc/storage-line.png)

La préparation aux situations d’urgence et de récupération d’urgence garantit l’absence totale de perte de données, avec la possibilité de protéger de façon synchrone les données dans différents racks, étages, bâtiments, campus, régions et villes, avec une utilisation plus efficace de plusieurs centres de données.

![](media/sddc/spacer1.png)
![](media/sddc/storage-replica.png)

Réplication synchrone

1. L’application écrit des données
2. Les données du journal sont écrites et les données sont répliquées sur le site distant
3. Les données du journal sont écrites sur le site distant
4. Accusé de réception du site distant
5. Réception de l’écriture d’application confirmée

t&t1: données vidées sur le volume, journaux toujours écrits en continu


![](media/sddc/learn.png)**[En savoir plus sur le réplica de stockage](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-overview)**


![](media/sddc/security.png)


### Structure protégée ###


![](media/sddc/security-line.png)

En tant que fournisseur de services cloud ou administrateur d’un cloud privé d’entreprise, vous pouvez utiliser une structure protégée pour offrir un environnement plus sécurisé pour les ordinateurs virtuels. Une structure protégée se compose d’un Service Guardian hôte (HGS), généralement, un cluster de trois nœuds, d’un ou de plusieurs hôtes protégés et d’un ensemble d’ordinateurs virtuels.

![](media/sddc/spacer1.png)![](media/sddc/guarded-fabric.png)

![](media/sddc/learn.png)**[En savoir plus sur la structure protégée](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)**

### Ordinateurs virtuels protégés ###

![](media/sddc/security-line.png)

Les données et l’état d’un ordinateur virtuel protégé sont protégés contre l’inspection, le vol et la falsification par des administrateurs de centre de données ou des programmes malveillants.

![](media/sddc/spacer1.png)![](media/sddc/shielded.png)

- Les ordinateurs virtuels protégés s’exécutent uniquement dans les infrastructures désignées en tant que propriétaires de l’ordinateur virtuel.
- Les ordinateurs virtuels protégés sont chiffrés à l’aide de BitLocker ou d’une autre méthode, afin que seuls les propriétaires désignés soient en mesure de les exécuter.
- Les ordinateurs virtuels en cours d’exécution peut être convertis en ordinateurs protégés.

![](media/sddc/learn.png)**[En savoir plus sur les ordinateurs virtuels protégés](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms)**

### Service Guardian hôte ###

![](media/sddc/security-line.png)

Le Service Guardian hôte contient les clés pour les infrastructures légitimes, ainsi que pour les ordinateurs virtuels chiffrés.

![](media/sddc/spacer1.png)![](media/sddc/guardian.png)

![](media/sddc/learn.png)**[En savoir plus sur le Service Guardian hôte](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-hgs)**

### Attestation d’intégrité des appareils ###

![](media/sddc/security-line.png)

L’attestation permet aux entreprises d’élever la sécurité de leur organisation au niveau d’une sécurité attestée et surveillée du matériel, avec un impact minime ou inexistant sur le coût d’exploitation.


![](media/sddc/spacer1.png)![](media/sddc/attestation.png)


Le mode de matériel sécurisé, ci-dessus, fournit le niveau d’assurance le plus élevé, avec l’approbation associée à une racine et la conformité à la stratégie d’intégrité du code pour la libération des clés, incluses dans la version2.0 du module de plateforme sécurisée.


![](media/sddc/learn.png)**[En savoir plus sur l’attestation d’intégrité des appareils](https://docs.microsoft.com/windows-server/security/device-health-attestation)**

![](media/sddc/management.png)

### PowerShellDSC ###

![](media/sddc/management-line.png)

La configuration d’état souhaité (DSC) WindowsPowerShell est une plateforme de gestion de la configuration intégrée à Windows, basée sur des normes ouvertes. DSC est suffisamment flexible pour fonctionner de manière fiable et cohérente à chaque étape du cycle de vie de déploiement (développement, test, préproduction, production), ainsi qu’au cours d’une montée en puissance parallèle.

![](media/sddc/spacer1.png)![](media/sddc/dsc.png)

DSC prend en charge les «déploiements continus», ce qui vous permet de déployer les configurations à de nombreuses reprises sans rien perturber.

-  Les configurations DSC appliquent uniquement les paramètres dont les valeurs d’origine ont été modifiées pour des déploiements plus rapides.
-  DSC peut être utilisé en local ou dans un environnement de Cloud public ou privé.
-  Vous pouvez intégrer DSC à n’importe quelle solution Microsoft ou tierce, tant que vous exécutez un script PowerShell sur le système cible.

![](media/sddc/learn.png)**[En savoir plus sur PowerShellDSC](https://docs.microsoft.com/powershell/dsc/overview)**


### SystemCenterVMM ###

![](media/sddc/management-line.png)

VirtualMachineManager fait partie de la suite SystemCenter, utilisée pour configurer, gérer et transformer des centres de données traditionnels afin d’offrir une expérience de gestion unifiée pour les ressources locales, le fournisseur de services et le cloud Azure.

![](media/sddc/spacer1.png)![](media/sddc/vmm.png)

- Centre de données: configuration et gestion des composants du centre de données en tant qu’infrastructure unique dans VMM. 
- Hôtes de virtualisation: VMM peut ajouter, configurer et gérer des hôtes de virtualisation et des clusters Hyper-V et VMware.
- Mise en réseau: VMM fournit la virtualisation de réseau, y compris la prise en charge de la création et la gestion des réseaux virtuels et des passerelles de réseau. 
- Stockage: VMM peut détecter, classer, configurer, allouer et affecter le stockage local et distant.

![](media/sddc/learn.png)**[En savoir plus sur SystemCenterVMM](https://docs.microsoft.com/system-center/vmm/)**

### Windows Admin Center ###

![](media/sddc/management-line.png)

Windows Admin Center est un ensemble d’outils de gestion basé sur un navigateur et déployé localement qui permet d'administrer en local des serveurs Windows Server sans dépendance à Azure ou au cloud. Windows Admin Center donne aux administrateurs informatiques un contrôle total sur tous les aspects de leur infrastructure de serveurs. Il s’avère particulièrement utile pour la gestion de réseaux privés qui ne sont pas connectés à Internet.

![](media/sddc/spacer1.png)![](media/sddc/architecture.png)

La publication du serveur web dans DNS et la configuration du pare-feu d’entreprise vous permettent d’accéder à Windows Admin Center à partir de l’Internet public. Ainsi, vous pouvez vous connecter à vos serveurs et les gérer à partir de n’importe quel emplacement avec MicrosoftEdge ou GoogleChrome.

![](media/sddc/learn.png)**[En savoir plus sur le projet Windows Admin Center de Microsoft](manage/windows-admin-center/overview.md)**

