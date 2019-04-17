---
title: Présentation et utilisation des types de planificateur de l’hyperviseur Hyper-V
description: Fournit des informations pour les administrateurs d’hôtes Hyper-V sur l’utilisation du Planificateur de Hyper-V de modes
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: 7af6d68b02367d349580eacb27405c6f37e97ff8
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783691"
---
# Gestion des types de planificateur de l’hyperviseur Hyper-V

>S’applique à: Windows 10, Windows Server 2016, Windows Server, version 1709, Windows Server, version 1803, Windows Server 2019

Cet article décrit les nouveaux modes de processeur virtuel planification logique tout d’abord introduite dans Windows Server 2016. Ces modes ou des types de planificateur, déterminent comment l’hyperviseur Hyper-V alloue et gère le travail sur les processeurs virtuels invités. Un administrateur de l’hôte Hyper-V permet de types de planificateur hyperviseur idéale pour les machines virtuelles invité (VM) et configurer les ordinateurs virtuels pour tirer parti de la logique de planification.

>[!NOTE]
>Mises à jour sont tenus d’utiliser les fonctionnalités de planificateur hyperviseur décrites dans ce document. Pour plus d’informations, voir [mises à jour requises](#required-updates).

## Arrière-plan

Avant d’aborder la logique et les contrôles derrière processeur virtuel Hyper-V planification, il est utile passer en revue les concepts de base traitées dans cet article.

### Compréhension SMT

Le multithreading simultané ou SMT, est une technique employée dans les conceptions de processeur moderne qui permet aux ressources du processeur être partagé par les threads distincts, indépendamment de l’exécution. En règle générale, SMT offre une amélioration des performances modeste pour la plupart des charges de travail par parallélisation des calculs dans la mesure du possible, augmenter le débit de l’instruction, même si aucun performances conquérir ou même une légère perte de performances peuvent se produire lorsque contention entre les threads pour ressources de processeur partagés se produit.
Processeurs prenant en charge SMT sont disponibles à partir d’Intel et AMD. Intel fait référence à leurs offres SMT en tant que technologie Intel Hyper-Threading ou HT Intel.

Dans le cadre de cet article, les descriptions de SMT et la façon dont il est utilisé par Hyper-V s’appliquent également aux systèmes Intel et AMD.

* Pour plus d’informations sur la technologie Intel HT, reportez-vous à [La technologie Intel Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* Pour plus d’informations sur AMD SMT, reportez-vous à [L’Architecture de «Simplicité»](https://www.amd.com/en/technologies/zen-core)

## Comprendre comment Hyper-V virtualise processeurs

Avant d’aborder hyperviseur types planificateur, il est également utile de comprendre l’architecture Hyper-V. Vous trouverez une synthèse générale de [Présentation de la technologie Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Voici quelques concepts importants pour cet article:

* Hyper-V crée et gère les partitions de machine virtuelle, sur le calcul des ressources sont allouées et partagés, sous le contrôle de l’hyperviseur. Les partitions offrent des limites d’isolement forte entre l’ensemble des machines virtuelles invitées et entre les machines virtuelles invitées et la partition racine.

* La partition racine est lui-même une partition de la machine virtuelle, bien qu’il a des propriétés et bien supérieures privilèges que les machines virtuelles invitées. La partition racine fournit les services de gestion qui contrôlent l’ensemble des machines virtuelles invitées, prend en charge les périphériques virtuels invités et gère tous les périphériques d’e/s pour les machines virtuelles invitées. Microsoft recommande vivement exécute ne pas les charges de travail d’application dans la partition racine.

* Chaque processeur virtuel (VC) de la partition racine est mappé 1:1 d’un processeur logique sous-jacent (site). Un hôte vice-président s’exécute toujours sur le même LP sous-jacent: il n’existe pas de migration de PV la partition racine.

* Par défaut, les pl sur lequel exécute PV hôte peuvent également exécuter PV invité.

* Un vice-président invité peut être planifié par l’hyperviseur à s’exécuter sur tous les processeurs logiques disponibles. Alors que le Planificateur de l’hyperviseur s’occupe de prendre en compte localité du cache temporelle, la topologie NUMA et nombreux autres facteurs lors de la planification d’un vice-président invité, en fin de compte le vice-président peut être planifié sur n’importe quel hôte LP.

## Types de planificateur de l’hyperviseur

À compter de Windows Server 2016, l’hyperviseur Hyper-V prend en charge plusieurs modes de logique planificateur, qui déterminent la façon dont l’hyperviseur planifie les processeurs virtuels sur les processeurs logiques sous-jacent. Ces types de planificateur sont:

- [Le planificateur classique, équitable](#the-classic-scheduler)
- [Le Planificateur de core](#the-core-scheduler)
- [Le Planificateur de racine](#the-root-scheduler)

### Le planificateur classique

Le planificateur classique a été la valeur par défaut pour toutes les versions de l’hyperviseur Hyper-V de Windows depuis son introduction, y compris Windows Server 2016 Hyper-V. Le planificateur classique offre une partie équitable, préemptive modèle de planification de tourniquet DNS pour les processeurs virtuels invités.

Le type de planificateur classique est la plus appropriée pour la grande majorité des utilisations de Hyper-V traditionnelles: aux clouds privés, fournisseurs d’hébergement et ainsi de suite. Les caractéristiques de performances encourus et sont mieux optimisés pour prendre en charge un large éventail de scénarios de virtualisation, telles que de surallocation de PV à des pl, exécution simultanée de nombreuses machines virtuelles et des charges de travail hétérogènes, en cours d’exécution plus grande échelle élevée performances de machines virtuelles, prise en charge de la fonctionnalité complète l’ensemble de Hyper-V sans restriction et bien plus encore.

### Le Planificateur de core

Le Planificateur de core hyperviseur est une nouvelle alternative à la logique de planificateur classique, introduite dans Windows Server 2016 et Windows 10 version 1607. Le Planificateur de core offre une limite de sécurité renforcée pour l’isolation de charge de travail invité et variabilité de baisse des performances pour les charges de travail à l’intérieur de machines virtuelles qui sont exécutent sur un hôte de virtualisation compatible SMT. Le Planificateur de core permet en cours d’exécution SMT et non-SMT machines virtuelles simultanément sur le même hôte de virtualisation compatible SMT.

Le Planificateur de core utilise la topologie SMT de l’hôte de virtualisation et si vous le souhaitez expose les paires SMT aux machines virtuelles invitées et aux groupes de planifications de processeurs virtuels invités à partir de la même machine virtuelle sur des groupes de processeurs logiques SMT. Cette opération est effectuée symétriques afin que si PL se trouvent dans des groupes de deux, PV sont planifiés dans des groupes de deux, et un cœur n’est jamais partagé entre les ordinateurs virtuels.
Si la VC est planifié pour une machine virtuelle sans SMT est activé, que vice-président utilisera les principales lorsqu’elle s’exécute.

Le résultat global du planificateur core est que:

* PV invité sont limités à s’exécuter sur sous-jacente des paires de cœur physique, isoler un ordinateur virtuel aux limites de cœur de processeur, ce qui réduit la vulnérabilité aux attaques snooping canal à partir d’ordinateurs virtuels malveillants.

* Variabilité de débit est considérablement réduite.

* Performances sont potentiellement réduites, car si seul un groupe de PV peut s’exécuter, uniquement un des flux instruction dans le noyau s’exécute pendant que l’autre est devenue inactive.

* Le système d’exploitation et les applications en cours d’exécution sur l’ordinateur virtuel invité peuvent utiliser le comportement SMT et (API) pour contrôler et distribuer le travail sur les threads MTS, tout comme ils exécuterait lorsque non virtualisés interfaces de programmation.

* Une limite de sécurité renforcée pour l’isolation de charge de travail invité - invité PV sont limitées à s’exécuter sur des paires de cœur physique sous-jacent, réduire la vulnérabilité aux attaques snooping de canal.

Le Planificateur de core servira par défaut à partir de Windows Server 2019. Sur Windows Server 2016, le planificateur core est facultatif et doit être activé explicitement par l’administrateur de l’hôte Hyper-V et le planificateur classique est la valeur par défaut.

#### Comportement de planificateur de Core avec hôte SMT désactivé

Si l’hyperviseur est configuré pour utiliser le type de planificateur core, mais la fonctionnalité SMT est désactivé ou non présents sur l’hôte de virtualisation, l’hyperviseur utilise le comportement classique planificateur, quel que soit le paramètre de type de planificateur de l’hyperviseur.

### Le Planificateur de racine

Le Planificateur de racine a été introduit avec Windows 10 version 1803. Lorsque le type de planificateur racine est activé, l’hyperviseur cedes contrôle de planification de travail à la partition racine. Le Planificateur de NT dans une instance de système d’exploitation de la partition racine gère tous les aspects de la planification de travail au système pl.

Le Planificateur de racine répond aux critères spécifiques inhérents avec prise en charge une partition utilitaire pour assurer l’isolation de la charge de travail forte, tel qu’utilisé avec Windows Defender Application Guard (WDAG). Dans ce scénario, en laissant planifier responsabilités à la racine du système d’exploitation offre plusieurs avantages. Par exemple, les contrôles de ressources de processeur applicables aux scénarios de conteneur est utilisable avec la partition d’utilitaires, simplifier la gestion et le déploiement. En outre, le Planificateur de système d’exploitation racine peut facilement collecter des mesures sur la charge de travail de l’UC à l’intérieur du conteneur et utiliser ces données comme entrée pour la même stratégie planification applicable à tous les autres charges de travail dans le système. Ces mêmes mesures également contribuer à clairement attribut travail effectuées dans un conteneur d’application au système hôte. Ces mesures de suivi est plus difficile avec les charges de travail traditionnels de machines virtuelles, où des tâches pour le compte tous les en cours d’exécution de la machine virtuelle a lieu dans la partition racine.

#### Utilisation de planificateur de racine sur les systèmes clients

À partir de Windows 10 version 1803, le Planificateur de racine est utilisé par défaut sur les systèmes client uniquement, où l’hyperviseur peut être activée à l’appui de sécurité basée sur la virtualisation et l’isolation de charge de travail WDAG et au bon fonctionnement de futurs systèmes avec architectures core hétérogènes. Il s’agit de la configuration de planificateur hyperviseur pris en charge uniquement pour les systèmes clients. Les administrateurs ne devraient pas essayer de remplacer le type de planificateur de l’hyperviseur par défaut sur les systèmes clients Windows 10.

#### Contrôles de ressources de processeur de l’ordinateur virtuel et le Planificateur de racine

Les contrôles de ressources de processeur de machine virtuelle fournies par Hyper-V ne sont pas pris en charge lorsque le Planificateur de racine de l’hyperviseur est activé, comme une logique planificateur du système d’exploitation racine est la gestion des ressources de l’hôte sur une base globale et n’a pas connaissance d’une machine virtuelle paramètres de configuration spécifiques. Les contrôles de ressources de processeur à par machines virtuelles de Hyper-V, comme VERR, poids et réserves, sont appliquent uniquement lorsque l’hyperviseur contrôle directement vice-président planification, comme avec les types de planificateur classique et standard.

#### Utilisation de planificateur de racine sur les systèmes de serveur

Le Planificateur de racine n’est pas recommandé pour une utilisation avec Hyper-V sur des serveurs à ce stade, comme ses caractéristiques de performances n’ont pas encore été entièrement caractérisés et ajustées pour prendre en charge la grande variété de charges de travail classiques de nombreux déploiements de la virtualisation de serveur.

## L’activation de SMT sur des machines virtuelles invité

Une fois que l’hyperviseur de l’hôte de virtualisation est configuré pour utiliser le type de planificateur core, les machines virtuelles invitées peuvent être configurés pour utiliser SMT si vous le souhaitez. Exposition au fait que PV multicœur à une machine virtuelle invitée permet le planificateur dans le système d’exploitation invité et les charges de travail en cours d’exécution sur la machine virtuelle à détecter et à utiliser la topologie de SMT dans leur propre planification de travail. Sur Windows Server 2016, invité SMT n’est pas configurée par défaut et doit être activée explicitement par l’administrateur de l’hôte Hyper-V. À compter de Windows Server 2019, nouveaux ordinateurs virtuels créés sur l’ordinateur hôte héritera de topologie SMT de l’hôte par défaut.  Autrement dit, une version que VM 9.0 créés sur un hôte avec des threads MTS 2 par cœur pourrait voir également 2 threads MTS par cœur.

PowerShell doit être utilisée pour activer SMT dans une machine virtuelle invitée; Il n’existe aucune interface utilisateur fournie dans le Gestionnaire Hyper-V.
Pour activer SMT dans une machine virtuelle invitée, ouvrez une fenêtre PowerShell avec des autorisations suffisantes et le type:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Où <n> est le nombre de threads MTS par cœur de l’invité d’ordinateur virtuel s’affiche.  
Notez que <n> = 0 définit la valeur HwThreadCountPerCore pour correspondre au nombre de thread de l’hôte SMT par valeur fondamentale.

>[!NOTE] 
>Paramètre HwThreadCountPerCore = 0 est pris en charge à compter de Windows Server 2019.

Voici un exemple d’informations système provenant du système d’exploitation invité en cours d’exécution dans une machine virtuelle avec 2 processeurs virtuels et SMT activée. Le système d’exploitation invité détecte 2 processeurs logiques appartenant à la même cœur.

![Capture d’écran qui s’affiche msinfo32 dans une machine virtuelle avec SMT activé invité](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## Configuration du type de planificateur hyperviseur sur Windows Server 2016 Hyper-V

Windows Server 2016 Hyper-V utilise le modèle de planificateur hyperviseur classique par défaut. L’hyperviseur peut éventuellement être configuré pour utiliser le planificateur core, pour renforcer la sécurité en limitant PV invité à s’exécuter sur des paires SMT physiques correspondantes et prendre en charge l’utilisation des machines virtuelles avec SMT planification pour leur PV invité.

>[!NOTE]
>Microsoft recommande que tous les clients exécutant Windows Server 2016 Hyper-V sélectionner le Planificateur de core pour vous assurer que leurs ordinateurs hôtes de virtualisation sont optimale protégés contre les machines virtuelles invitées potentiellement malveillants.

## Valeurs par défaut de Windows Server 2019 Hyper-V à l’aide du Planificateur de core

Pour garantir la hôtes Hyper-V sont déployés dans les personnels une sécurité optimale, Windows Server 2019 Hyper-V utilise maintenant le modèle de planificateur de l’hyperviseur core par défaut. L’administrateur de l’hôte peut éventuellement configurer l’hôte afin d’utiliser le planificateur classique hérité. Les administrateurs doivent Lisez attentivement, comprendre et prendre en compte l’impact de que chaque type de planificateur présente sur la sécurité et les performances des hôtes de virtualisation avant de remplacer les paramètres par défaut du type de maintenance.  Pour plus d’informations, consultez la [sélection du type de planificateur présentation d’Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) .

### Mises à jour requises

>[!NOTE]
>Les mises à jour suivantes sont tenus d’utiliser les fonctionnalités de planificateur hyperviseur décrites dans ce document. Ces mises à jour incluent des modifications pour prendre en charge de la nouvelle option BCD «hypervisorschedulertype», ce qui est nécessaire pour la configuration de l’hôte.

| Version | Release  | Mise à jour requise | Article |
|--------------------|------|---------|-------------:|
|WindowsServer2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|WindowsServer2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|WindowsServer2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Aucun(e) | Aucun(e) |

## Sélection du type de planificateur hyperviseur sur Windows Server

La configuration du Planificateur de l’hyperviseur est contrôlée par l’entrée BCD hypervisorschedulertype.

Pour sélectionner un type de planificateur, ouvrez une invite de commandes avec des privilèges d’administrateur:

``` command
     bcdedit /set hypervisorschedulertype type
```

Où `type` est l’un des:

* Classique
* Standard

Le système doit être redémarré pour que les modifications dans le type de planificateur hyperviseur prennent effet.

>[!NOTE]
>Le Planificateur de racine de l’hyperviseur n'est pas pris en charge sur Windows Server Hyper-V pour l’instant. Les administrateurs Hyper-V ne devraient pas essayer de configurer le Planificateur de racine pour une utilisation avec des scénarios de virtualisation de serveur.

## Détermination du type de planificateur actuel

Vous pouvez déterminer le type de planificateur hyperviseur actuel en cours d’utilisation en examinant le journal système dans l’Observateur d’événements pour l’événement de lancement de l’hyperviseur ID 2, la plus récente qui signale le type de planificateur hyperviseur configuré au lancement de l’hyperviseur. Événements de lancement de l’hyperviseur peuvent être obtenus à partir de l’Observateur d’événements Windows, ou via PowerShell.

Événement de lancement de l’hyperviseur ID 2 indique le type de planificateur hyperviseur, où:

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Capture d’écran montrant les informations de 2 ID d’événement du lancement de l’hyperviseur](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Capture d’écran montrant l’Observateur d’événements affichant l’événement de lancement de l’hyperviseur ID 2](media/Hyper-V-CoreScheduler-EventViewer.png)

### Interrogation de l’événement de lancement de type de planificateur Hyper-V hyperviseur à l’aide de PowerShell

Pour rechercher l’hyperviseur événement ID 2 à l’aide de PowerShell, entrez les commandes suivantes à partir d’une invite de commandes PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Capture d’écran montrant la requête de PowerShell et les résultats de l’événement de lancement de l’hyperviseur ID 2](media/Hyper-V-CoreScheduler-PowerShell.png)
