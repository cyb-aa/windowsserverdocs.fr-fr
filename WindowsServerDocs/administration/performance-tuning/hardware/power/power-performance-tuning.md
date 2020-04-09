---
title: Réglage de l’alimentation et des performances
description: Réglage de la gestion de l’alimentation du processeur (PPM) pour le mode de gestion de l’alimentation équilibré de Windows Server
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: qizha;tristanb
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 1457328a151c87d2d4cb41c4ee91b4759f4fb8e2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851992"
---
# <a name="power-and-performance-tuning"></a>Réglage de l’alimentation et des performances

L’efficacité énergétique est de plus en plus importante dans les environnements d’entreprise et de centre de données, et elle ajoute un autre ensemble de compromis à la combinaison d’options de configuration.

Windows Server 2016 est optimisé pour un excellent rendement énergétique avec un impact minimal sur les performances sur un large éventail de charges de travail client. [Réglage de la gestion de l’alimentation du processeur (ppm) pour le mode d’alimentation équilibrée de Windows Server](processor-power-management-tuning.md) décrit les charges de travail utilisées pour paramétrer les paramètres par défaut dans windows server 2016 et fournit des suggestions pour les réglages personnalisés.

Cette section développe les compromis d’efficacité énergétique pour vous aider à prendre des décisions avisées si vous devez ajuster les paramètres d’alimentation par défaut sur votre serveur. Toutefois, la majeure partie du matériel et des charges de travail du serveur ne doit pas nécessiter un réglage de l’alimentation de l’administrateur lors de l’exécution de Windows Server 2016.

## <a name="calculating-server-energy-efficiency"></a>Calcul de l’efficacité énergétique du serveur

Lorsque vous réglez votre serveur pour les économies d’énergie, vous devez également prendre en compte les performances. Le paramétrage affecte les performances et la puissance, parfois dans des montants disproportionnés. Pour chaque ajustement possible, prenez en compte vos objectifs de performances et de budget d’alimentation pour déterminer si le compromis est acceptable.

Vous pouvez calculer le taux de rendement énergétique de votre serveur pour une métrique utile qui intègre des informations sur l’alimentation et les performances. L’efficacité énergétique est le rapport entre le travail effectué et la puissance moyenne requise pendant un laps de temps spécifié.

![formule d’efficacité énergétique](../../media/perftune-guide-power-formula.png)

Vous pouvez utiliser cette mesure pour définir des objectifs pratiques qui respectent le compromis entre la puissance et les performances. En revanche, un objectif de 10% d’économies d’énergie dans le centre de données ne parvient pas à capturer les effets correspondants sur les performances, et vice versa.

De même, si vous réglez votre serveur afin d’augmenter les performances de 5% et que cela aboutit à une consommation d’énergie de 10% supérieure, le résultat total peut être acceptable ou non pour vos objectifs d’entreprise. La mesure de l’efficacité énergétique permet une prise de décision plus éclairée que les mesures d’alimentation ou de performances seules.

## <a name="measuring-system-energy-consumption"></a>Mesure de la consommation d’énergie du système

Vous devez établir une mesure de la puissance de référence avant de régler votre serveur à des fins d’efficacité énergétique.

Si votre serveur dispose de la prise en charge nécessaire, vous pouvez utiliser les fonctionnalités de contrôle de l’alimentation et de budgétisation de Windows Server 2016 pour afficher la consommation d’énergie au niveau du système à l’aide de l’analyseur de performances.

Une façon de déterminer si votre serveur prend en charge le contrôle et la budgétisation consiste à consulter le [Catalogue Windows Server](https://www.windowsservercatalog.com). Si votre modèle de serveur qualifie la nouvelle qualification de gestion de l’alimentation améliorée dans le programme de certification matérielle de Windows, il est garanti qu’il prend en charge les fonctionnalités de contrôle et de budgétisation.

Une autre façon de vérifier la prise en charge du contrôle consiste à rechercher manuellement les compteurs dans l’analyseur de performances. Ouvrez l’analyseur de performances, sélectionnez **Ajouter des compteurs**, puis recherchez le groupe de compteurs de compteur **d’alimentation** .

Si les instances nommées des compteurs d’alimentation s’affichent dans la zone **instances de l’objet sélectionné**, votre plateforme prend en charge le contrôle. Le compteur **d’alimentation** qui affiche puissance en watts apparaît dans le groupe de compteurs sélectionné. La dérivation exacte de la valeur des données d’alimentation n’est pas spécifiée. Par exemple, il peut s’agir d’une puissance de dessin instantanée ou d’une puissance moyenne sur un intervalle de temps.

Si la plate-forme de votre serveur ne prend pas en charge le contrôle, vous pouvez utiliser un appareil de contrôle physique connecté à l’entrée d’alimentation pour mesurer la consommation d’énergie ou le système.

Pour établir une ligne de base, vous devez mesurer la puissance moyenne requise sur divers points de charge système, de l’inactivité à 100% (débit maximal) pour générer une ligne de charge. L’illustration suivante montre des lignes de charge pour trois exemples de configurations :

![exemples de lignes de charge](../../media/perftune-guide-sample-loadlines.png)

Vous pouvez utiliser les lignes de charge pour évaluer et comparer les performances et la consommation d’énergie des configurations sur tous les points de charge. Dans cet exemple particulier, il est facile de voir la meilleure configuration. Toutefois, il peut y avoir facilement des scénarios dans lesquels une configuration fonctionne mieux pour les charges de travail lourdes et l’autre fonctionne mieux pour les charges de travail légères.

Vous devez parfaitement comprendre les exigences de votre charge de travail pour choisir une configuration optimale. Ne partez pas du principe que lorsque vous trouvez une bonne configuration, elle restera toujours optimale. Vous devez mesurer régulièrement l’utilisation du système et la consommation d’énergie, et après avoir modifié les charges de travail, les niveaux de charge de travail ou le matériel serveur.

## <a name="diagnosing-energy-efficiency-issues"></a>Diagnostic des problèmes d’efficacité énergétique

**Powercfg. exe** prend en charge une option de ligne de commande que vous pouvez utiliser pour analyser l’efficacité énergétique inactive de votre serveur. Lorsque vous exécutez PowerCfg. exe avec l’option **/Energy** , l’outil effectue un test de 60 seconde pour détecter les problèmes potentiels d’efficacité énergétique. L’outil génère un rapport HTML simple dans le répertoire actif.

> [!Important]
> Pour garantir une analyse précise, assurez-vous que toutes les applications locales sont fermées avant d’exécuter **powercfg. exe**. 

Les vitesses de cycle de minuterie raccourcies, les pilotes qui ne prennent pas en charge la gestion de l’alimentation et l’utilisation excessive du processeur sont quelques-uns des problèmes de comportement qui sont détectés par la commande **powercfg/Energy** . Cet outil offre un moyen simple d’identifier et de résoudre les problèmes de gestion de l’alimentation, ce qui peut entraîner des économies considérables dans un grand centre de précis.

Pour plus d’informations sur PowerCfg. exe, consultez [utilisation de powercfg pour évaluer l’efficacité énergétique du système](https://msdn.microsoft.com/windows/hardware/gg463250.aspx).

## <a name="using-power-plans-in-windows-server"></a>Utilisation des modes de gestion de l’alimentation dans Windows Server

Windows Server 2016 possède trois modes de gestion de l’alimentation intégrés conçus pour répondre à différents besoins professionnels. Ces plans offrent un moyen simple de personnaliser un serveur pour répondre aux objectifs d’alimentation ou de performances. Le tableau suivant décrit les plans, répertorie les scénarios courants d’utilisation de chaque plan et fournit des détails d’implémentation pour chaque plan.

| **Plan** | **Description** | **Scénarios courants applicables** | **Principales mises en œuvre** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Équilibré (recommandé) | Paramètre par défaut. Cible une bonne efficacité énergétique avec un impact minime sur les performances. | Calcul général | Correspond à la capacité à la demande. Les fonctionnalités d’économie d’énergie équilibrent la puissance et les performances. |
| Hautes performances | Augmente les performances au détriment de la consommation d’énergie élevée. Les limitations d’alimentation et thermique, les dépenses d’exploitation et les considérations de fiabilité s’appliquent. | Applications à faible latence et code d’application sensible aux modifications des performances du processeur | Les processeurs sont toujours verrouillés à l’état de performances le plus élevé (y compris les fréquences « Turbo »). Tous les cœurs sont désimmobilisés. La sortie thermique peut être importante. |
| Économiseur d’énergie | Limite les performances pour économiser de l’énergie et réduire les coûts d’exploitation. Non recommandé sans test minutieux pour s’assurer que les performances sont adéquates. | Déploiements avec des budgets d’alimentation et des contraintes thermiques limités | Fréquence du processeur en majuscules à un pourcentage maximal (si pris en charge) et permet d’autres fonctionnalités d’économie d’énergie. |


Ces modes de gestion de l’alimentation existent dans Windows pour les systèmes d’alimentation en courant alternatif et en mode courant (DC) directs, mais nous supposerons que les serveurs utilisent toujours une source d’alimentation secteur.

Pour plus d’informations sur les modes de gestion de l’alimentation et les configurations de stratégie d’alimentation, consultez [configuration et déploiement de stratégies d’alimentation dans Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

> [!Note]
> Certains fabricants de serveurs disposent de leurs propres options de gestion de l’alimentation disponibles via les paramètres du BIOS. Si le système d’exploitation n’a pas le contrôle sur la gestion de l’alimentation, la modification des modes de gestion de l’alimentation dans Windows n’affecte pas la puissance et les performances du système.

## <a name="tuning-processor-power-management-parameters"></a>Paramétrage des paramètres de gestion de l’alimentation du processeur

Chaque mode de gestion de l’alimentation représente une combinaison de nombreux paramètres de gestion de l’alimentation sous-jacents. Les plans intégrés sont trois collections de paramètres recommandés qui couvrent un large éventail de charges de travail et de scénarios. Toutefois, nous reconnaissons que ces plans ne répondent pas aux besoins de chaque client.

Les sections suivantes décrivent les différentes façons d’ajuster certains paramètres de gestion de l’alimentation du processeur pour atteindre des objectifs non traités par les trois plans intégrés. Si vous avez besoin de comprendre un ensemble plus vaste de paramètres d’alimentation, consultez [configuration et déploiement de stratégies d’alimentation dans Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

## <a name="processor-performance-boost-mode"></a>Mode d’amélioration des performances du processeur

Les technologies Intel Turbo Boost et AMD Turbo CORE sont des fonctionnalités qui permettent aux processeurs d’obtenir des performances supplémentaires quand elles sont les plus utiles (c’est-à-dire à des charges système élevées). Toutefois, cette fonctionnalité augmente la consommation d’énergie des cœurs de processeur, de sorte que Windows Server 2016 configure les technologies Turbo en fonction de la stratégie d’alimentation utilisée et de l’implémentation de processeur spécifique.

Turbo est activé pour les modes de gestion de l’alimentation hautes performances sur tous les processeurs Intel et AMD, et il est désactivé pour les modes d’alimentation Power Saver. Pour les modes de gestion de l’alimentation équilibrés sur les systèmes qui reposent sur la gestion de fréquence basée sur l’État P traditionnelle, Turbo est activé par défaut uniquement si la plateforme prend en charge le registre EPB.

> [!Note]
> Le registre EPB est uniquement pris en charge dans les processeurs Intel Westmere et versions ultérieures.

Pour les processeurs Intel Nehalem et AMD, Turbo est désactivé par défaut sur les plateformes basées sur l’État P. Toutefois, si un système prend en charge le contrôle de performance du processeur collaboratif (CPPC), qui est un nouveau mode de communication des performances entre le système d’exploitation et le matériel (défini dans ACPI 5,0), Turbo peut être engagé si le système d’exploitation Windows demande le matériel pour fournir les niveaux de performances les plus élevés possibles.

Pour activer ou désactiver la fonctionnalité Turbo Boost, le paramètre du mode de renforcement des performances du processeur doit être configuré par l’administrateur ou par les paramètres de paramètre par défaut pour le mode de gestion de l’alimentation choisi. Le mode d’amélioration des performances du processeur a cinq valeurs autorisées, comme indiqué dans le tableau 5.

Pour le contrôle basé sur l’État P, les choix sont désactivés, activés (Turbo est disponible pour le matériel à chaque demande de performances nominales) et efficace (Turbo est disponible uniquement si le registre EPB est implémenté).

Dans le cas d’un contrôle basé sur CPPC, les choix sont désactivés, l’efficacité est activée (Windows spécifie la quantité exacte de Turbo à fournir) et agressive (Windows demande une « performance maximale » pour activer Turbo).

Dans Windows Server 2016, la valeur par défaut du mode Boost est 3.

| **Nom** | **Comportement basé sur l’État P** | **Comportement de CPPC** |
|--------------------------|------------------------|-------------------|
| 0 (désactivé) | Désactivé | Désactivé |
| 1 (activé) | Activé | Activation efficace |
| 2 (agressif) | Activé | Aggressive |
| 3 (activé efficacement) | Efficace | Activation efficace |
| 4 (efficacité agressive) | Efficace | Aggressive |

 
Les commandes suivantes activent le mode d’amélioration des performances du processeur sur le mode de gestion de l’alimentation actuel (spécifiez la stratégie à l’aide d’un alias GUID) :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> Vous devez exécuter la commande **powercfg-SetActive** pour activer les nouveaux paramètres. Vous n’avez pas besoin de redémarrer le serveur.

Pour définir cette valeur pour les modes de gestion de l’alimentation autres que le plan actuellement sélectionné, vous pouvez utiliser des alias tels que le schéma\_MAX (économies d’énergie), le schéma\_MIN (High performance) et le schéma\_équilibré (équilibré) à la place du schéma\_actuel. Remplacez « Scheme Current » dans les commandes powercfg-SetActive précédemment affichées par l’alias souhaité pour activer ce mode de gestion de l’alimentation.

Par exemple, pour ajuster le mode d’amélioration dans le plan de l’économie d’énergie et faire en sorte que l’économiseur d’énergie soit le plan actuel, exécutez les commandes suivantes :

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>État des performances minimales et maximales du processeur

Les processeurs changent très rapidement entre les États de performance (P-States) pour faire correspondre l’approvisionnement à la demande, en fournissant des performances si nécessaire et en économisant de l’énergie lorsque cela est possible. Si votre serveur a des exigences spécifiques en matière de haute performance ou de consommation d’énergie minimale, vous pouvez envisager de configurer le paramètre d' **État de performance minimum du processeur** ou le paramètre d' **État de performance maximal du processeur** .

Les valeurs des paramètres d’état de performance **minimum du processeur** et de l' **État de performance maximal du processeur** sont exprimées sous la forme d’un pourcentage de la fréquence maximale du processeur, avec une valeur comprise entre 0 et 100.

Si votre serveur requiert une latence très faible, une fréquence d’UC invariante (par exemple, pour des tests reproductibles) ou des niveaux de performances plus élevés, vous pouvez ne pas souhaiter que les processeurs basculent vers des États de performances inférieurs. Pour un serveur de ce type, vous pouvez limiter l’État minimal des performances du processeur à 100% à l’aide des commandes suivantes :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

Si votre serveur requiert une consommation d’énergie inférieure, vous souhaiterez peut-être limiter l’état des performances du processeur à un pourcentage maximal. Par exemple, vous pouvez limiter le processeur à 75% de sa fréquence maximale à l’aide des commandes suivantes :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> La limitation des performances du processeur à un pourcentage maximal nécessite une prise en charge du processeur. Consultez la documentation du processeur pour déterminer si une telle prise en charge existe ou affichez le compteur de l’analyseur **de performances% de la fréquence maximale** dans le groupe de **processeurs** pour déterminer si des seuils de fréquence ont été appliqués.

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>Augmentation des performances du processeur et diminution des seuils et des stratégies

La vitesse à laquelle un état des performances du processeur augmente ou diminue est contrôlée par plusieurs paramètres. Les quatre paramètres suivants présentent l’impact le plus visible :

-   Le **seuil d’augmentation des performances du processeur** définit la valeur d’utilisation au-delà de laquelle l’état des performances d’un processeur augmente. Des valeurs plus élevées ralentissent le taux d’augmentation de l’état des performances en réponse à l’augmentation des activités.

-   Le **seuil de baisse des performances du processeur** définit la valeur d’utilisation en dessous de laquelle l’état des performances d’un processeur diminue. Des valeurs plus élevées augmentent le taux de baisse de l’état des performances pendant les périodes d’inactivité.

-   **Stratégie d’augmentation des performances du processeur et baisse des performances du processeur** La stratégie détermine l’état des performances qui doit être défini lorsqu’une modification se produit. La stratégie « unique » signifie qu’elle choisit l’état suivant. « Rocket » correspond à l’état de performances d’alimentation maximal ou minimal. « Idéal » tente de trouver un équilibre entre la puissance et les performances.

Par exemple, si votre serveur nécessite une latence très faible tout en profitant de la faible puissance pendant les périodes d’inactivité, vous pouvez Quicken l’augmentation de l’état des performances pour toute augmentation de la charge et ralentir la baisse lorsque la charge est interrompue. Les commandes suivantes définissent la stratégie d’augmentation sur « Rocket » pour accélérer l’augmentation de l’État et attribuent à la stratégie de réduction la valeur « Single ». Les seuils d’augmentation et de réduction sont respectivement définis sur 10 et 8.

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>Nombre maximal et minimum de cœurs des performances du processeur

Le parking central est une fonctionnalité qui a été introduite dans Windows Server 2008 R2. Le moteur de gestion de l’alimentation du processeur (PPM) et le planificateur fonctionnent ensemble pour ajuster dynamiquement le nombre de cœurs disponibles pour exécuter les threads. Le moteur PPM choisit un nombre minimal de cœurs pour les threads qui seront planifiés.

En règle générale, aucun thread n’est planifié pour les cœurs qui sont en stationnement, et ceux-ci sont débranchés dans des États d’alimentation très faibles lorsqu’ils ne traitent pas les interruptions, les appels DPC ou tout autre travail strictement affinité. Les cœurs restants sont responsables du reste de la charge de travail. Le parking de base peut potentiellement accroître l’efficacité énergétique pendant une utilisation inférieure.

Pour la plupart des serveurs, le comportement de parking central par défaut fournit un équilibre raisonnable entre le débit et l’efficacité énergétique. Sur les processeurs où le parking central ne peut pas afficher autant d’avantages sur les charges de travail génériques, il peut être désactivé par défaut.

Si votre serveur a des exigences spécifiques en matière de parking, vous pouvez contrôler le nombre de cœurs disponibles pour le stationnement à l’aide du paramètre **Core performance Core maximum cœurs** ou du paramètre Core **performance Core Park minimum cores** dans Windows Server 2016.

Un scénario dans lequel le parking central n’est pas toujours optimal est lorsqu’il y a un ou plusieurs threads actifs affinité à un sous-ensemble non négligeable d’UC dans un nœud NUMA (autrement dit, plus d’un processeur, mais inférieur à l’ensemble des processeurs du nœud). Lorsque l’algorithme de parking principal choisit les cœurs à déparcer (en supposant qu’une augmentation de l’intensité de la charge de travail se produit), il ne peut pas toujours sélectionner les cœurs au sein du sous-ensemble affinité actif (ou sous-ensembles) pour le désinstaller, ce qui peut finir par désinstaller les cœurs qui ne seront pas réellement utilisés.

Les valeurs de ces paramètres sont des pourcentages compris entre 0 et 100. Le paramètre **nombre maximal** de cœurs de la performance du processeur gère le pourcentage maximal de cœurs pouvant être désimmobilisés (disponibles pour l’exécution des threads) à tout moment, tandis que le paramètre **performances du processeur cœurs du parking minimum** contrôle le pourcentage minimal de cœurs pouvant être désimmobilisés. Pour désactiver le parking central, définissez le paramètre **Core performance Core Park minimum cœurs** sur 100 pour cent à l’aide des commandes suivantes :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

Pour réduire le nombre de cœurs planifiables à 50% du nombre maximal, définissez le paramètre **Core performance Core maximum cores** sur 50 comme suit :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>Distribution de l’utilitaire de parking des performances du processeur

La distribution de l’utilitaire est une optimisation algorithmique de Windows Server 2016 conçue pour améliorer l’efficacité de certaines charges de travail. Il effectue le suivi de l’activité de l’UC (c’est-à-dire des appels DPC, des interruptions ou des threads strictement affinité) et prédit le travail à venir sur chaque processeur en partant de l’hypothèse que tout travail déplaçable peut être réparti uniformément entre tous les cœurs non immobilisés.

La distribution de l’utilitaire est activée par défaut pour le mode de gestion de l’alimentation équilibré pour certains processeurs. Cela peut réduire la consommation d’énergie du processeur en diminuant les fréquences d’UC requises des charges de travail qui sont dans un État raisonnablement stable. Toutefois, la distribution de l’utilitaire n’est pas nécessairement un bon choix d’algorithme pour les charges de travail soumises à des pics d’activité élevés ou à des programmes dans lesquels la charge de travail passe rapidement et de façon aléatoire sur les processeurs.

Pour ces charges de travail, nous vous recommandons de désactiver la distribution de l’utilitaire à l’aide des commandes suivantes :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>Voir aussi
- [Considérations relatives aux performances matérielles du serveur](../index.md)
- [Considérations relatives à la puissance du matériel du serveur](../power.md)
- [Réglage de la gestion de la puissance du processeur](processor-power-management-tuning.md)
- [Paramètres de planification équilibrée recommandés](recommended-balanced-plan-parameters.md)
