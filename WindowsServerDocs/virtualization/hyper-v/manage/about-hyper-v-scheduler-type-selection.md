---
title: À propos de la sélection du type de planificateur hyperviseur Hyper-V
description: Fournit des informations pour les administrateurs d’hôtes Hyper-V sur l’utilisation du Planificateur de Hyper-V de modes
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823880"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>À propos de la sélection du type de planificateur hyperviseur Hyper-V

S'applique à :

* Windows Server 2016
* Windows Server, version 1709
* Windows Server, version 1803
* Windows Server 2019

Ce document décrit les modifications importantes apportées à la valeur par défaut de Hyper-V et utilisation d’hyperviseur de recommandée de types de planificateur. Impact de ces modifications à la fois performances de la virtualisation et de sécurité système. Administrateurs d’hôtes de virtualisation doivent examiner comprendre les modifications et les implications en matière de décrites dans ce document et évaluer avec soin les impacts, les instructions de déploiement suggéré et les facteurs de risque impliqué pour mieux comprendre comment déployer et gérer Hôtes Hyper-V face à l’évolution de sécurité rapidement.

>[!IMPORTANT]
>Sécurité de canal latéral susceptibles d’être exploitées par une machine virtuelle invitée malveillante via le comportement de planification du type de planificateur classique hyperviseur hérités sur les hôtes avec simultanée des vulnérabilités dans plusieurs architectures de processeur inconnu Multithreading (SMT) activé.  Si elles sont exploitées avec succès, une charge de travail malveillant pourrait observer les données en dehors de sa limite de partition. Cette classe d’attaques peut être atténuée en configurant l’hyperviseur Hyper-V pour utiliser le type de planificateur d’hyperviseur core et invité reconfigurer les machines virtuelles. Avec le planificateur core, l’hyperviseur restreint VPs un invité de la machine virtuelle pour s’exécuter sur le même cœur de processeur physique, par conséquent isolant fortement capacité de la machine virtuelle pour accéder aux données pour les limites du cœur physique sur lequel il s’exécute.  Il s’agit d’une atténuation très efficace contre ces attaques côté canal, ce qui empêche que la machine virtuelle en observant les artefacts à partir d’autres partitions, si la racine ou une autre partition de l’invité.  Par conséquent, Microsoft modifie la valeur par défaut et recommandé des paramètres de configuration pour les hôtes de virtualisation et les machines virtuelles invitées.

## <a name="background"></a>Arrière-plan

À compter de Windows Server 2016, Hyper-V prend en charge plusieurs méthodes de planification et gestion des processeurs virtuels, appelés types de planificateur d’hyperviseur.  Vous trouverez une description détaillée de tous les types de planificateur hyperviseur dans [compréhension et l’utilisation de types de planificateur d’hyperviseur Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).

>[!NOTE]
>Nouveaux types de planificateur hyperviseur ont été introduites avec Windows Server 2016 et ne sont pas disponibles dans les versions antérieures. Toutes les versions d’Hyper-V avant Windows Server 2016 prend en charge uniquement le planificateur classique. Prise en charge pour le Planificateur de noyau a été que récemment publié.

## <a name="about-hypervisor-scheduler-types"></a>À propos des types de planificateur d’hyperviseur

Cet article se concentre spécifiquement sur l’utilisation du nouveau type de planificateur hyperviseur core et le Planificateur de « classique » hérité, et comment ces types de planificateur croisent avec l’utilisation de Symmetric Multi-Threading ou SMT.  Il est important de comprendre les différences entre les planificateurs core et le déploiement classique et comment chacune place fonctionne à partir de machines virtuelles invitées sur les processeurs du système sous-jacent.

### <a name="the-classic-scheduler"></a>Le planificateur classique

Le planificateur classique fait référence à la méthode du tourniquet (Round Robin) partage équitable, de planifier le travail sur les processeurs virtuels (VPs) sur le système - y compris racine VPs, ainsi que des VPs appartenant à des machines virtuelles invitées. Le planificateur classique a été le type de planificateur par défaut utilisé sur toutes les versions d’Hyper-V (jusqu'à ce que Windows Server 2019, comme décrit dans le présent document).  Les caractéristiques de performances du planificateur classique sont bien comprises, et le planificateur classique est illustré pour prendre en charge ably de surabonnement des charges de travail - autrement dit, d’un surabonnement de ratio de VP:LP de l’hôte d’une marge raisonnable (en fonction de la types de charges de travail virtualisées, l’utilisation globale des ressources, etc.).

Exécuté sur un hôte de virtualisation avec SMT activé, le planificateur classique planifiera VPs invité à partir de toutes les machines virtuelles sur chaque thread SMT appartenant à un cœur indépendamment. Par conséquent, différentes machines virtuelles peuvent être en cours d’exécution sur le même cœur en même temps (une machine virtuelle en cours d’exécution sur un thread d’un cœur pendant l’exécution d’une autre machine virtuelle sur l’autre thread).

### <a name="the-core-scheduler"></a>Le Planificateur de noyau

Le Planificateur de noyau s’appuie sur les propriétés de SMT pour assurer l’isolation des charges de travail invité, ce qui a un impact sur la sécurité et les performances du système. Le planificateur core garantit que VPs à partir d’une machine virtuelle sont planifiées sur des threads SMT frère. Cela symétriquement afin que si LPs se trouvent dans des groupes de deux, VPs sont planifiées dans les deux groupes, et un cœur de processeur système n’est jamais partagé entre les machines virtuelles.

En planifiant VPs invité sur les paires SMT sous-jacent, le planificateur core offre une limite de sécurité renforcée pour l’isolation de la charge de travail et peut également servir à réduire la variabilité des performances pour les charges de travail sensibles de latence.

Notez que paramètre lorsque le vice-président est planifiée pour une machine virtuelle sans SMT activé, que VP consommera les principales lorsqu’elle s’exécute et frère de la base thread SMT restera inactif.  Cela est nécessaire de fournir l’isolation de la charge de travail correct, mais a un impact sur les performances globales du système, surtout lorsque les système LPs deviennent trop sollicitées - autrement dit, lorsque les taux total VP:LP dépasse 1:1. Par conséquent, les machines virtuelles invitées configurés sans que plusieurs threads par cœur en cours d’exécution est une configuration optimale.

### <a name="benefits-of-the-using-the-core-scheduler"></a>Avantages de l’utilisation du Planificateur de noyau

Le planificateur core offre les avantages suivants :

* Une limite de sécurité renforcée pour l’isolation des charges de travail invité - invité VPs sont limités à une exécution sur des paires de cœur physique sous-jacent, ce qui réduit la vulnérabilité aux attaques par espionnage de canal latéral.

* Variabilité réduit la charge de travail - variabilité de débit de charge de travail invité est considérablement réduite, offre une plus grande cohérence de la charge de travail.

* Utilisation de SMT dans invité des machines virtuelles - le système d’exploitation et les applications exécutées dans la machine virtuelle invitée peut utiliser le comportement de SMT et interfaces de programmation (API) pour contrôler et de répartir le travail entre les threads SMT, comme s’ils étaient exécutés quand non virtualisés.

Le Planificateur de noyau est actuellement utilisé sur les hôtes de virtualisation Azure, spécifiquement pour tirer parti de la limite de sécurité renforcée et variabilty de faible charge de travail. Microsoft est convaincu que le type de planificateur core doit être et continue à être l’hyperviseur par défaut type pour la majorité des scénarios de virtualisation de planification.  Par conséquent, pour garantir à que nos clients sont sécurisés par défaut, Microsoft apporte cette modification pour Windows Server 2019 maintenant.

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>Core impacts sur les performances du planificateur sur les charges de travail invité

Tandis que nécessaire pour réduire certains types de vulnérabilités de manière efficace, le Planificateur de noyau peut également réduire les performances. Les clients peuvent constater une différence entre les caractéristiques de performances avec leurs machines virtuelles et leurs impacts à la capacité de charge de travail globale de leurs hôtes de virtualisation. Dans les cas où le Planificateur de noyau doit exécuter un non - SMT VP, seul les flux d’instructions dans le cœur logique sous-jacente s’exécute pendant que l’autre doit rester inactive. Cela limitera la capacité totale d’hôte pour les charges de travail invité.

Ces impacts sur les performances peuvent être réduites en suivant les instructions de déploiement dans ce document. Les administrateurs d’hôtes doivent soigneusement prendre en compte leur virtualisation spécifique de scénarios de déploiement et équilibre leur tolérance en matière de risque de sécurité et la nécessité de densité de la charge de travail maximale, une consolidation des hôtes de virtualisation, etc.

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Modifications apportées à la valeur par défaut et les configurations recommandées pour Windows Server 2016 et Windows Server 2019

Déploiement d’hôtes Hyper-V avec la posture de sécurité maximale nécessite l’utilisation du type de planificateur core hyperviseur. Pour garantir à que nos clients sont sécurisés par défaut, Microsoft modifie la valeur par défaut suivant et les paramètres recommandés.

>[!NOTE]
>Alors que la prise en charge interne de l’hyperviseur pour les types de planificateur a été incluse dans la version initiale de Windows Server 2016, Windows Server 1709 et Windows Server 1803, mises à jour sont nécessaires pour accéder au contrôle de configuration qui permet de sélectionner le type de planificateur d’hyperviseur.  Reportez-vous à [compréhension et l’utilisation de types de planificateur d’hyperviseur Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) pour plus d’informations sur ces mises à jour.

### <a name="virtualization-host-changes"></a>Modifications d’hôte de virtualisation

* L’hyperviseur utilise le Planificateur de noyau par défaut à compter avec Windows Server 2019.

* Reccommends Microsoft configuration le planificateur core sur Windows Server 2016. Le type de planificateur hyperviseur core est pris en charge dans Windows Server 2016, cependant, la valeur par défaut est le planificateur classique. Le Planificateur de noyau est facultatif et doit être explicitement activé par l’administrateur de l’hôte Hyper-V.

### <a name="virtual-machine-configuration-changes"></a>Modifications de configuration de machine virtuelle

* Sur Windows Server 2019, nouveaux ordinateurs virtuels créés à l’aide de la version de machine virtuelle par défaut 9.0 héritera automatiquement les propriétés SMT (activé ou désactivé) de l’hôte de virtualisation. Autrement dit, si SMT est activé sur l’hôte physique, qui vient d’être créé les machines virtuelles auront également SMT activé et hérite la topologie SMT de l’hôte par défaut, avec la machine virtuelle ayant le même nombre de threads matériels par cœur en tant que le système sous-jacent. Il apparaîtra dans la configuration de la machine virtuelle avec HwThreadCountPerCore = 0, 0 correspondant à la machine virtuelle doit hériter des paramètres de SMT de l’hôte.

* Machines virtuelles existantes avec une version de la machine virtuelle de 8.2 ou une version antérieure sera conservent leur paramètre de processeur de machine virtuelle d’origine pour HwThreadCountPerCore, et la valeur par défaut pour les visiteurs de version 8.2 machine virtuelle est HwThreadCountPerCore = 1. Lorsque ces invités exécutent sur un hôte Windows Server 2019, ils seront traités comme suit :

    1. Si la machine virtuelle a un nombre de VP est inférieur ou égal au nombre de cœurs de LP, la machine virtuelle est considérée comme une non - SMT machine virtuelle par le Planificateur de noyau. Lorsque le vice-président invité s’exécute sur un seul thread SMT, frère de la base thread SMT sera inactif. Cela n’est pas optimal et provoquer une indisponibilité globale des performances.

    2. Si la machine virtuelle a VPs plus de cœurs de LP, la machine virtuelle est considérée comme une VM SMT par le Planificateur de noyau. Toutefois, la machine virtuelle ne sera pas équilibrée autres indications qu’il s’agit d’une VM SMT. Par exemple, utilisation de l’API de Windows ou d’une instruction CPUID pour interroger la topologie du processeur par le système d’exploitation ou les applications n’indiquera pas que l’option SMT est activée.

* Lorsqu’une machine virtuelle existante est explicitement mis à jour à partir de versions de machine virtuelle de versions antérieures à la version 9.0 via l’opération de mise à jour-machine virtuelle, la machine virtuelle conserve sa valeur actuelle pour HwThreadCountPerCore.  La machine virtuelle n’a pas de SMT prenant en charge de force.

* Sur Windows Server 2016, Microsoft recommande l’activation de SMT pour les machines virtuelles invitées.  Par défaut, les machines virtuelles créées sur Windows Server 2016 serait ont SMT désactivés, qui est que hwthreadcountpercore est défini sur 1, sans modification explicite.

>[!NOTE]
>Windows Server 2016 ne prend pas en charge le paramètre HwThreadCountPerCore sur 0.

#### <a name="managing-virtual-machine-smt-configuration"></a>Gestion de la configuration de SMT de machine virtuelle

La configuration de SMT d’ordinateur virtuel invité est définie sur une base par machine virtuelle. L’administrateur de l’hôte peut inspecter et configurer la configuration de SMT d’une machine virtuelle pour sélectionner parmi les options suivantes :

    1. Configurer des machines virtuelles pour exécuter comme SMT compatibles, éventuellement héritant automatiquement la topologie SMT hôte

    2. Configurer des machines virtuelles pour exécuter en tant que non-SMT

La configuration SMT pour une machine virtuelle s’affiche dans les volets de résumé dans la console du Gestionnaire Hyper-V.  Configuration des paramètres de SMT d’une machine virtuelle peut être effectuée à l’aide des paramètres de la machine virtuelle ou de PowerShell.

#### <a name="configuring-vm-smt-settings-using-powershell"></a>Configuration des paramètres de VM SMT à l’aide de PowerShell

Pour configurer les paramètres de SMT pour une machine virtuelle invitée, ouvrez une fenêtre PowerShell avec des autorisations suffisantes, puis tapez :

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Où :

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Pour lire les paramètres de SMT pour une machine virtuelle invitée, ouvrez une fenêtre PowerShell avec des autorisations suffisantes, puis tapez :

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Notez qu’invité de machines virtuelles configurées avec HwThreadCountPerCore = 0 indique que SMT est activée pour l’invité et exposera le même nombre de threads SMT à l’invité comme ils se trouvent sur l’hôte de virtualisation sous-jacente, en général 2.

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>Machines virtuelles invitées peut observer les modifications apportées à la topologie de l’UC entre les scénarios de mobilité de machine virtuelle

Le système d’exploitation et les applications dans une machine virtuelle peuvent voir les modifications à l’hôte et les paramètres de machine virtuelle avant et après que les événements de cycle de vie de machine virtuelle comme migration dynamique ou l’enregistrement et les opérations de restauration. Pendant une opération dans les machines virtuelles est enregistré et restauré, les paramètres de la machine virtuelle HwThreadCountPerCore et la valeur réalisée (autrement dit, la combinaison calculée du paramètre de la machine virtuelle et de configuration de l’hôte source) sont migrés. La machine virtuelle continue à s’exécuter avec ces paramètres sur l’hôte de destination. Au point de la machine virtuelle est arrêté et redémarré, il est possible que la valeur réalisée observée par la machine virtuelle changera. Cette valeur doit être sans gravité, en tant que système d’exploitation et applications logiciels de la couche doivent rechercher les informations sur la topologie du processeur dans le cadre de leurs flux de code d’initialisation et de démarrage normal. Toutefois, étant donné que ces initialisation démarrage séquences sont ignorés lors des opérations de migration ou de sauvegarde/restauration en direct, les machines virtuelles que suivent les ces transitions d’état pourrait observer calculée à l’origine valeur réalisée jusqu'à ce qu’ils sont arrêtés et redémarré.  

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>Alertes concernant des configurations non optimales de machine virtuelle

Machines virtuelles configurées avec des VPs plus qu’il ne sont cœurs physiques sur le résultat de l’hôte dans une configuration non optimales. Le planificateur d’hyperviseur traitera ces machines virtuelles comme s’ils sont compatibles avec SMT. Toutefois, système d’exploitation et logiciels d’application dans la machine virtuelle s’afficheront une topologie de processeur montrant SMT est désactivé. Lorsque cette condition est détectée, le processus de travail Hyper-V enregistrera un événement sur l’hôte de virtualisation avertissement de l’administrateur de l’hôte que la configuration de la machine virtuelle n’est pas optimal, et recommander SMT être activée pour la machine virtuelle.

#### <a name="how-to-identify-non-optimally-configured-vms"></a>Comment identifier de façon non optimale configuré des machines virtuelles

Vous pouvez identifier les non - SMT machines virtuelles en examinant le journal système dans l’Observateur d’événements pour l’événement de processus de travail Hyper-V 3498 ID, qui sera déclenchée pour une machine virtuelle chaque fois que le nombre de VPs dans la machine virtuelle est supérieur au nombre de cœur physique. Événements du processus de travail peuvent être obtenus à partir de l’Observateur d’événements, ou via PowerShell.

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>Interrogation de l’événement de machine virtuelle de processus de travail Hyper-V à l’aide de PowerShell

À la requête pour l’événement de processus de travail Hyper-V 3498 d’ID à l’aide de PowerShell, entrez les commandes suivantes à partir d’une invite de PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>Impacts d’invité SMT personnels sur l’utilisation de l’hyperviseur les enlightments pour les systèmes d’exploitation invités

L’hyperviseur Microsoft offre plusieurs enlightments, ou indicateurs, ce qui le système d’exploitation en cours d’exécution sur une machine virtuelle invitée peut interroger et utiliser pour déclencher des optimisations, telles que celles qui peuvent bénéficier de performances ou sinon améliore la gestion de diverses conditions lors de l’exécution virtualisé. Un seul révération récemment introduite concerne la gestion de la planification de processeur virtuel et l’utilisation de solutions d’atténuation du système d’exploitation pour les attaques côté canal qui exploitent SMT.

>[!NOTE]
>Microsoft recommande que les administrateurs d’hôtes activent SMT pour les machines virtuelles invitées optimiser les performances de la charge de travail.

Les détails de cette révération invité sont fournis ci-dessous, toutefois l’essentiel à retenir pour les administrateurs de virtualisation hôte n’est que les machines virtuelles doivent avoir HwThreadCountPerCore configurée pour correspondre à physique SMT configuration l’hôte. Ainsi, l’hyperviseur signaler qu’il n’existe aucun core-architecturaux partage. Par conséquent, aucune optimisation de prise en charge du système d’exploitation invité qui nécessitent les connaissances peut être activée. Sur Windows Server 2019, créer des machines virtuelles et conservez la valeur par défaut HwThreadCountPerCore (0). Anciennes machines virtuelles migrées à partir de Windows Server 2016 hôtes peuvent être mis à jour vers la version de configuration de Windows Server 2019. Après cela, Microsoft recommande de définir HwThreadCountPerCore = 0.  Sur Windows Server 2016, Microsoft vous recommande de paramètre HwThreadCountPerCore pour correspondre à la configuration d’hôte (en général, 2).

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>Détails de révération NoNonArchitecturalCoreSharing

À compter de Windows Server 2016, l’hyperviseur définit une nouveau révération pour décrire sa gestion des VP de planification et le placement du système d’exploitation invité. Cette révération est définie dans le [v5.0c de spécification fonctionnelle de hyperviseur haut niveau](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

Feuille CPUID synthétique hyperviseur CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] indique qu’un processeur virtuel partagent jamais un cœur physique avec un autre processeur virtuel, à l’exception des processeurs virtuels qui sont signalés en tant que frère SMT threads. Par exemple, un vice-président invité sera jamais exécuté sur un thread SMT parallèlement à un vice-président racine en cours d’exécution simultanément sur un thread SMT sur le même cœur de processeur frère. Cette condition n’est possible que lors de l’exécution virtualisé et représente donc un comportement SMT-architecturaux qui a également des implications de sécurité sérieux. Le système d’exploitation invité peut utiliser NoNonArchitecturalCoreSharing = 1 en tant qu’indication qu’il est sûr activer les optimisations qui peuvent aider à éviter la surcharge de performances de la configuration STIBP.

Dans certaines configurations, l’hyperviseur n’indiquera pas ce NoNonArchitecturalCoreSharing = 1. Par exemple, si un hôte a SMT activé et est configuré pour utiliser le Planificateur de classique de l’hyperviseur, NoNonArchitecturalCoreSharing est égal à 0. Cela peut empêcher les invités compatibles de l’activation de certaines optimisations. Par conséquent, Microsoft recommande que les administrateurs d’hôtes à l’aide de SMT s’appuient sur le Planificateur de noyau d’hyperviseur et assurez-vous que les machines virtuelles sont configurés pour héritent leur configuration SMT de l’hôte pour garantir des performances optimales de la charge de travail.

## <a name="summary"></a>Récapitulatif

Le paysage de menaces de sécurité continue d’évoluer. Pour garantir à que nos clients sont sécurisés par défaut, Microsoft modifie la configuration par défaut pour l’hyperviseur et les machines virtuelles à partir de Windows Server 2019 Hyper-V, et fournissant des mises à jour des conseils et des recommandations pour les clients qui exécutent Windows Server 2016 Hyper-V. Administrateurs d’hôtes de virtualisation doivent :

* Lire et comprendre les instructions fournies dans ce document

* Évaluer avec soin et d’ajuster leurs déploiements de virtualisation pour vérifier qu’ils respectent la sécurité, les performances, densité de virtualisation et objectifs de réactivité de charge de travail de leurs exigences uniques

* Prendre en compte la reconfiguration des hôtes Windows Server 2016 Hyper-V existants pour tirer parti des avantages de sécurité renforcée offertes par le Planificateur de noyau d’hyperviseur

* Mettre à jour non - SMT des machines virtuelles existantes afin de réduire l’impact sur les performances de la planification des contraintes imposées par l’isolation VP qui traite des vulnérabilités de sécurité matériel
