---
title: Comprendre et utiliser les types de planificateur de l’hyperviseur Hyper-V
description: Fournit des informations pour les administrateurs d’ordinateurs hôtes Hyper-V sur l’utilisation des modes de planification d’Hyper-V
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: c7c2de8354d067faf0dcf1787c3e178421e2ac03
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872031"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>Gestion des types de planificateur de l’hyperviseur Hyper-V

>S'applique à : Windows 10, Windows Server 2016, Windows Server, version 1709, Windows Server, version 1803, Windows Server 2019

Cet article décrit les nouveaux modes de logique de planification de processeur virtuel introduits dans Windows Server 2016. Ces modes, ou types de planificateur, déterminent la façon dont l’hyperviseur Hyper-V alloue et gère le travail entre les processeurs virtuels invités. Un administrateur de l’hôte Hyper-V peut sélectionner des types de planificateur d’hyperviseur qui conviennent le mieux aux machines virtuelles invitées et à configurer les machines virtuelles pour tirer parti de la logique de planification.

>[!NOTE]
>Les mises à jour sont nécessaires pour utiliser les fonctionnalités du planificateur d’hyperviseur décrites dans ce document. Pour plus d’informations, consultez [mises à jour requises](#required-updates).

## <a name="background"></a>Présentation

Avant de discuter de la logique et des contrôles de la planification du processeur virtuel Hyper-V, il est utile de passer en revue les concepts de base abordés dans cet article.

### <a name="understanding-smt"></a>Compréhension de SMT

Le multithreading simultané, ou SMT, est une technique utilisée dans les conceptions de processeur modernes qui permettent aux ressources du processeur d’être partagées par des threads d’exécution distincts et indépendants. En général, SMT offre une légère amélioration des performances pour la plupart des charges de travail en renforçant les calculs lorsque cela est possible, ce qui augmente le débit des instructions, bien qu’aucun gain de performances ou même une légère perte de performances ne se produise lors de la contention entre les threads pour les ressources du processeur partagé se produisent.
Les processeurs prenant en charge SMT sont disponibles à la fois pour Intel et AMD. Intel fait référence à leurs offres SMT en tant que technologie Intel Hyper-Threading ou Intel HT.

Dans le cadre de cet article, les descriptions de SMT et comment elles sont utilisées par Hyper-V s’appliquent également aux systèmes Intel et AMD.

* Pour plus d’informations sur la technologie Intel HT, reportez-vous à la [technologie Intel Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) .

* Pour plus d’informations sur AMD SMT, reportez-vous à [l’architecture de base « Zen »](https://www.amd.com/en/technologies/zen-core) .

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>Comprendre comment Hyper-V virtualise les processeurs

Avant de prendre en compte les types de planificateur d’hyperviseur, il est également utile de comprendre l’architecture Hyper-V. Vous trouverez une synthèse générale dans [vue d’ensemble de la technologie Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Il s’agit de concepts importants pour cet article :

* Hyper-V crée et gère des partitions d’ordinateur virtuel, sur lesquelles des ressources de calcul sont allouées et partagées, sous le contrôle de l’hyperviseur. Les partitions fournissent de fortes limites d’isolation entre toutes les machines virtuelles invitées et entre les machines virtuelles invitées et la partition racine.

* La partition racine est elle-même une partition d’ordinateur virtuel, bien qu’elle possède des propriétés uniques et des privilèges bien supérieurs à ceux des ordinateurs virtuels invités. La partition racine fournit les services de gestion qui contrôlent toutes les machines virtuelles invitées, fournit la prise en charge des appareils virtuels pour les invités et gère toutes les e/s de périphérique pour les machines virtuelles invitées. Microsoft recommande vivement de ne pas exécuter de charges de travail d’application dans la partition racine.

* Chaque processeur virtuel (VP) de la partition racine est mappé 1:1 à un processeur logique sous-jacent (LP). Un VP hôte s’exécutera toujours sur la même LP sous-jacente : il n’existe aucune migration de l’VPs de la partition racine.

* Par défaut, le niveau de VPs sur lequel s’exécute l’ordinateur hôte peut également exécuter VPs invité.

* Un vice-président peut être planifié par l’hyperviseur pour s’exécuter sur n’importe quel processeur logique disponible. Alors que le planificateur d’hyperviseur s’occupe de la localité du cache temporel, de la topologie NUMA et de nombreux autres facteurs lors de la planification d’un vice-président, le VP peut être planifié sur n’importe quel hôte LP.

## <a name="hypervisor-scheduler-types"></a>Types de planificateur d’hyperviseur

À compter de Windows Server 2016, l’hyperviseur Hyper-V prend en charge plusieurs modes de logique de planificateur, qui déterminent la façon dont l’hyperviseur planifie les processeurs virtuels sur les processeurs logiques sous-jacents. Ces types de planificateurs sont les suivants :

- [Le planificateur classique, à partage équitable](#the-classic-scheduler)
- [Planificateur principal](#the-core-scheduler)
- [Planificateur racine](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>Le planificateur classique

Le planificateur classique a été utilisé par défaut pour toutes les versions de l’hyperviseur Windows Hyper-V depuis son commencement, y compris Windows Server 2016 Hyper-V. Le planificateur classique offre un modèle de planification à part et préemption pour les processeurs virtuels invités.

Le type de planificateur classique est le plus approprié pour la grande majorité des utilisations d’Hyper-V traditionnelles (pour les clouds privés, les fournisseurs d’hébergement, etc.). Les caractéristiques de performances sont bien comprises et sont optimisées pour prendre en charge un large éventail de scénarios de virtualisation, tels que la surabonnement de VPs à la majorité des machines virtuelles et les charges de travail qui s’exécutent simultanément, à grande échelle. Machines virtuelles de performances, prenant en charge l’ensemble complet des fonctionnalités d’Hyper-V sans restrictions, et bien plus encore.

### <a name="the-core-scheduler"></a>Planificateur principal

Le planificateur du noyau de l’hyperviseur est une nouvelle alternative à la logique du planificateur classique, introduit dans Windows Server 2016 et Windows 10 version 1607. Le Planificateur principal offre une limite de sécurité renforcée pour l’isolation des charges de travail invitées et réduit la variabilité des performances pour les charges de travail à l’intérieur des machines virtuelles qui s’exécutent sur un hôte de virtualisation compatible SMT. Le Planificateur principal permet d’exécuter simultanément des machines virtuelles SMT et non SMT sur le même hôte de virtualisation compatible SMT.

Le Planificateur principal utilise la topologie SMT de l’hôte de virtualisation et expose éventuellement des paires SMT aux machines virtuelles invitées, et planifie des groupes de processeurs virtuels invités à partir de la même machine virtuelle sur des groupes de processeurs logiques SMT. Cette opération est effectuée de manière symétrique, de sorte que si la base de l’ensemble de ces deux groupes est de 2, les VPs sont planifiés dans des groupes de deux, et un cœur n’est jamais partagé entre les machines virtuelles.
Lorsque le VP est planifié pour un ordinateur virtuel sans SMT activé, ce vice-président consommera l’intégralité du noyau lors de son exécution.

Le résultat global du Planificateur principal est le suivant :

* Les VPs invités sont limités pour s’exécuter sur les paires de cœurs physiques sous-jacentes, ce qui permet d’isoler une machine virtuelle aux limites principales du processeur, réduisant ainsi les attaques de vulnérabilité à la surveillance des canaux secondaires à partir de machines virtuelles malveillantes.

* La variabilité du débit est considérablement réduite.

* Les performances sont potentiellement réduites, car si un seul groupe de VPs peut s’exécuter, un seul des flux d’instructions du noyau s’exécute alors que l’autre est resté inactif.

* Le système d’exploitation et les applications qui s’exécutent sur la machine virtuelle invitée peuvent utiliser le comportement SMT et les interfaces de programmation (API) pour contrôler et distribuer le travail entre les threads SMT, comme ils le feraient lors de l’exécution non virtualisée.

* Une limite de sécurité renforcée pour l’isolation de la charge de travail invité-les VPs invités sont limités pour s’exécuter sur les paires de cœurs physiques sous-jacentes, ce qui réduit les risques d’attaques d’espionnage des canaux secondaires.

Le Planificateur principal sera utilisé par défaut à partir de Windows Server 2019. Sur Windows Server 2016, le planificateur de base est facultatif et doit être explicitement activé par l’administrateur de l’hôte Hyper-V, et le planificateur classique est la valeur par défaut.

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>Comportement du Planificateur principal avec l’hôte SMT désactivé

Si l’hyperviseur est configuré pour utiliser le type de planificateur principal, mais que la fonctionnalité SMT est désactivée ou absente sur l’hôte de virtualisation, l’hyperviseur utilise le comportement de planificateur classique, quel que soit le paramètre de type de planificateur de l’hyperviseur.

### <a name="the-root-scheduler"></a>Planificateur racine

Le planificateur racine a été introduit avec Windows 10 version 1803. Lorsque le type de planificateur racine est activé, l’hyperviseur a remplacé le contrôle de la planification du travail par la partition racine. Le planificateur NT dans l’instance de système d’exploitation de la partition racine gère tous les aspects de la planification du travail sur le système de la segmentation.

Le planificateur racine traite des exigences uniques inhérentes à la prise en charge d’une partition de l’utilitaire pour fournir une isolation forte des charges de travail, telle qu’elle est utilisée avec Windows Defender application Guard (WDAG). Dans ce scénario, les responsabilités de planification sur le système d’exploitation racine offrent plusieurs avantages. Par exemple, les contrôles de ressources de processeur applicables aux scénarios de conteneur peuvent être utilisés avec la partition de l’utilitaire, ce qui simplifie la gestion et le déploiement. En outre, le planificateur du système d’exploitation racine peut facilement collecter des métriques sur l’utilisation du processeur de la charge de travail dans le conteneur et utiliser ces données comme entrée dans la même stratégie de planification applicable à toutes les autres charges de travail du système. Ces mêmes métriques permettent également d’attribuer clairement le travail effectué dans un conteneur d’application au système hôte. Le suivi de ces métriques est plus difficile avec les charges de travail des machines virtuelles traditionnelles, où un travail sur l’ensemble des machines virtuelles en cours d’exécution a lieu dans la partition racine.

#### <a name="root-scheduler-use-on-client-systems"></a>Utilisation du planificateur racine sur les systèmes clients

À compter de Windows 10 version 1803, le planificateur racine est utilisé par défaut sur les systèmes clients uniquement, où l’hyperviseur peut être activé pour la prise en charge de la sécurité basée sur la virtualisation et de l’isolation de la charge de travail WDAG, et pour le bon fonctionnement des futurs systèmes avec architectures de base hétérogènes. Il s’agit de la seule configuration du planificateur d’hyperviseur prise en charge pour les systèmes clients. Les administrateurs ne doivent pas tenter de remplacer le type de planificateur d’hyperviseur par défaut sur les systèmes clients Windows 10.

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>Contrôles de ressources du processeur de l’ordinateur virtuel et planificateur racine

Les contrôles de ressources du processeur de l’ordinateur virtuel fournis par Hyper-V ne sont pas pris en charge lorsque le planificateur racine de l’hyperviseur est activé, car la logique du planificateur du système d’exploitation racine gère les ressources des ordinateurs hôtes sur une base mondiale et n’a pas connaissance des machines virtuelles paramètres de configuration spécifiques. Les contrôles de ressources du processeur par machine virtuelle Hyper-V, tels que les Cap, les poids et les réserves, s’appliquent uniquement lorsque l’hyperviseur contrôle directement la planification du VP, par exemple avec les types de planificateur classiques et principaux.

#### <a name="root-scheduler-use-on-server-systems"></a>Utilisation du planificateur racine sur les systèmes serveurs

Il n’est pas recommandé d’utiliser le planificateur racine avec Hyper-V sur les serveurs à ce stade, car ses caractéristiques de performances n’ont pas encore été entièrement caractérisées et réglées pour s’adapter à la large gamme de charges de travail typiques de nombreux déploiements de virtualisation de serveur.

## <a name="enabling-smt-in-guest-virtual-machines"></a>Activation de SMT dans les machines virtuelles invitées

Une fois que l’hyperviseur de l’hôte de virtualisation est configuré pour utiliser le type de planificateur principal, les machines virtuelles invitées peuvent être configurées pour utiliser SMT Si vous le souhaitez. L’exposition du fait que les VPs sont hyperthreadées sur une machine virtuelle invitée permet au planificateur du système d’exploitation invité et aux charges de travail exécutées dans la machine virtuelle de détecter et d’utiliser la topologie SMT dans leur propre planification de travail. Sur Windows Server 2016, invité SMT n’est pas configuré par défaut et doit être explicitement activé par l’administrateur de l’hôte Hyper-V. À compter de Windows Server 2019, les nouvelles machines virtuelles créées sur l’hôte héritent par défaut de la topologie SMT de l’hôte.  Autrement dit, une machine virtuelle version 9,0 créée sur un ordinateur hôte avec 2 threads SMT par cœur contiendra également 2 threads SMT par cœur.

PowerShell doit être utilisé pour activer SMT sur une machine virtuelle invitée. aucune interface utilisateur n’est fournie dans le Gestionnaire Hyper-V.
Pour activer SMT dans une machine virtuelle invitée, ouvrez une fenêtre PowerShell avec des autorisations suffisantes, puis tapez :

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Où <n> est le nombre de threads SMT par cœur que la machine virtuelle invitée verra.  
Notez que <n> = 0 définit la valeur HwThreadCountPerCore pour qu’elle corresponde au nombre de threads SMT de l’hôte par valeur de base.

>[!NOTE] 
>Le paramètre HwThreadCountPerCore = 0 est pris en charge à partir de Windows Server 2019.

Voici un exemple d’informations système extraites du système d’exploitation invité qui s’exécute sur une machine virtuelle avec 2 processeurs virtuels et SMT activés. Le système d’exploitation invité détecte deux processeurs logiques appartenant au même noyau.

![Capture d’écran montrant Msinfo32 dans une machine virtuelle invitée avec SMT activé](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>Configuration du type de planificateur de l’hyperviseur sur Windows Server 2016 Hyper-V

Windows Server 2016 Hyper-V utilise le modèle de planificateur d’hyperviseur classique par défaut. L’hyperviseur peut éventuellement être configuré pour utiliser le Planificateur principal, afin d’accroître la sécurité en restreignant les VPs invités à s’exécuter sur les paires de SMT physiques correspondantes et pour prendre en charge l’utilisation de machines virtuelles avec la planification SMT pour leurs VPs invités.

>[!NOTE]
>Microsoft recommande que tous les clients qui exécutent Windows Server 2016 Hyper-V sélectionnent le planificateur de base pour s’assurer que leurs hôtes de virtualisation sont protégés de manière optimale contre les machines virtuelles invitées potentiellement malveillantes.

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Windows Server 2019 Hyper-V utilise par défaut le Planificateur principal

Pour garantir que les ordinateurs hôtes Hyper-V sont déployés dans la configuration de sécurité optimale, Windows Server 2019 Hyper-V utilisera désormais le modèle de planificateur de l’hyperviseur principal par défaut. L’administrateur de l’hôte peut éventuellement configurer l’hôte pour utiliser le planificateur classique hérité. Les administrateurs doivent lire, comprendre et prendre en compte l’impact de chaque type de planificateur sur la sécurité et les performances des hôtes de virtualisation avant de remplacer les paramètres par défaut du type de planificateur.  Pour plus d’informations, consultez fonctionnement de la [sélection du type de planificateur Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) .

### <a name="required-updates"></a>Mises à jour nécessaires

>[!NOTE]
>Les mises à jour suivantes sont requises pour utiliser les fonctionnalités du planificateur d’hyperviseur décrites dans ce document. Ces mises à jour incluent des modifications pour prendre en charge la nouvelle option « hypervisorschedulertype » BCD, qui est nécessaire pour la configuration de l’hôte.

| Version | Libérer  | Mise à jour requise | Article de la base de connaissances |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018,07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018,07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018,07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Aucun | Aucun |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>Sélection du type de planificateur de l’hyperviseur sur Windows Server

La configuration du planificateur de l’hyperviseur est contrôlée via l’entrée hypervisorschedulertype BCD.

Pour sélectionner un type de planificateur, ouvrez une invite de commandes avec des privilèges d’administrateur :

``` command
     bcdedit /set hypervisorschedulertype type
```

Où `type` est l’un des éléments suivants :

* Classique
* Standard
* Racine

Le système doit être redémarré pour que les modifications apportées au type de planificateur de l’hyperviseur prennent effet.

>[!NOTE]
>Le planificateur racine de l’hyperviseur n’est pas pris en charge sur Windows Server Hyper-V pour l’instant. Les administrateurs Hyper-V ne doivent pas tenter de configurer le planificateur racine pour une utilisation avec les scénarios de virtualisation de serveur.

## <a name="determining-the-current-scheduler-type"></a>Détermination du type de planificateur actuel

Vous pouvez déterminer le type de planificateur d’hyperviseur en cours d’utilisation en examinant le journal système dans observateur d’événements pour l’ID d’événement 2 de lancement de l’hyperviseur le plus récent, qui signale le type de planificateur d’hyperviseur configuré au lancement de l’hyperviseur. Les événements de lancement d’hyperviseur peuvent être obtenus à partir de Windows observateur d’événements ou via PowerShell.

L’ID d’événement 2 du lancement de l’hyperviseur indique le type de planificateur d’hyperviseur, où :

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Capture d’écran montrant les détails de l’ID d’événement 2 du lancement de l’hyperviseur](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Capture d’écran montrant observateur d’événements afficher l’ID d’événement de lancement de l’hyperviseur 2](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>Interrogation de l’événement de lancement du type de planificateur de l’hyperviseur Hyper-V à l’aide de PowerShell

Pour interroger l’ID d’événement 2 de l’hyperviseur à l’aide de PowerShell, entrez les commandes suivantes à partir d’une invite PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Capture d’écran montrant la requête PowerShell et les résultats de l’hyperviseur événement de lancement 2](media/Hyper-V-CoreScheduler-PowerShell.png)
