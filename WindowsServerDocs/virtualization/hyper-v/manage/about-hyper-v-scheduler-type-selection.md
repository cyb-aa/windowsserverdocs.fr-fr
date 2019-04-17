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
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905126"
---
# À propos de la sélection du type de planificateur hyperviseur Hyper-V

S’applique à:

* WindowsServer2016
* WindowsServer, version1709
* WindowsServer, version1803
* Windows Server 2019

Ce document décrit les modifications importantes apportées à la valeur par défaut de Hyper-V et recommandé d’utilisation de l’hyperviseur types planificateur. Ces modifications affecteront à la fois le système de sécurité et la virtualisation les performances. Administrateurs d’hôtes de virtualisation doivent passer en revue et comprendre les modifications et les implications décrites dans ce document et évaluer avec soin les impacts, le Guide de déploiement suggéré et le risque facteurs qui entrent en jeu à mieux comprendre comment déployer et gérer Hôtes Hyper-V sur le paysage de sécurité évolutions.

>[!IMPORTANT]
>Actuellement connues sécurité canal vulnérabilités dans plusieurs architectures de processeur susceptibles d’être exploitées par un invité malveillant machine virtuelle par le biais de la planification comportement du type planificateur classique hyperviseur hérité lorsque s’exécutent sur des hôtes avec simultanée Multithreading (MTS) activé.  Si elle est exploitée avec succès, une charge de travail malveillante pourrait observer les données en dehors de sa limite de partition. Cette classe d’attaques peut être atténuée en configurant l’hyperviseur Hyper-V pour utiliser le type de planificateur de base de l’hyperviseur et l’invité reconfiguration machines virtuelles. Avec le planificateur core, l’hyperviseur restreint PV un invité de la machine virtuelle s’exécute sur le même cœur de processeur physique, par conséquent fortement isolant capacité de l’ordinateur virtuel pour accéder aux données des limites du cœur physique sur lequel elle s’exécute.  Il s’agit d’une atténuation très efficace contre ces attaques de canal, ce qui empêche la machine virtuelle à partir de l’observation tous les artefacts à partir d’autres partitions, indique si la racine ou une autre partition invité.  Par conséquent, Microsoft modifie la valeur par défaut et les paramètres de configuration pour les hôtes de virtualisation et les machines virtuelles invitées recommandés.

## Arrière-plan

À compter de Windows Server 2016, Hyper-V prend en charge plusieurs méthodes de la planification et la gestion des processeurs virtuels, considérés comme des types de planificateur de l’hyperviseur.  Vous trouverez une description détaillée de tous les types de planificateur hyperviseur [comprendre](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)et à l’aide de types de planificateur de l’hyperviseur Hyper-V.

>[!NOTE]
>Nouveaux types de planificateur hyperviseur ont initialement été introduites avec Windows Server 2016 et ne sont pas disponibles dans les versions antérieures. Toutes les versions d’Hyper-V avant Windows Server 2016 prend en charge uniquement le planificateur classique. Prise en charge pour le planificateur core a été que récemment publié.

## À propos des types de planificateur de l’hyperviseur

Cet article se concentre plus particulièrement sur l’utilisation du nouveau type de planificateur hyperviseur core par rapport au planificateur de «classique» hérité, et comment ces types de planificateur intersection avec l’utilisation de symétrique multi-thread ou SMT.  Il est important de comprendre les différences entre les planificateurs core et classique et la façon dont chaque place travailler à partir d’ordinateurs virtuels invités sur les processeurs système sous-jacent.

### Le planificateur classique

Le planificateur classique fait référence à la méthode du tourniquet équitable, de la planification de travail sur les processeurs virtuels (PV) sur le système, y compris PV racine, ainsi que des PV appartenant aux ordinateurs virtuels invités. Le planificateur classique a été le type de planificateur par défaut utilisé sur toutes les versions d’Hyper-V (jusqu'à ce que Windows Server 2019, comme décrit dans le présent document).  Les caractéristiques de performances du planificateur classique encourus, et le planificateur classique est illustré pour prendre en charge ably de surallocation de charges de travail - autrement dit, surallocation de coefficient de VP:LP de l’hôte par une marge raisonnable (en fonction de la types de charges de travail virtualisées en cours, l’utilisation des ressources globale, etc..).

Lorsque vous l’exécutez sur un hôte de virtualisation avec SMT activé, le planificateur classique sera planifier PV invité à partir de n’importe quel ordinateur virtuel sur chaque thread SMT appartenant à un cœur de manière indépendante. Par conséquent, les ordinateurs virtuels différents peuvent s’exécuter sur le même cœur en même temps (une ordinateur virtuel en cours d’exécution sur un thread d’un noyau pendant un autre ordinateur virtuel est en cours d’exécution sur le thread d’autre).

### Le Planificateur de core

Le Planificateur de core s’appuie sur les propriétés de SMT pour assurer l’isolation des charges de travail invité, ce qui a une incidence sur la sécurité et les performances du système. Le Planificateur de core garantit que PV à partir d’une machine virtuelle est planifiés sur des threads SMT sœur. Cette opération est effectuée symétriques afin que si PL se trouvent dans des groupes de deux, PV sont planifiés dans des groupes de deux, et un noyau du système du processeur n’est jamais partagé entre les ordinateurs virtuels.

En planifiant PV invité sur des paires SMT sous-jacente, le Planificateur de core offre une limite de sécurité renforcée pour l’isolation de charge de travail et peut également servir à réduire la variabilité de performances pour les charges de travail sensibles de latence.

Notez que paramètre lorsque le vice-président est planifiée pour une machine virtuelle sans SMT activé, que vice-président utilisera les principales lorsqu’elle s’exécute, et frère du cœur thread SMT restera inactif.  Cela est nécessaire pour fournir l’isolation de charge de travail correct, mais a une incidence sur les performances globales du système, en particulier comme les PL système trop sollicité - autrement dit, lorsque deviennent total VP:LP coefficient dépasse 1:1. Par conséquent, en cours d’exécution machines virtuelles invitées configurés sans plusieurs threads par core est une configuration non optimale.

### Avantages de l’utilisation du Planificateur de core

Le Planificateur de core offre les avantages suivants:

* Une limite de sécurité renforcée pour l’isolation de charge de travail invité - invité PV sont limitées à s’exécuter sur des paires de cœur physique sous-jacent, réduire la vulnérabilité aux attaques snooping de canal.

* Charge de travail réduite variabilité - variabilité de débit de la charge de travail invité est considérablement réduite, offre une plus grande cohérence de charge de travail.

* Utilisation de SMT dans invité machines virtuelles - le système d’exploitation et les applications en cours d’exécution sur l’ordinateur virtuel invité peut utiliser le comportement SMT et (API) pour contrôler et distribuer le travail sur les threads MTS, tout comme ils s’exécute lorsque des interfaces de programmation non virtualisés.

Le Planificateur de core est actuellement utilisé sur les hôtes de virtualisation Azure, spécifiquement pour tirer parti de la limite de sécurité renforcée et variabilty faible charge de travail. Microsoft pense que le type de planificateur core doit être et continue d’être l’hyperviseur par défaut type pour la plupart des scénarios de virtualisation de planification.  Par conséquent, pour vous assurer que nos clients sont sécurisées par défaut, Microsoft est cette modification pour Windows Server 2019 maintenant.

### Impacts sur les performances du planificateur Core sur les charges de travail invité

Tandis que nécessaire pour minimiser efficacement certaines classes de vulnérabilités, le Planificateur de core peut également potentiellement réduire les performances. Les clients peuvent voir une différence dans les caractéristiques de performances avec leurs ordinateurs virtuels et l’impact de la capacité globale de la charge de travail de leurs ordinateurs hôtes de virtualisation. Dans les cas où le planificateur core doit s’exécuter un vice-président non - SMT, uniquement un des flux instruction dans le cœur logique sous-jacent exécute tandis que l’autre doit rester inactif. Cela limite la capacité de l’hôte total pour les charges de travail invité.

Ces impacts sur les performances peuvent être réduits en suivant les instructions de déploiement dans ce document. Les administrateurs d’hôtes doivent soigneusement prendre en compte leur virtualisation spécifique de scénarios de déploiement et équilibrer leur tolérance de pannes pour les risques de sécurité et la nécessité de densité de charge de travail maximale, dépassement consolidation des hôtes de virtualisation, etc..

## Modifications apportées à la valeur par défaut et configurations recommandées pour Windows Server 2016 et Windows Server 2019

Déployer des hôtes Hyper-V avec le sécuritaire maximale nécessite l’utilisation du type planificateur hyperviseur core. Pour vous assurer que nos clients sont sécurisées par défaut, Microsoft modifie la valeur par défaut suivant et les paramètres recommandés.

>[!NOTE]
>Tandis que la prise en charge interne de l’hyperviseur pour les types de planificateur a été inclus dans la version initiale de Windows Server 2016, Windows Server 1709 et Windows Server 1803, les mises à jour sont nécessaires pour accéder au contrôle de configuration, ce qui permet de sélectionner le type du Planificateur de l’hyperviseur.  Pour plus d’informations sur ces mises à jour, reportez-vous à [comprendre et à l’aide de types de planificateur de l’hyperviseur Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) .

### Changements d’hôte de virtualisation

* L’hyperviseur utiliseront le planificateur core par en commençant par défaut par Windows Server 2019.

* Microsoft reccommends configuration le planificateur core sur Windows Server 2016. Le type de planificateur hyperviseur core est pris en charge dans Windows Server 2016, toutefois, la valeur par défaut est le planificateur classique. Le Planificateur de core est facultatif et doit être activé explicitement par l’administrateur de l’hôte Hyper-V.

### Modifications de configuration de machine virtuelle

* Sur Windows Server 2019, nouveaux ordinateurs virtuels créés à l’aide de la version de machine virtuelle par défaut 9.0 héritera automatiquement les propriétés SMT (activé ou désactivé) de l’hôte de virtualisation. Autrement dit, si SMT est activé sur l’hôte physique, qui vient d’être créé machines virtuelles ont également SMT activé et héritera de la topologie SMT de l’hôte par défaut, avec l’ordinateur virtuel ayant le même nombre de threads matériels par cœur que le système sous-jacent. Cela est répercutée dans la configuration de l’ordinateur virtuel avec HwThreadCountPerCore = 0, où 0 indique les paramètres de l’hôte SMT doit hériter de l’ordinateur virtuel.

* Les ordinateurs virtuels existants avec une version de la machine virtuelle de sera 8.2 ou une version antérieure conserver sa configuration de processeur de machine virtuelle d’origine pour HwThreadCountPerCore, et la valeur par défaut pour 8.2 invités de version d’ordinateur virtuel est HwThreadCountPerCore = 1. Lorsque ces invités s’exécuter sur un hôte Windows Server 2019, ils seront traités comme suit:

    1. Si la machine virtuelle dispose d’un nombre vice-président qui est inférieure ou égale au nombre de cœurs LP, l’ordinateur virtuel sera traitée comme un non - SMT VM par le Planificateur de base. Lorsque le vice-président invité s’exécute sur un seul thread SMT, frère du cœur thread SMT s’être anticipée ne devienne inactif. Cela est non optimale et se traduit par une perte globale des performances.

    2. Si l’ordinateur virtuel a PV plus que les cœurs LP, la machine virtuelle est considérée comme une VM SMT par le Planificateur de base. Toutefois, l’ordinateur virtuel ne sera pas équilibrée autres indications qu’il s’agit d’une VM SMT. Par exemple, utilisation de l’instruction CPUID ou les API Windows pour interroger la topologie de processeur par le système d’exploitation ou les applications non indique que SMT est activée.

* Lorsqu’une machine virtuelle existante est explicitement mis à jour à partir des versions eariler à la version 9.0 par le biais de l’opération de mise à jour-VM, l’ordinateur virtuel conserve sa valeur actuelle pour HwThreadCountPerCore.  La machine virtuelle ne peut pas avoir SMT prenant en vigueur.

* Sur Windows Server 2016, Microsoft recommande d’activer SMT pour les machines virtuelles invitées.  Par défaut, les ordinateurs virtuels créés sur Windows Server 2016 serait avez SMT désactivé, qui est que hwthreadcountpercore est définie sur 1, sauf si modifié de manière explicite.

>[!NOTE]
>Windows Server 2016 ne prend pas en charge HwThreadCountPerCore valeur 0 au paramètre.

#### Gestion de la machine virtuelle SMT configuration

La configuration de SMT de machine virtuelle invité est définie sur une base par machines virtuelles. L’administrateur de l’hôte peut examiner et configuration des SMT d’une machine virtuelle pour sélectionner parmi les options suivantes:

    1. Configurer des ordinateurs virtuels à s’exécuter comme SMT compatible, si vous le souhaitez hériter automatiquement la topologie SMT hôte

    2. Configurer des ordinateurs virtuels à s’exécuter en tant que non-SMT

Les personnels SMT pour un ordinateur virtuel s’affiche dans les volets résumées dans la console Gestionnaire Hyper-V.  Configuration des paramètres de SMT d’une machine virtuelle peut être effectuée à l’aide de l’ordinateur virtuel paramètres ou PowerShell.

#### Configuration des paramètres SMT VM à l’aide de PowerShell

Pour configurer les paramètres SMT pour une machine virtuelle invitée, ouvrez une fenêtre PowerShell avec des autorisations suffisantes et le type:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Où:

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Pour lire les paramètres SMT pour une machine virtuelle invitée, ouvrez une fenêtre PowerShell avec des autorisations suffisantes et le type:

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Notez qu’invité machines virtuelles configurés avec HwThreadCountPerCore = 0 indique que SMT sera activée pour l’invité et exposera le même nombre de threads MTS à l’invité qu’ils sont sur l’hôte de virtualisation sous-jacente, en règle générale, 2.

### Machines virtuelles invitées peuvent observer les modifications de topologie de processeur sur les scénarios de mobilité de machine virtuelle

Le système d’exploitation et les applications dans une machine virtuelle peuvent voir des modifications à la fois hôte et les paramètres de machine virtuelle avant et après que les événements de cycle de vie de machine virtuelle telles que la migration dynamique ou enregistrement et de restauration. Lors d’une opération dans le VM état est enregistrée et restaurée, les paramètres l’ordinateur virtuel HwThreadCountPerCore et la valeur réalisée (autrement dit, la combinaison de calculée de paramètre de la machine virtuelle et configuration de l’hôte de la source) sont migrés. La machine virtuelle continue à s’exécuter avec ces paramètres sur l’hôte de destination. Au point de l’ordinateur virtuel est arrêté et redémarré, il est possible que la valeur réalisée observée par l’ordinateur virtuel changera. Il doit s’agir bénigne, en tant que système d’exploitation et application logiciel de couche doit se présenter pour les informations sur la topologie du processeur dans le cadre de leur flux de code l’initialisation et démarrage normal. Toutefois, étant donné que ces initialisation de temps de démarrage séquences sont ignorés lors des opérations de migration ou de sauvegarde/restauration dynamiques, les ordinateurs virtuels qui sont soumis à ces transitions d’état pu observer l’à l’origine calculée réalisés valeur jusqu'à ce qu’ils sont arrêtés et redémarré.  

### Alertes concernant des configurations d’ordinateur virtuel non optimale

Machines virtuelles configurées avec PV plus qu’il ne sont cœurs physiques sur le résultat de l’hôte dans une configuration non optimale. Le Planificateur de l’hyperviseur traite ces machines virtuelles comme si elles sont SMT prenant en charge. Toutefois, système d’exploitation et les logiciels d’application dans l’ordinateur virtuel s’affiche une topologie de processeur montrant SMT est désactivé. Lorsque cette condition est détectée, le processus de travail Hyper-V consigne un événement sur l’hôte de virtualisation avertissement l’administrateur hôte que la configuration de l’ordinateur virtuel est non optimale, et si vous recommandez SMT être activé pour la machine virtuelle.

#### Comment identifier de manière non optimale configuré des machines virtuelles

Vous pouvez identifier non - SMT machines virtuelles en examinant le journal système dans l’Observateur d’événements pour les événements de processus de travail Hyper-V 3498 ID, lequel sera déclenché pour un ordinateur virtuel chaque fois que le nombre de PV sur la machine virtuelle est supérieur au nombre de cœur physique. Événements de processus de travail peuvent être obtenues à partir de l’Observateur d’événements, ou via PowerShell.

#### Interrogation de l’événement de machine virtuelle de processus de travail Hyper-V à l’aide de PowerShell

Requête pour les événements de processus de travail Hyper-V 3498 ID à l’aide de PowerShell, entrez les commandes suivantes à partir d’une invite de commandes PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### Impacts d’invité SMT personnels sur l’utilisation d’hyperviseur enlightments pour les systèmes d’exploitation invités

L’hyperviseur Microsoft propose plusieurs enlightments ou des conseils, lequel le système d’exploitation en cours d’exécution sur un ordinateur virtuel invité peut-être interroger et l’utiliser pour déclencher les optimisations, telles que celles qui peuvent bénéficier de performances ou dans le cas contraire améliorant la gestion des différentes conditions lors de l’exécution virtualisé. Probables récemment introduit un concerne la gestion de la planification de processeur virtuel et l’utilisation des mesures de prévention du système d’exploitation pour les attaques de canal qui exploitent SMT.

>[!NOTE]
>Microsoft recommande que les administrateurs d’hôtes activent SMT pour les ordinateurs virtuels invités optimiser les performances de la charge de travail.

Les détails de ce probables invité sont fournis ci-dessous, cependant la clé à retenir pour les administrateurs d’hôtes de virtualisation est que les machines virtuelles doivent avoir HwThreadCountPerCore configurée pour correspondre à la configuration SMT physique de l’hôte. Cela permet à l’hyperviseur signaler qu’il n’existe aucune core-architecturaux le partage. Par conséquent, n’importe quel optimisations de prise en charge du système d’exploitation invité qui nécessitent la probables peuvent être activées. Sur Windows Server 2019, créer de nouveaux ordinateurs virtuels et laissez la valeur par défaut de HwThreadCountPerCore (0). Machines virtuelles plus anciennes migrés à partir de Windows Server 2016 hôtes peuvent être mis à jour vers la version de configuration de Windows Server 2019. Après avoir effectué par conséquent, Microsoft recommande définissant HwThreadCountPerCore = 0.  Sur Windows Server 2016, Microsoft vous recommande de paramètre HwThreadCountPerCore pour correspondre à la configuration d’hôte (en règle générale, 2).

### Détails de compatibilité NoNonArchitecturalCoreSharing

À compter de Windows Server 2016, l’hyperviseur définit un nouveau probables pour décrire sa gestion des vice-président planification et le placement du système d’exploitation invité. Cette probables est défini dans la [spécification fonctionnelle de l’hyperviseur haut niveau v5.0c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

Hyperviseur synthétique CPUID nœud terminal CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] indique qu’un processeur virtuel jamais partagent un cœur physique avec un autre processeur virtuel, à l’exception des processeurs virtuels qui sont signalés comme frère SMT threads. Par exemple, un vice-président invité jamais exécutera sur un thread SMT en parallèle avec un vice-président racine en cours d’exécution simultanément sur un thread SMT sur le même cœur de processeur frère. Cette condition n’est possible que lors de l’exécution virtualisé et par conséquent, représente un comportement SMT-architecturaux qui a également des implications de sécurité graves. La système d’exploitation invité peut utiliser NoNonArchitecturalCoreSharing = 1 en tant qu’une indication qu’il est sûr d’activer les optimisations, ce qui peuvent l’aider éviter la surcharge de performances de la définition de STIBP.

Dans certaines configurations, l’hyperviseur n’indique pas cette NoNonArchitecturalCoreSharing = 1. Par exemple, si un hôte SMT activé et est configuré pour utiliser le Planificateur de classique de l’hyperviseur, NoNonArchitecturalCoreSharing est égal à 0. Cela peut empêcher les invités compatibles de l’activation de certaines optimisations. Par conséquent, Microsoft recommande que les administrateurs d’hôtes à l’aide de SMT s’appuient sur le Planificateur de core hyperviseur et vous assurer que les machines virtuelles sont configurés pour leur configuration SMT hériter de l’hôte pour garantir des performances optimales de la charge de travail.

## Résumé

Le paysage des menaces de sécurité continue d’évoluer. Pour vous assurer que nos clients sont sécurisées par défaut, Microsoft modifie la configuration par défaut pour l’hyperviseur et des machines virtuelles à partir de Windows Server 2019 Hyper-V, et en fournissant mis à jour les informations et des recommandations pour les clients exécutant Windows Server 2016 Hyper-V. Les administrateurs hôte de virtualisation doivent:

* Lire et comprendre les instructions fournies dans ce document

* Évaluer soigneusement et d’ajuster des leurs déploiements de la virtualisation pour vérifier qu’ils répondent à la sécurité, performances, la densité de la virtualisation et objectifs de réactivité de charge de travail pour leurs besoins uniques

* Prendre en compte la reconfiguration des hôtes Windows Server 2016 Hyper-V existants pour tirer parti des avantages une sécurité renforcée offerts par le Planificateur de core hyperviseur

* Mettre à jour non - SMT machines virtuelles existantes pour réduire l’impact sur les performances de la planification des limites imposées par isolement vice-président qui résout les vulnérabilités de sécurité matériel
