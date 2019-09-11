---
title: À propos de la sélection du type de planificateur de l’hyperviseur Hyper-V
description: Fournit des informations pour les administrateurs d’ordinateurs hôtes Hyper-V sur l’utilisation des modes de planification d’Hyper-V
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: 1e77535548cccd1c821163dabbad381f35d2948a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872059"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>À propos de la sélection du type de planificateur de l’hyperviseur Hyper-V

S'applique à :

* Windows Server 2016
* Windows Server, version 1709
* Windows Server, version 1803
* Windows Server 2019

Ce document décrit les modifications importantes apportées à l’utilisation par défaut et recommandée d’Hyper-V pour les types de planificateur d’hyperviseur. Ces modifications ont un impact sur les performances de la sécurité et de la virtualisation du système. Les administrateurs hôtes de virtualisation doivent examiner et comprendre les modifications et les implications décrites dans ce document, et évaluer avec soin les impacts, les recommandations de déploiement suggérées et les facteurs de risque impliqués dans la meilleure compréhension du déploiement et de la gestion Ordinateurs hôtes Hyper-V en face du paysage de sécurité à évolution rapide.

>[!IMPORTANT]
>Les vulnérabilités de sécurité de canal virtuel actuellement connues dans des architectures à plusieurs processeurs peuvent être exploitées par une machine virtuelle invitée malveillante via le comportement de planification du type de planificateur classique d’hyperviseur hérité lorsqu’ils sont exécutés sur des ordinateurs hôtes avec Multithread (SMT) activé.  Si elle est exploitée avec succès, une charge de travail malveillante pourrait observer des données en dehors de ses limites de partition. Cette classe d’attaques peut être atténuée en configurant l’hyperviseur Hyper-V pour qu’il utilise le type de planificateur du noyau de l’hyperviseur et reconfigure les machines virtuelles invitées. Avec le planificateur de base, l’hyperviseur restreint les VPs d’une machine virtuelle invitée à s’exécuter sur le même noyau de processeur physique, ce qui permet d’isoler fortement la capacité de la machine virtuelle à accéder aux données dans les limites du noyau physique sur lequel elle s’exécute.  Il s’agit d’une atténuation très efficace contre ces attaques de canaux secondaires, qui empêche la machine virtuelle d’observer les artefacts d’autres partitions, que ce soit la racine ou une autre partition invitée.  Par conséquent, Microsoft modifie les paramètres de configuration par défaut et recommandés pour les hôtes de virtualisation et les machines virtuelles invitées.

## <a name="background"></a>Présentation

À compter de Windows Server 2016, Hyper-V prend en charge plusieurs méthodes de planification et de gestion des processeurs virtuels, appelées types de planificateur d’hyperviseur.  Une description détaillée de tous les types de planificateur d’hyperviseur est disponible dans [présentation et utilisation des types de planificateur d’hyperviseur Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).

>[!NOTE]
>De nouveaux types de planificateur d’hyperviseur ont été introduits pour la première fois avec Windows Server 2016 et ne sont pas disponibles dans les versions antérieures. Toutes les versions d’Hyper-V antérieures à Windows Server 2016 prennent uniquement en charge le planificateur classique. La prise en charge du planificateur de base a été récemment publiée.

## <a name="about-hypervisor-scheduler-types"></a>À propos des types de planificateur d’hyperviseur

Cet article se concentre spécifiquement sur l’utilisation du nouveau type de planificateur du noyau de l’hyperviseur par rapport au planificateur « classique » hérité, et sur la façon dont ces types de planificateur se croisent avec l’utilisation du Multi-Threading symétrique, ou SMT.  Il est important de comprendre les différences entre les planificateurs principaux et les planificateurs classiques et la façon dont chacun d’entre eux s’effectue à partir de machines virtuelles invitées sur les processeurs système sous-jacents.

### <a name="the-classic-scheduler"></a>Le planificateur classique

Le planificateur classique fait référence à la méthode de planification de travail sur les processeurs virtuels (VPs) à l’échelle du système, y compris les VPs racine, ainsi que les VPs appartenant aux machines virtuelles invitées. Le planificateur classique est le type de planificateur par défaut utilisé sur toutes les versions d’Hyper-V (jusqu’à Windows Server 2019, comme décrit dans le présent document).  Les caractéristiques de performances du planificateur classique sont bien comprises, et le planificateur classique est démontré à la prise en charge de la surabonnement des charges de travail, c’est-à-dire au surabonnement du ratio VP/de l’hôte par une marge raisonnable (en fonction de la types de charges de travail virtualisées, utilisation globale des ressources, etc.).

Lorsqu’il est exécuté sur un ordinateur hôte de virtualisation avec SMT activé, le planificateur classique planifie les VPs invités à partir de n’importe quelle machine virtuelle sur chaque thread SMT appartenant indépendamment à un cœur. Par conséquent, différentes machines virtuelles peuvent s’exécuter sur le même cœur simultanément (une machine virtuelle s’exécutant sur un thread d’un cœur alors qu’une autre machine virtuelle est en cours d’exécution sur l’autre thread).

### <a name="the-core-scheduler"></a>Planificateur principal

Le Planificateur principal exploite les propriétés de SMT pour assurer l’isolation des charges de travail invitées, ce qui a un impact sur la sécurité et les performances du système. Le Planificateur principal s’assure que les VPs d’une machine virtuelle sont planifiés sur les threads SMT frères. Cette opération est effectuée symétriquement, de sorte que si la base de l’UC est de deux groupes de deux, les VPs sont planifiés dans des groupes de deux et un cœur du processeur système n’est jamais partagé entre les machines virtuelles.

En planifiant les VPs invités sur les paires de SMT sous-jacentes, le planificateur de base offre une limite de sécurité renforcée pour l’isolation de la charge de travail et peut également être utilisé pour réduire la variabilité des performances pour les charges de travail sensibles à la latence.

Notez que lorsque le VP est planifié pour un ordinateur virtuel sans SMT activé, ce vice-président consommera l’intégralité du noyau lors de son exécution, et le thread de SMT frère du cœur restera inactif.  Cela est nécessaire pour fournir l’isolation de charge de travail correcte, mais a un impact sur les performances globales du système, en particulier lorsque le nombre de mètres du système est dépassé, c’est-à-dire lorsque le ratio VP total est supérieur à 1:1. Par conséquent, l’exécution de machines virtuelles invitées configurées sans plusieurs threads par cœur est une configuration sous-optimale.

### <a name="benefits-of-the-using-the-core-scheduler"></a>Avantages de l’utilisation du planificateur de base

Le Planificateur principal offre les avantages suivants :

* Une limite de sécurité renforcée pour l’isolation de la charge de travail invité-les VPs invités sont limités pour s’exécuter sur les paires de cœurs physiques sous-jacentes, ce qui réduit les risques d’attaques d’espionnage des canaux secondaires.

* Variabilité de la charge de travail réduite-la variabilité du débit de la charge de travail invitée est considérablement réduite, ce qui offre une plus grande cohérence

* Utilisation de SMT dans les machines virtuelles invitées : le système d’exploitation et les applications qui s’exécutent sur la machine virtuelle invitée peuvent utiliser le comportement SMT et les interfaces de programmation (API) pour contrôler et distribuer le travail entre les threads SMT, comme c’est le cas lors de l’exécution non virtualisée.

Le Planificateur principal est actuellement utilisé sur les hôtes de virtualisation Azure, en particulier pour tirer parti de la forte limite de sécurité et de la faible charge de travail variabilty. Microsoft est convaincu que le type de planificateur de base doit être et qu’il continuera à être le type de planification de l’hyperviseur par défaut pour la majorité des scénarios de virtualisation.  Par conséquent, pour garantir la sécurité de nos clients par défaut, Microsoft apporte cette modification pour Windows Server 2019.

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>Impact sur les performances du Planificateur principal sur les charges de travail invitées

Bien qu’il soit nécessaire d’atténuer efficacement certaines classes de vulnérabilités, le planificateur de base peut également potentiellement réduire les performances. Les clients peuvent constater une différence entre les caractéristiques de performances et leurs machines virtuelles et leur impact sur la capacité globale de la charge de travail de leurs hôtes de virtualisation. Dans les cas où le Planificateur principal doit exécuter un vice-président non-SMT, seul l’un des flux d’instructions du cœur logique sous-jacent s’exécute alors que l’autre doit être laissé inactif. Cela limite la capacité totale de l’ordinateur hôte pour les charges de travail invitées.

Ces impacts sur les performances peuvent être réduits en suivant les instructions de déploiement dans ce document. Les administrateurs d’ordinateurs hôtes doivent prendre en compte soigneusement leurs scénarios de déploiement de virtualisation spécifiques et équilibrer leur tolérance pour les risques de sécurité par rapport au besoin de densité de charge de travail maximale, de consolidation des hôtes de virtualisation, etc.

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Modifications apportées aux configurations par défaut et recommandées pour Windows Server 2016 et Windows Server 2019

Le déploiement d’ordinateurs hôtes Hyper-V avec la position de sécurité maximale nécessite l’utilisation du type de planificateur du noyau de l’hyperviseur. Pour garantir la sécurité de nos clients par défaut, Microsoft modifie les paramètres par défaut et recommandés suivants.

>[!NOTE]
>Alors que la prise en charge interne de l’hyperviseur pour les types de planificateur était incluse dans la version initiale de Windows Server 2016, Windows Server 1709 et Windows Server 1803, des mises à jour sont nécessaires pour accéder au contrôle de configuration qui permet de sélectionner le type de planificateur d’hyperviseur.  Pour plus d’informations sur ces mises à jour [, consultez comprendre et utiliser les types de planificateur de l’hyperviseur Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) .

### <a name="virtualization-host-changes"></a>Modifications de l’hôte de virtualisation

* L’hyperviseur utilisera par défaut le Planificateur principal à partir de Windows Server 2019.

* Microsoft reccommends la configuration du planificateur de base sur Windows Server 2016. Le type de planificateur du noyau de l’hyperviseur est pris en charge dans Windows Server 2016, mais la valeur par défaut est le planificateur classique. Le Planificateur principal est facultatif et doit être explicitement activé par l’administrateur de l’hôte Hyper-V.

### <a name="virtual-machine-configuration-changes"></a>Modifications de la configuration des machines virtuelles

* Sur Windows Server 2019, les nouvelles machines virtuelles créées à l’aide de la version 9,0 de la machine virtuelle par défaut héritent automatiquement des propriétés SMT (activées ou désactivées) de l’hôte de virtualisation. Autrement dit, si SMT est activé sur l’hôte physique, les machines virtuelles nouvellement créées disposent également de SMT activée, et héritent de la topologie SMT de l’hôte par défaut, avec la machine virtuelle qui a le même nombre de threads matériels par cœur que le système sous-jacent. Cela sera reflété dans la configuration de la machine virtuelle avec HwThreadCountPerCore = 0, où 0 indique que la machine virtuelle doit hériter des paramètres SMT de l’hôte.

* Les machines virtuelles existantes avec une version de machine virtuelle de 8,2 ou antérieure conservent leur paramètre de processeur d’ordinateur virtuel d’origine pour HwThreadCountPerCore, et la valeur par défaut de la version de machine virtuelle 8,2 invités est HwThreadCountPerCore = 1. Lorsque ces invités s’exécutent sur un ordinateur hôte Windows Server 2019, ils sont traités comme suit :

    1. Si la machine virtuelle a un nombre de VP inférieur ou égal au nombre de cœurs LP, la machine virtuelle sera traitée comme une machine virtuelle non-SMT par le Planificateur principal. Lorsque le VP invité s’exécute sur un seul thread SMT, le thread SMT frère du noyau est inactif. Cela n’est pas optimal et entraîne une perte de performances globale.

    2. Si la machine virtuelle a plus de VPs que de cœurs LP, la machine virtuelle sera traitée comme une machine virtuelle SMT par le Planificateur principal. Toutefois, la machine virtuelle n’observera pas d’autres indications qu’il s’agit d’une machine virtuelle SMT. Par exemple, l’utilisation de l’instruction CPUID ou des API Windows pour interroger le processeur topologie par le système d’exploitation ou les applications n’indique pas que SMT est activé.

* Quand une machine virtuelle existante est mise à jour explicitement à partir des versions de machine virtuelle la vers la version 9,0 via l’opération de mise à jour-machine virtuelle, la machine virtuelle conserve sa valeur actuelle pour HwThreadCountPerCore.  La machine virtuelle n’est pas compatible avec la force SMT.

* Sur Windows Server 2016, Microsoft recommande d’activer SMT pour les machines virtuelles invitées.  Par défaut, SMT est désactivé sur les machines virtuelles créées sur Windows Server 2016, HwThreadCountPerCore a la valeur 1, sauf si elle a été modifiée explicitement.

>[!NOTE]
>Windows Server 2016 ne prend pas en charge la définition de HwThreadCountPerCore sur 0.

#### <a name="managing-virtual-machine-smt-configuration"></a>Gestion de la configuration de l’ordinateur virtuel

La configuration de l’ordinateur virtuel invité est définie pour chaque machine virtuelle. L’administrateur de l’hôte peut inspecter et configurer la configuration SMT d’une machine virtuelle pour effectuer une sélection parmi les options suivantes :

    1. Configurer des machines virtuelles pour qu’elles s’exécutent en tant que SMT, en héritant éventuellement de la topologie du SMT hôte automatiquement

    2. Configurer des machines virtuelles pour qu’elles s’exécutent en tant que non-SMT

Le configuration SMT pour une machine virtuelle s’affiche dans les volets Résumé de la console du Gestionnaire Hyper-V.  La configuration des paramètres SMT d’une machine virtuelle peut être effectuée à l’aide des paramètres de machine virtuelle ou de PowerShell.

#### <a name="configuring-vm-smt-settings-using-powershell"></a>Configuration des paramètres de machine virtuelle SMT à l’aide de PowerShell

Pour configurer les paramètres SMT pour une machine virtuelle invitée, ouvrez une fenêtre PowerShell avec des autorisations suffisantes, puis tapez :

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Où :

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Pour lire les paramètres SMT d’une machine virtuelle invitée, ouvrez une fenêtre PowerShell avec des autorisations suffisantes, puis tapez :

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Notez que les machines virtuelles invitées configurées avec HwThreadCountPerCore = 0 indiquent que SMT est activé pour l’invité et exposent le même nombre de threads SMT à l’invité comme sur l’hôte de virtualisation sous-jacent, en général 2.

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>Les machines virtuelles invitées peuvent observer les modifications apportées à la topologie de l’UC dans les scénarios de mobilité

Le système d’exploitation et les applications d’une machine virtuelle peuvent voir les modifications apportées aux paramètres d’ordinateur hôte et de machine virtuelle avant et après les événements de cycle de vie de la machine virtuelle, tels que les opérations de migration dynamique ou d’enregistrement Au cours d’une opération dans laquelle l’état de la machine virtuelle est enregistré et restauré, le paramètre HwThreadCountPerCore de la machine virtuelle et la valeur réalisée (autrement dit, la combinaison calculée du paramètre de la machine virtuelle et de la configuration de l’hôte source) sont migrés. La machine virtuelle continue de s’exécuter avec ces paramètres sur l’ordinateur hôte de destination. Au moment où la machine virtuelle est arrêtée et redémarrée, il est possible que la valeur réalisée observée par la machine virtuelle change. Cela doit être Bénin, car le système d’exploitation et le logiciel de la couche d’application doivent rechercher les informations de topologie de l’UC dans le cadre de leurs flux de démarrage et de code d’initialisation normaux. Toutefois, étant donné que ces séquences d’initialisation du temps de démarrage sont ignorées pendant les opérations de migration en direct ou d’enregistrement/restauration, les machines virtuelles qui subissent ces transitions d’État peuvent observer la valeur réalisée initialement calculée jusqu’à ce qu’elles soient arrêtées et redémarrées.  

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>Alertes relatives aux configurations de machine virtuelle non optimales

Les machines virtuelles configurées avec plus de VPs qu’il n’y a de cœurs physiques sur l’ordinateur hôte entraînent une configuration non optimale. Le planificateur d’hyperviseur traitera ces machines virtuelles comme si elles étaient compatibles avec la technologie SMT. Toutefois, le système d’exploitation et les logiciels d’application de la machine virtuelle sont présentés dans une topologie de processeur qui indique que SMT est désactivé. Lorsque cette condition est détectée, le processus de travail Hyper-V enregistre un événement sur l’hôte de virtualisation en avertissant l’administrateur de l’ordinateur hôte que la configuration de la machine virtuelle n’est pas optimale et recommande l’activation de SMT pour la machine virtuelle.

#### <a name="how-to-identify-non-optimally-configured-vms"></a>Comment identifier les machines virtuelles configurées de manière non optimale

Vous pouvez identifier les machines virtuelles non-SMT en examinant le journal système dans observateur d’événements pour l’ID d’événement 3498 du processus de travail Hyper-V, qui sera déclenché pour une machine virtuelle chaque fois que le nombre de VPs dans la machine virtuelle est supérieur au nombre de cœurs physiques. Les événements du processus de travail peuvent être obtenus à partir de observateur d’événements ou via PowerShell.

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>Interrogation de l’événement de machine virtuelle du processus de travail Hyper-V à l’aide de PowerShell

Pour Rechercher l’ID d’événement 3498 du processus de travail Hyper-V à l’aide de PowerShell, entrez les commandes suivantes à partir d’une invite PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>Impact de l’invité SMT configuration sur l’utilisation des avantages de l’hyperviseur pour les systèmes d’exploitation invités

L’hyperviseur Microsoft offre plusieurs conseils, ou indicateurs, que le système d’exploitation s’exécutant sur une machine virtuelle invitée peut interroger et utiliser pour déclencher des optimisations, telles que celles qui peuvent tirer parti des performances ou améliorer la gestion de diverses conditions lors de l’exécution virtualisé. Un problème récemment apparu concerne la gestion de la planification du processeur virtuel et l’utilisation des atténuations du système d’exploitation pour les attaques de canaux secondaires qui exploitent SMT.

>[!NOTE]
>Microsoft recommande que les administrateurs d’hôtes activent SMT pour les machines virtuelles invitées pour optimiser les performances de la charge de travail.

Les détails de cette information de l’invité sont fournis ci-dessous, mais le principal obligé pour les administrateurs des hôtes de virtualisation est que les ordinateurs virtuels doivent avoir des HwThreadCountPerCore configurés pour correspondre à la configuration du SMT physique de l’ordinateur hôte. Cela permet à l’hyperviseur de signaler qu’il n’existe aucun partage de noyau non architectural. Par conséquent, tout système d’exploitation invité prenant en charge les optimisations qui requièrent l’activation peut être activé. Sur Windows Server 2019, créez de nouvelles machines virtuelles et laissez la valeur par défaut HwThreadCountPerCore (0). Les machines virtuelles plus anciennes migrées à partir des hôtes Windows Server 2016 peuvent être mises à jour vers la version de configuration de Windows Server 2019. Après cela, Microsoft recommande de définir HwThreadCountPerCore = 0.  Sur Windows Server 2016, Microsoft recommande de définir HwThreadCountPerCore pour qu’il corresponde à la configuration de l’hôte (en général, 2).

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>Détails de l’NoNonArchitecturalCoreSharing

À partir de Windows Server 2016, l’hyperviseur définit une nouvelle assistance pour décrire sa gestion de la planification et de l’emplacement du VP au système d’exploitation invité. Cette recommandation est définie dans la [spécification fonctionnelle de niveau supérieur de l’hyperviseur v 5.0 c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

Hyperviseur CPUID feuille cpuid. 0x40000004. EAX : 18 [NoNonArchitecturalCoreSharing = 1] indique qu’un processeur virtuel ne partagera jamais un cœur physique avec un autre processeur virtuel, sauf pour les processeurs virtuels qui sont signalés en tant que SMT thèmes. Par exemple, un vice-président invité ne s’exécutera jamais sur un thread SMT en même temps qu’un VP racine exécuté simultanément sur un thread SMT frère sur le même cœur de processeur. Cette condition est uniquement possible lors de l’exécution virtualisée et représente donc un comportement SMT non architectural qui a également des implications graves en matière de sécurité. Le système d’exploitation invité peut utiliser NoNonArchitecturalCoreSharing = 1 pour indiquer qu’il est possible d’activer des optimisations, ce qui peut permettre d’éviter la surcharge de performances liée à la définition de STIBP.

Dans certaines configurations, l’hyperviseur n’indique pas que NoNonArchitecturalCoreSharing = 1. Par exemple, si SMT est activé pour un hôte et configuré pour utiliser le planificateur Classic hyperviseur, NoNonArchitecturalCoreSharing sera 0. Cela peut empêcher les invités activés d’activer certaines optimisations. Par conséquent, Microsoft recommande que les administrateurs d’hôtes utilisant SMT s’appuient sur le planificateur du noyau de l’hyperviseur et s’assurent que les machines virtuelles sont configurées pour hériter de leur configuration SMT de l’hôte afin de garantir des performances optimales.

## <a name="summary"></a>Récapitulatif

Le paysage des menaces de sécurité continue à évoluer. Pour garantir la sécurité par défaut de nos clients, Microsoft change la configuration par défaut de l’hyperviseur et des machines virtuelles à partir de Windows Server 2019 Hyper-V, et fournit des conseils et recommandations mis à jour pour les clients exécutant Windows. Serveur 2016 Hyper-V. Les administrateurs de l’hôte de virtualisation doivent :

* Lisez et comprenez les conseils fournis dans ce document

* Évaluez et ajustez soigneusement leurs déploiements de virtualisation pour vous assurer qu’ils répondent aux objectifs de sécurité, de performances, de densité de virtualisation et de réactivité de la charge de travail pour leurs besoins uniques.

* Envisagez de reconfigurer les ordinateurs hôtes Hyper-V Windows Server 2016 existants pour tirer parti des avantages de sécurité renforcés offerts par le planificateur de noyau de l’hyperviseur.

* Mettre à jour les machines virtuelles non-SMT existantes pour réduire l’impact sur les performances des contraintes de planification imposées par l’isolation du VP qui résout les vulnérabilités de sécurité matérielle
