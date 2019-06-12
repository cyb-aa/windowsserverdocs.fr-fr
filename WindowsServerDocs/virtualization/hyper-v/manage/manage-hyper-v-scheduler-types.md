---
title: Comprendre et utiliser des types de planificateur d’hyperviseur Hyper-V
description: Fournit des informations pour les administrateurs d’hôtes Hyper-V sur l’utilisation du Planificateur de Hyper-V de modes
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: c0c2f85fbbeca9e8ac5d40bbcb71f286fabfb65c
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501670"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>La gestion des types de planificateur d’hyperviseur Hyper-V

>S'applique à : Windows 10, Windows Server 2016, Windows Server, version 1709, Windows Server, version 1803, Windows Server 2019

Cet article décrit les nouveaux modes de processeur virtuel planification logique introduite dans Windows Server 2016. Ces modes ou des types de planificateur, déterminent comment l’hyperviseur Hyper-V alloue et gère le travail entre les processeurs virtuels invités. Un administrateur de l’hôte Hyper-V peut sélectionner les types de planificateur hyperviseur qui conviennent le mieux pour les ordinateurs virtuels invités (machines virtuelles) et configurer les machines virtuelles pour tirer parti de la logique de planification.

>[!NOTE]
>Mises à jour sont nécessaires pour utiliser les fonctionnalités de planificateur hyperviseur décrites dans ce document. Pour plus d’informations, consultez [mises à jour requises](#required-updates).

## <a name="background"></a>Arrière-plan

Avant d’aborder la logique et les contrôles derrière processeur virtuel Hyper-V de planification, il est utile passer en revue les concepts de base abordées dans cet article.

### <a name="understanding-smt"></a>Présentation SMT

Simultanée multithreading ou SMT, est une technique utilisée dans les conceptions de processeur moderne qui permet aux ressources du processeur devant être partagé par les threads d’exécution distinct et indépendant. En règle générale, SMT offre une amélioration des performances idéales pour la plupart des charges de travail en parallélisant calculs dans la mesure du possible, augmentation du débit de l’instruction, même si aucun performances obtenir ou même une légère perte de performances peuvent se produire lors de la contention entre les threads pour ressources du processeur partagé se produit.
Processeurs prenant en charge de SMT sont disponibles à partir d’Intel et AMD. Intel fait référence à leurs offres SMT en tant que technologie Hyper-Threading d’Intel ou HT d’Intel.

Dans le cadre de cet article, les descriptions de SMT et comment il est utilisé par Hyper-V s’appliquent également aux systèmes Intel et AMD.

* Pour plus d’informations sur la technologie HT Intel, consultez [technologie Hyper-Threading d’Intel](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* Pour plus d’informations sur AMD SMT, reportez-vous à [l’Architecture de base « Zen »](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>Comprendre comment Hyper-V virtualise les processeurs

Avant de considérer les types de planificateur que hyperviseur, il est également utile de comprendre l’architecture Hyper-V. Vous trouverez un résumé général dans [vue d’ensemble de la technologie Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Il s’agit des concepts importants pour cet article :

* Hyper-V crée et gère les partitions de la machine virtuelle, sur le calcul des ressources sont alloués et partagés, sous contrôle de l’hyperviseur. Les partitions offrent des limites d’isolation renforcée entre toutes les machines de virtuelles invitées et entre les machines virtuelles invitées et la partition racine.

* La partition racine est lui-même une partition de la machine virtuelle, bien qu’il ait des propriétés uniques et bien supérieure privilèges que les machines virtuelles invitées. La partition racine fournit les services de gestion qui contrôlent toutes les machines virtuelles d’invité fournit la prise en charge de l’appareil virtuel pour les invités et gère toutes les e/s de périphérique pour les machines virtuelles invitées. Microsoft vous recommande fortement de ne pas en cours d’exécution de charges de travail d’application dans la partition racine.

* Chaque processeur virtuel (VP) de la partition racine est mappé 1:1 pour un processeur logique sous-jacent (LP). Un hôte VP s’exécute toujours sur le même LP sous-jacente : il n’existe aucune migration de VPs la partition racine.

* Par défaut, les LPs sur lequel exécuter hôte VPs peuvent également exécuter des VPs d’invité.

* Un vice-président invité peut être planifié par l’hyperviseur pour s’exécuter sur n’importe quel processeur logique disponible. Alors que le Planificateur de l’hyperviseur prend soin d’envisager localité de cache temporelle, topologie NUMA et beaucoup d’autres facteurs lors de la planification de vice-président des invités, au final le vice-président peut être planifiée sur n’importe quel hôte LP.

## <a name="hypervisor-scheduler-types"></a>Types de planificateur d’hyperviseur

À compter de Windows Server 2016, l’hyperviseur Hyper-V prend en charge plusieurs modes de logique de planificateur, qui déterminent comment l’hyperviseur planifie les processeurs virtuels sur les processeurs logiques sous-jacents. Ces types de planificateur sont :

- [Le planificateur classique, le partage équitable](#the-classic-scheduler)
- [Le Planificateur de noyau](#the-core-scheduler)
- [Le Planificateur de racine](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>Le planificateur classique

Le planificateur classique a été la valeur par défaut pour toutes les versions de l’hyperviseur Hyper-V de Windows depuis sa création, y compris Windows Server 2016 Hyper-V. Le planificateur classic fournit une répartition de charge équilibrée, preemptive modèle de planification de tourniquet (round-robin) pour les processeurs virtuels invités.

Le type de planificateur classique est le plus approprié pour la grande majorité des utilisations traditionnelles de Hyper-V – pour les clouds privés, les fournisseurs d’hébergement et ainsi de suite. Les caractéristiques de performances sont bien comprises et sont mieux optimisés pour prendre en charge un large éventail de scénarios de virtualisation, telles que surabonnement des vice-présidents LPs, l’exécution simultanée de plusieurs machines virtuelles et des charges de travail hétérogènes, en cours d’exécution élevé de plus grande échelle performances des machines virtuelles, prenant en charge la fonctionnalité complète ensemble d’Hyper-V sans restrictions et bien plus encore.

### <a name="the-core-scheduler"></a>Le Planificateur de noyau

Le Planificateur de noyau hyperviseur est une nouvelle alternative à la logique de planificateur classique, introduite dans Windows Server 2016 et Windows 10 version 1607. Le planificateur core offre une limite de sécurité renforcée pour l’isolation de la charge de travail invité et la variabilité des performances réduites pour les charges de travail à l’intérieur de machines virtuelles qui sont exécutent sur un hôte de virtualisation prenant en charge SMT. Le planificateur core permet en cours d’exécution des machines virtuelles SMT et non-SMT simultanément sur le même hôte de virtualisation prenant en charge SMT.

Le planificateur core utilise la topologie SMT de l’hôte de virtualisation et éventuellement expose les paires SMT aux machines virtuelles invitées et aux groupes de planifications de processeurs virtuels invités à partir de la même machine virtuelle sur des groupes de processeurs logiques SMT. Cela symétriquement afin que si LPs se trouvent dans des groupes de deux, VPs sont planifiées dans les deux groupes, et un noyau n’est jamais partagé entre les machines virtuelles.
Lorsque le vice-président est planifié pour une machine virtuelle sans SMT activé, que VP consommera les principales lorsqu’elle s’exécute.

Le résultat global du planificateur core est que :

* Invité VPs sont limités à une exécution sur sous-jacent de paires de cœur physique, isoler une machine virtuelle à des limites de cœur de processeur, ce qui réduit la vulnérabilité aux attaques d’espionnage côté canal à partir d’ordinateurs virtuels malveillants.

* Variabilité de débit est considérablement réduite.

* Les performances sont potentiellement réduites, car si seul un groupe de VPs peut exécuter, seul les flux d’instructions dans le noyau exécute alors que l’autre est devenue inactive.

* Le système d’exploitation et les applications en cours d’exécution dans la machine virtuelle invitée peuvent utiliser un comportement SMT et interfaces de programmation (API) pour contrôler et de répartir le travail entre les threads SMT, comme s’ils étaient exécutés quand non virtualisés.

* Une limite de sécurité renforcée pour l’isolation des charges de travail invité - invité VPs sont limités à une exécution sur des paires de cœur physique sous-jacent, ce qui réduit la vulnérabilité aux attaques par espionnage de canal latéral.

Le planificateur core sera utilisé par défaut à compter de Windows Server 2019. Sur Windows Server 2016, le Planificateur de noyau est facultatif et doit être explicitement activé par l’administrateur de l’hôte Hyper-V, et le planificateur classique est la valeur par défaut.

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>Comportement de planificateur principal avec hôte SMT désactivé

Si l’hyperviseur est configuré pour utiliser le type de planificateur core, mais la fonctionnalité SMT est désactivé ou n’existe pas sur l’hôte de virtualisation, l’hyperviseur utilisera le comportement du planificateur classique, quel que soit le paramètre de type de planificateur hyperviseur.

### <a name="the-root-scheduler"></a>Le Planificateur de racine

Le planificateur racine a été introduit avec Windows 10 version 1803. Lorsque le type de planificateur racine est activé, l’hyperviseur cède le contrôle de la planification de travail à la partition racine. Le Planificateur de NT dans l’instance de système d’exploitation de la partition racine gère tous les aspects de la planification de travail au système LPs.

Le planificateur racine répond aux besoins inhérentes à une partition de l’utilitaire de prise en charge pour assurer l’isolation de forte charge de travail, tel qu’utilisé avec Windows Defender Application Guard (WDAG). Dans ce scénario, en laissant la planification des responsabilités à la racine du système d’exploitation offre plusieurs avantages. Par exemple, les contrôles de ressources processeur applicable aux scénarios de conteneur peuvent servir avec la partition de l’utilitaire, ce qui simplifie la gestion et le déploiement. En outre, le planificateur du système d’exploitation racine peut facilement collecter des mesures sur la charge de travail de l’utilisation du processeur à l’intérieur du conteneur et de les utiliser en tant qu’entrée à la même stratégie de planification applicable à tous les autres charges de travail dans le système. Ces mêmes mesures également aider à clairement attribut le travail effectué dans un conteneur d’application pour le système hôte. Ces mesures de suivi est plus difficile avec des charges de travail traditionnels de machines virtuelles, où un travail procuration tous en cours d’exécution de la machine virtuelle a lieu dans la partition racine.

#### <a name="root-scheduler-use-on-client-systems"></a>Utilisation du Planificateur de racine sur les systèmes clients

À compter de Windows 10 version 1803, le planificateur racine est utilisé par défaut sur les systèmes clients uniquement, où l’hyperviseur peut être activée pour prendre en charge la sécurité basée sur la virtualisation et d’isolation de la charge de travail WDAG et au bon fonctionnement de futurs systèmes avec architectures de core hétérogènes. Il s’agit de la configuration du planificateur hyperviseur pris en charge uniquement pour les systèmes clients. Les administrateurs ne devraient pas essayer de substituer le type de planificateur par défaut hyperviseur sur les systèmes clients Windows 10.

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>Les contrôles de ressources du processeur d’ordinateur virtuel et le Planificateur de racine

Les contrôles de ressources de processeur de machine virtuelle fournies par Hyper-V ne sont pas pris en charge lorsque le Planificateur de racine hyperviseur est activé comme logique du planificateur du système d’exploitation racine est la gestion des ressources de l’hôte de manière globale et n’a pas connaissance d’une machine virtuelle paramètres de configuration spécifiques. Les contrôles de ressources processeur de Hyper-V par machine virtuelle, tels que les majuscules, les poids et les réserves de ressources, sont appliquent uniquement lorsque l’hyperviseur contrôle directement VP de planification, comme avec les types de planificateur classique et core.

#### <a name="root-scheduler-use-on-server-systems"></a>Utilisation de planificateur de racine sur des systèmes de serveur

Le Planificateur de racine n’est pas recommandé pour une utilisation avec Hyper-V sur les serveurs pour l’instant, car ses caractéristiques de performance n’ont pas encore été caractérisés entièrement et ajustées pour prendre en charge la large gamme de charges de travail typiques de nombreux déploiements de virtualisation de serveur.

## <a name="enabling-smt-in-guest-virtual-machines"></a>L’activation de SMT dans les machines virtuelles invitées

Une fois que l’hyperviseur de l’hôte de virtualisation est configuré pour utiliser le type de planificateur core, les ordinateurs virtuels invités peuvent être configurés pour utiliser SMT si vous le souhaitez. Le planificateur dans le système d’exploitation invité et les charges de travail en cours d’exécution dans la machine virtuelle pour détecter et utiliser la topologie SMT dans leur propre planification de travail permet d’exposer le fait que VPs sont hyperthreaded à une machine virtuelle invitée. Sur Windows Server 2016, invité SMT n’est pas configurée par défaut et doit être explicitement activée par l’administrateur de l’hôte Hyper-V. À compter de Windows Server 2019, nouvelles machines virtuelles créées sur l’ordinateur hôte hérite topologie SMT de l’hôte par défaut.  Autrement dit, une version que 9.0 de machine virtuelle créée sur un ordinateur hôte avec 2 threads SMT par cœur serait également voir 2 threads SMT par cœur.

PowerShell doit être utilisé pour activer SMT sur une machine virtuelle invitée ; Il n’existe aucune interface utilisateur fourni dans le Gestionnaire Hyper-V.
Pour activer SMT dans une machine virtuelle invitée, ouvrez une fenêtre PowerShell avec des autorisations suffisantes, puis tapez :

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Où <n> est le nombre de threads SMT par cœur de l’invité de machine virtuelle s’affiche.  
Notez que <n> = 0 définit la valeur HwThreadCountPerCore pour faire correspondre le nombre de threads de l’hôte SMT par valeur fondamentale.

>[!NOTE] 
>Paramètre HwThreadCountPerCore = 0 est prise en charge à partir de Windows Server 2019.

Voici un exemple de système d’informations provenant du système d’exploitation invité s’exécutant dans une machine virtuelle avec 2 processeurs virtuels et SMT activé. Le système d’exploitation invité détecte 2 processeurs logiques appartenant à la même cœur.

![Capture d’écran montrant msinfo32 dans un invité de machine virtuelle avec SMT activé](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>Configuration du type de planificateur de hyperviseur sur Windows Server 2016 Hyper-V

Windows Server 2016 Hyper-V utilise le modèle de planificateur d’hyperviseur classique par défaut. L’hyperviseur peut éventuellement être configuré pour utiliser le Planificateur de noyau, pour augmenter la sécurité en limitant les VPs invité à exécuter sur des paires SMT physiques correspondants et pour prendre en charge l’utilisation de machines virtuelles avec la planification de SMT pour leurs VPs invité.

>[!NOTE]
>Microsoft recommande que tous les clients qui exécutent Windows Server 2016 Hyper-V sélectionnez le Planificateur de noyau pour garantir la que protection optimale de leurs hôtes de virtualisation sur les machines virtuelles invitées potentiellement malveillants.

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Valeurs par défaut de Windows Server 2019 Hyper-V à l’aide du Planificateur de noyau

Pour garantir les hôtes Hyper-V sont déployés dans la configuration d’une sécurité optimale, Windows Server 2019 Hyper-V utilisent désormais le modèle planificateur d’hyperviseur core par défaut. L’administrateur de l’hôte peut éventuellement configurer l’hôte pour utiliser le planificateur classic hérité. Les administrateurs doivent soigneusement lire, comprendre et prendre en compte l’impact de que chaque type de planificateur a sur la sécurité et les performances des hôtes de virtualisation avant de remplacer les paramètres par défaut du type du planificateur.  Consultez [sélection du type de présentation d’Hyper-V planificateur](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) pour plus d’informations.

### <a name="required-updates"></a>Mises à jour nécessaires

>[!NOTE]
>Les mises à jour suivantes sont requises pour utiliser les fonctionnalités de planificateur hyperviseur décrites dans ce document. Ces mises à jour incluent des modifications pour prendre en charge de la nouvelle option de BCD 'hypervisorschedulertype', qui est nécessaire pour la configuration de l’hôte.

| Version | Release  | Mise à jour requise | Article de la base de connaissances |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Aucune | Aucune |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>Sélection du type de planificateur hyperviseur sur Windows Server

La configuration du planificateur hyperviseur est contrôlée par le biais de l’entrée de BCD hypervisorschedulertype.

Pour sélectionner un type de planificateur, ouvrez une invite de commandes avec des privilèges d’administrateur :

``` command
     bcdedit /set hypervisorschedulertype type
```

Où `type` est une des :

* Classique
* Standard
* Racine

Le système doit être redémarré pour toutes les modifications vers le type de planificateur hyperviseur entrent en vigueur.

>[!NOTE]
>Le Planificateur de racine hyperviseur n'est pas pris en charge sur Windows Server Hyper-V pour l’instant. Les administrateurs Hyper-V ne doivent pas tenter de configurer le Planificateur de racine pour une utilisation avec les scénarios de virtualisation de serveur.

## <a name="determining-the-current-scheduler-type"></a>Détermination du type de planificateur actuel

Vous pouvez déterminer le type de planificateur hyperviseur actuel en cours d’utilisation en examinant le journal système dans l’Observateur d’événements pour l’événement de lancement hyperviseur plus récent ID 2, ce qui indique le type de planificateur hyperviseur configuré au lancement de l’hyperviseur. Événements de lancement d’hyperviseur peuvent être obtenus à partir de l’Observateur d’événements Windows, ou via PowerShell.

Événement de lancement hyperviseur ID 2 indique le type de planificateur hyperviseur, où :

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Affichage des détails de l’événement ID 2 hyperviseur lancement capture d’écran](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Capture d’écran montrant l’Observateur d’événements affichant l’événement de lancement hyperviseur ID 2](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>Interrogation de l’événement de lancement de type de planificateur Hyper-V hyperviseur à l’aide de PowerShell

À la requête pour l’événement hyperviseur ID 2 à l’aide de PowerShell, entrez les commandes suivantes à partir d’une invite de PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Capture d’écran montrant la requête PowerShell et les résultats de l’événement de lancement hyperviseur ID 2](media/Hyper-V-CoreScheduler-PowerShell.png)
