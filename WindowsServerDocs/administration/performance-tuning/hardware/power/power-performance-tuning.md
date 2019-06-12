---
title: Puissance et performance Tuning
description: Processeur gestion de l’alimentation (PPM) de Windows Server à charge équilibrée alimentation de paramétrage
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 4ad58e9b477f61844dedd9f6638efb12f1a96500
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811563"
---
# <a name="power-and-performance-tuning"></a>Alimentation et réglage des performances

L’efficacité énergétique est plus en plus importante dans les environnements de centres de données et d’entreprise, et il ajoute un autre ensemble de compromis à la combinaison des options de configuration.

Windows Server 2016 est optimisé pour l’efficacité énergétique excellente avec un impact sur les performances minimale pour un large éventail de charges de travail client. [Paramétrage de gestion d’alimentation (PPM) de processeur pour le serveur à charge équilibrée gestion de l’alimentation](processor-power-management-tuning.md) décrit les charges de travail utilisées pour régler les paramètres par défaut dans Windows Server 2016 et fournit des suggestions pour les réglages personnalisés.

Cette section se développe sur les inconvénients de l’efficacité énergétique pour vous aider à prendre des décisions avisées si vous avez besoin d’ajuster les paramètres d’alimentation par défaut sur votre serveur. Toutefois, la majorité des charges de travail et de matériel de serveur ne doit pas nécessiter administrateur réglage de puissance lors de l’exécution de Windows Server 2016.

## <a name="calculating-server-energy-efficiency"></a>Calcul de l’efficacité énergétique de serveur

Lorsque vous paramétrez votre serveur pour l’économie d’énergie, vous devez également prendre en compte les performances. Paramétrage affecte les performances et puissance, parfois dans les quantités disproportionnées. Pour chaque ajustement possible, pensez à vos objectifs de performances et de budget power pour déterminer si le compromis est acceptable.

Vous pouvez calculer le taux de rendement énergétique de votre serveur pour une mesure utile qui incorpore les informations de puissance et de performances. L’efficacité énergétique est le rapport de travail est effectué sur le moyen d’électricité qui est nécessaire pendant un laps de temps.

![formule de l’efficacité énergétique](../../media/perftune-guide-power-formula.png)

Vous pouvez utiliser cette mesure pour définir des objectifs de pratiques qui respectent le compromis entre la puissance et les performances. En revanche, des objectifs des économies d’énergie de 10 pour cent au sein du centre de données ne peut pas capturer les effets correspondants sur les performances et vice versa.

De même, si vous paramétrez votre serveur pour améliorer les performances de 5 % et qui se traduit par la consommation d’énergie de 10 pour cent plus élevée, le résultat total peut ou ne peut pas être acceptable pour vos objectifs professionnels. La métrique de l’efficacité énergétique permet des décisions mieux informées que les métriques d’alimentation ou les performances uniquement.

## <a name="measuring-system-energy-consumption"></a>Consommation d’énergie de système de mesure

Vous devez établir une mesure de puissance de référence avant de vous adapter votre serveur pour l’efficacité énergétique.

Si votre serveur dispose de la prise en charge nécessaire, vous pouvez utiliser la puissance de contrôle et la budgétisation de fonctionnalités dans Windows Server 2016 pour afficher la consommation d’énergie au niveau du système à l’aide de l’Analyseur de performances.

Une façon de déterminer si votre serveur est prise en charge pour le contrôle et la budgétisation consiste à examiner le [catalogue Windows Server](http://www.windowsservercatalog.com). Si votre modèle de serveur est éligible pour la nouvelle qualification de gestion de l’alimentation améliorée dans le programme de Certification de matériel Windows, il est garanti pour prendre en charge la fonctionnalité de contrôle et la budgétisation.

Un autre moyen pour le contrôle de prise en charge consiste à rechercher manuellement pour les compteurs de l’Analyseur de performances. Ouvrez l’Analyseur de performances, sélectionnez **ajouter des compteurs**, puis recherchez le **jauge d’alimentation** groupe de compteurs.

Si les instances nommées de jauges d’alimentation s’affichent dans la zone intitulée **Instances de l’objet sélectionné**, votre prend en charge de la plateforme de contrôle. Le **Power** compteur qui montre la puissance en watts apparaît dans le groupe de compteur sélectionné. La dérivation exacte de la valeur de données d’alimentation n’est pas spécifiée. Par exemple, il peut être un dessin power instantanée ou un dessin moyen d’électricité dans un intervalle de temps.

Si votre plateforme de serveur ne prend pas en charge le contrôle, vous pouvez utiliser un périphérique de contrôle physique connecté à l’alimentation pour mesurer l’énergie de dessin ou de la consommation.

Pour établir une ligne de base, vous devez mesurer le moyen d’électricité requis à différents stades de charge système, du mode inactif à 100 pour cent (débit maximal) pour générer une ligne de la charge. La figure suivante montre les lignes de charge pour les trois exemples de configurations :

![exemples de lignes de charge](../../media/perftune-guide-sample-loadlines.png)

Vous pouvez utiliser des lignes de charge pour évaluer et comparer les performances et la consommation d’énergie des configurations à tous les points de la charge. Dans cet exemple particulier, il est facile de voir ce qui est la meilleure configuration. Toutefois, il peut facilement être scénarios où une seule configuration mieux adapté aux charges de travail et un mieux adapté aux charges de travail légères.

Vous devez bien comprendre vos besoins de la charge de travail pour choisir une configuration optimale. Ne supposez pas que lorsque vous trouvez une configuration idéale, il est toujours optimal. Vous devez mesurer système utilisation consommation d’énergie de manière régulière et après les modifications de charges de travail, les niveaux de charge de travail ou matériel de serveur.

## <a name="diagnosing-energy-efficiency-issues"></a>Diagnostic des problèmes de l’efficacité énergétique

**PowerCfg.exe** prend en charge une option de ligne de commande que vous pouvez utiliser pour analyser l’efficacité énergétique inactive de votre serveur. Lorsque vous exécutez PowerCfg.exe avec le **/energy** option, l’outil effectue un test de 60 secondes pour détecter les problèmes de l’efficacité de consommation d’énergie. L’outil génère un rapport HTML simple dans le répertoire actif.

> [!Important]
> Pour garantir une analyse précise, assurez-vous que toutes les applications locales sont fermées avant d’exécuter **PowerCfg.exe**. 

Réduit le taux de tick du minuteur, pilotes de prise en charge de manque power management et l’utilisation excessive du processeur quelques-uns des problèmes de comportements qui sont détectés par le **powercfg /energy** commande. Cet outil fournit un moyen simple d’identifier et résoudre les problèmes de gestion d’alimentation, pouvant provoquer des réductions de coût significatives dans un grand centre de données.

Pour plus d’informations sur PowerCfg.exe, consultez [à l’aide de PowerCfg pour évaluer l’efficacité énergétique du système](https://msdn.microsoft.com/windows/hardware/gg463250.aspx).

## <a name="using-power-plans-in-windows-server"></a>À l’aide de la gestion de l’alimentation dans Windows Server

Windows Server 2016 a trois modes d’alimentation intégrés conçus pour répondre aux différents ensembles de besoins professionnels. Ces plans fournissent un moyen simple pour vous permettent de personnaliser un serveur pour répondre aux objectifs de l’alimentation ou les performances. Le tableau suivant décrit les plans, répertorie les scénarios courants dans lesquels utiliser chaque plan et donne des détails d’implémentation pour chaque plan.

| **Planification** | **Description** | **Scénarios applicables courants** | **Caractéristiques de l’implémentation** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| À charge équilibrée (recommandé) | Paramètre par défaut. Cible de l’efficacité énergétique bonne avec un impact minimal sur les performances. | Général de l’informatique | Correspond à la capacité à la demande. Les fonctionnalités d’économie d’énergie équilibrent la puissance et les performances. |
| Hautes performances | Augmente les performances au détriment de la consommation d’énergie élevée. Alimentation et des limitations thermiques, fonctionnement et les considérations relatives à la fiabilité s’appliquent. | Les applications à faible latence et le code d’application est sensible aux modifications des performances de processeur | Processeurs sont toujours verrouillées lors de l’état de performance le plus élevé (y compris des fréquences de « turbo »). Tous les cœurs sont unparked. Sortie THERMIQUE peut être importante. |
| Économiseur d’énergie | Limite les performances pour économiser de l’énergie et réduire les coûts d’exploitation. Non recommandé sans tests approfondis pour s’assurer que performance n’est pas suffisante. | Déploiements avec budgets d’alimentation limitées et contraintes thermiques | Limite de fréquence de processeur à un pourcentage de valeur maximale (si pris en charge) et permet d’autres fonctionnalités d’économie d’énergie. |


Ces modes d’alimentation existent dans Windows (CA) en courant alternatif et courant (DC) powered systèmes, mais nous supposerons que serveurs utilisent toujours une source d’alimentation.

Pour plus d’informations sur les modes d’alimentation et de configurations de stratégie d’alimentation, consultez [Configuration de la stratégie d’alimentation et de déploiement dans Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

> [!Note]
> Certains fabricants de serveur ont leurs propres options de gestion d’alimentation disponibles via les paramètres du BIOS. Si le système d’exploitation n’a pas de contrôle sur la gestion de l’alimentation, les modes d’alimentation dans Windows de modification n’affectent pas, performances et alimentation du système.

## <a name="tuning-processor-power-management-parameters"></a>Réglage des paramètres de gestion de puissance de processeur

Chaque mode d’alimentation représente une combinaison de nombreux paramètres de gestion d’alimentation sous-jacent. Les plans intégrés sont les trois collections de paramètres recommandés qui couvrent un large éventail de scénarios et des charges de travail. Toutefois, nous reconnaissons que ces plans ne répondra pas aux besoins de chaque client.

Les sections suivantes décrivent les façons de régler certains paramètres de gestion d’alimentation processeur spécifique pour répondre aux objectifs ne pas traités dans les trois modes de gestion intégrées. Si vous avez besoin de comprendre une plus large gamme de paramètres d’alimentation, consultez [Configuration de la stratégie d’alimentation et de déploiement dans Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

## <a name="processor-performance-boost-mode"></a>Mode de renforcement de performances de processeur

Technologies Intel Turbo Boost et AMD Turbo CORE sont des fonctionnalités qui permettent de processeurs obtenir des performances supplémentaires lorsqu’il est plus utile (autrement dit, au système haute la charge). Toutefois, cette fonctionnalité augmente la consommation d’énergie du processeur core, donc Windows Server 2016 configure technologies Turbo selon la stratégie d’alimentation qui se trouve dans l’utilisation et l’implémentation de processeur spécifique.

Turbo est activée pour la gestion de l’alimentation haute Performance sur tous les processeurs Intel et AMD et qu’elle est désactivée pour la gestion de l’alimentation économies d’énergie. Pour les modes d’alimentation à charge équilibrée sur les systèmes qui s’appuient sur des fréquences basées sur P-état traditionnel, Turbo est activé par défaut uniquement si la plateforme prend en charge le Registre EPB.

> [!Note]
> Le Registre EPB est uniquement pris en charge Intel Westmere et des processeurs plus loin.

Pour les processeurs Intel Nehalem et AMD, Turbo est désactivée par défaut sur les plateformes P-état. Toutefois, si un système prend en charge la collaboration processeur performances contrôle (CPPC), qui est un autre nouveau mode de communication de performances entre le système d’exploitation et le matériel (défini dans ACPI 5.0), Turbo peut-être s’appliquer si le système d’exploitation de Windows système demande dynamiquement le matériel pour offrir les meilleurs niveaux de performances possibles.

Pour activer ou désactiver la fonctionnalité Turbo Boost, le paramètre de Mode de renforcement de performances de processeur doit être configuré par l’administrateur ou par les paramètres par défaut pour le mode d’alimentation choisi. Mode de renforcement de performances de processeur a cinq valeurs autorisées, comme indiqué dans le tableau 5.

Pour le contrôle en fonction P-état, les choix sont désactivées, activé (Turbo est disponible pour le matériel chaque fois que les performances nominale sont demandé) et efficaces (l’option Turbo est disponible uniquement si le Registre EPB est implémenté).

Pour le contrôle en fonction du CPPC, les choix sont désactivées, activé efficace (Windows spécifie la quantité exacte de Turbo pour fournir) et agressif (Windows demande « performances maximales » pour activer Turbo).

Dans Windows Server 2016, la valeur par défaut pour le Mode Boost est 3.

| **Nom** | **Comportement en fonction P-état** | **Comportement CPPC** |
|--------------------------|------------------------|-------------------|
| 0 (désactivé) | Désactivée | Désactivée |
| 1 (activé) | Enabled | Activé efficace |
| 2 (agressif) | Enabled | Agressive |
| 3 (efficace activé) | Efficace | Activé efficace |
| 4 (efficace agressif) | Efficace | Agressive |

 
Les commandes suivantes activer le Mode de renforcement de performances de processeur sur le mode d’alimentation actuelle (spécifier la stratégie à l’aide d’un alias GUID) :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> Vous devez exécuter le **powercfg - setactive** commande pour activer les nouveaux paramètres. Vous n’avez pas besoin de redémarrer le serveur.

Pour définir cette valeur pour les modes d’alimentation autre que le plan sélectionné, vous pouvez utiliser les alias tels que schéma\_max. (économies d’énergie) et schéma\_MIN (haute Performance) et le schéma\_équilibré (équilibré) à la place de schéma\_Actuel. Remplacez « schéma en cours » dans les commandes de - setactive powercfg présentée précédemment avec l’alias de votre choix pour activer ce mode d’alimentation.

Par exemple, pour ajuster le Mode de renforcement dans le plan d’économie d’énergie et de rendre ce économiseur d’énergie est le plan actuel, exécutez les commandes suivantes :

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>Au minimum et état des performances de processeur maximal

Processeurs convertir entre les États de performances (P-États) très rapidement offre de correspondance à la demande, qui offre des performances lorsque cela est nécessaire et économie d’énergie lorsque cela est possible. Si votre serveur a des exigences spécifiques de hautes performances ou la consommation d’énergie au minimum, vous pouvez envisager de configurer le **état de Performance de processeur Minimum** paramètre ou le **optimiser les performances du processeur État** paramètre.

Les valeurs pour le **état de Performance de processeur Minimum** et **état de Performance de processeur maximale** paramètres sont exprimés sous forme de pourcentage de la fréquence maximale du processeur, avec une valeur dans la plage 0- 100.

Si votre serveur nécessite une latence très faible, invariant fréquence du processeur (par exemple, pour effectuer des tests renouvelables) ou les niveaux de performances le plus élevés, vous ne souhaiterez pas les processeurs de commutation aux États de performance inférieur. Pour ce type de serveur, vous pouvez limiter l’état des performances processeur minimum à 100 % en utilisant les commandes suivantes :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

Si votre serveur nécessite une faible consommation d’énergie, vous souhaiterez limiter l’état de performances de processeur à un pourcentage de valeur maximale. Par exemple, vous pouvez limiter le processeur de 75 pour cent de sa fréquence maximale en utilisant les commandes suivantes :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> Limitation des performances du processeur en pourcentage du nombre maximal nécessite la prise en charge du processeur. Consultez la documentation de processeur pour déterminer l’existence de cette prise en charge, ou d’afficher le compteur Analyseur de performances **% de fréquence maximale** dans le **processeur** groupe pour voir si tout en majuscules de fréquence appliqué.

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>Performances du processeur augmentent et diminuent de seuils et des stratégies

La vitesse à laquelle un état de performances du processeur augmente ou diminue est contrôlée par plusieurs paramètres. Les quatre paramètres suivants ont l’impact de la plus visible :

-   **Seuil d’augmenter la Performance processeur** définit la valeur d’utilisation ci-dessus afin d’augmente état des performances d’un processeur. Plus grandes valeurs ralentissent le taux d’accroissement de l’état des performances en réponse à des activités accrues.

-   **Seuil de diminution de performances de processeur** définit la valeur d’utilisation ci-dessous diminuer état des performances d’un processeur. Valeurs plus élevées augmentent la vitesse de sortie de l’état des performances pendant les périodes d’inactivité.

-   **Stratégie d’augmenter la Performance processeur et la diminution des performances de processeur** stratégie déterminer quel état de performances doit être définie quand une modification se produit. Stratégie « Single » signifie qu’il choisit le prochain état. « Rocket » signifie que l’état de performances d’alimentation maximale ou minimale. « Idéale » tente de trouver un équilibre entre puissance et les performances.

Par exemple, si votre serveur nécessite une latence très faible tout en voulant toujours tirer profit de faible consommation d’énergie durant les périodes d’inactivité, vous pourriez quicken l’augmentation des performances état pour une augmentation de charge et ralentir la baisse lors de la charge tombe en panne. Les commandes suivantes de définir la stratégie d’augmentation pour « Rocket » pour une augmentation d’état plus rapide et de définir la stratégie de réduction pour « Unique ». Les seuils d’augmentation et diminution sont définies respectivement sur 10 et 8.

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>Cœur de performances de processeur maximales et minimales de cœurs de stationnement

L’immobilisation de cœur est une fonctionnalité qui a été introduite dans Windows Server 2008 R2. Le moteur de gestion (PPM) de puissance processeur et le planificateur fonctionnent ensemble pour ajuster dynamiquement le nombre de cœurs disponibles exécuter les threads. Le moteur PPM choisit un nombre minimal de cœurs pour les threads qui seront planifiées.

Cœurs sont généralement stationnés n’ont pas tous les threads planifiées, et ils supprime dans les États de très faible consommation d’énergie quand ils ne traitent pas les interruptions, les appels DPC ou les autres tâches strictement avec affinité. Les cœurs restants sont responsables de la suite de la charge de travail. L’immobilisation de cœur peut potentiellement accroître l’efficacité énergétique lors d’une utilisation inférieure.

Pour la plupart des serveurs, le comportement d’immobilisation de cœur par défaut fournit un équilibre raisonnable de débit et le rendement énergétique. Processeurs où l’immobilisation de cœur peut ne pas affiche autant d’avantage sur les charges de travail génériques, il peut être désactivé par défaut.

Si votre serveur a des exigences de parcage spécifique, vous pouvez contrôler le nombre de cœurs disponibles pour park à l’aide de la **de stationnement maximale cœurs de processeur performances Core** paramètre ou le **processeur Nombre Minimum de stationnement performances Core cœurs** paramètre dans Windows Server 2016.

L’immobilisation de cœur n’est pas toujours optimale pour un scénario est lorsqu’il existe un ou plusieurs threads actifs associés à un sous-ensemble non négligeable de processeurs dans un nœud NUMA (autrement dit, plus de 1 processeur, mais inférieur à l’ensemble des processeurs sur le nœud). Lors du choix des cœurs à unpark par l’algorithme de stationnement principal (en supposant une augmentation de l’intensité de la charge de travail se produit), il peut ne pas sélectionne les cœurs dans le sous-ensemble avec affinité active (ou sous-ensembles) unpark toujours, et par conséquent peut finissent unparking cœurs qui ne fait pas utilisé.

Les valeurs pour ces paramètres sont des pourcentages dans la plage 0-100. Le **de stationnement maximale cœurs de processeur performances Core** paramètre contrôle le pourcentage maximal de cœurs qui peuvent être unparked (disponible pour exécuter les threads) à tout moment, tandis que le **processeur performances Core stationnement Nombre minimal de cœurs** paramètre contrôle le pourcentage minimal de cœurs qui peuvent être unparked. Pour désactiver l’immobilisation de cœur, définissez le **processeur performances Core stationnement nombre Minimum de cœurs** paramètre à 100 % en utilisant les commandes suivantes :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

Pour réduire le nombre de cœurs planifiables à 50 % du nombre maximal, définissez le **de stationnement maximale cœurs de processeur performances Core** paramètre à 50 comme suit :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>Cœur de performances de processeur la distribution de l’utilitaire de parking

Utilitaire de Distribution est une optimisation algorithmique dans Windows Server 2016 est conçu pour améliorer l’efficacité de puissance pour certaines charges de travail. Il effectue le suivi de l’activité du processeur non déplaçable (autrement dit, les appels DPC, interruptions ou strictement avec affinité des threads), et qu’il prévoit les travaux futurs pour chaque processeur basée sur l’hypothèse que n’importe quel travail mobile peut être répartie entre tous les cœurs unparked.

Utilitaire de Distribution est activée par défaut pour le mode d’alimentation à charge équilibrée pour certains processeurs. Il peut réduire la consommation de processeur en réduisant la fréquence du processeur demandée de charges de travail qui se trouvent dans un état stable raisonnablement. Toutefois, utilitaire de Distribution n’est pas nécessairement algorithmique idéale pour les charges de travail qui sont soumises aux pics d’activité élevée ou pour les programmes où la charge de travail rapidement et de façon aléatoire se déplace entre les processeurs.

Pour ces charges de travail, nous vous recommandons de la désactivation de la Distribution de l’utilitaire en utilisant les commandes suivantes :

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>Voir aussi
- [Les considérations sur les performances du matériel serveur](../index.md)
- [Considérations relatives à la puissance du matériel du serveur](../power.md)
- [Réglage de la gestion de la puissance du processeur](processor-power-management-tuning.md)
- [Paramètres de planification équilibrée recommandés](recommended-balanced-plan-parameters.md)
