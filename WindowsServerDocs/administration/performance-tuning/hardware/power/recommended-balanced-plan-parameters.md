---
title: Recommandé de paramètres de Plan d’alimentation à charge équilibrée pour le temps de réponse rapides
description: Recommandé de paramètres de Plan d’alimentation à charge équilibrée pour le temps de réponse rapide
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 134e868e1400729f754039fc8120cea0c73945bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878790"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>Recommandé de paramètres de Plan d’alimentation à charge équilibrée pour les charges de travail nécessitant des temps de réponse rapides

La valeur par défaut **équilibré** power plan utilise **débit** en tant que les mesures de performance pour le paramétrage. Au cours de l’état stable, **débit** ne change pas avec les différentes utilisations jusqu'à ce que le système est totalement surchargé (environ 100 % d’utilisation).  Par conséquent, le **équilibré** power beaucoup de choses avec en réduisant la fréquence du processeur et d’optimiser l’utilisation du favorise la gestion de l’alimentation.

Toutefois **temps de réponse** peut augmenter de manière exponentielle avec l’utilisation augmente. Aujourd'hui, l’exigence du temps de réponse rapide a augmenté. Même si Microsoft suggérée aux utilisateurs de basculer vers le **hautes performances** de l’alimentation lorsqu’ils ont besoin de temps de réponse rapide, certains utilisateurs ne voulez pas perdre l’avantage de l’alimentation au cours de la lumière aux niveaux de charge moyenne. Par conséquent, Microsoft fournit l’ensemble suivant de modifications apportées aux paramètres suggérés pour les charges de travail nécessitant des temps de réponse rapide.


| Paramètre | Description | Valeur par défaut | Valeur proposée |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Seuil d’augmentation de performances de processeur | Seuil d’utilisation ci-dessus c'est-à-dire pour augmenter la fréquence | 90 | 60 |
| Seuil de diminution de performances de processeur | Seuil d’utilisation ci-dessous c'est-à-dire afin de réduire la fréquence | 80 | 40 |
| Augmentation du temps des performances processeur | Nombre de fenêtres de cocher PPM avant que la fréquence est d’augmenter | 3 | 1 |
| Stratégie d’augmentation des performances processeur | La fréquence est la rapidité d’augmenter | Unique | Durée idéale |

Pour définir les valeurs proposées, les utilisateurs peuvent exécuter les commandes suivantes dans une fenêtre avec l’administrateur :

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

Cette modification est basée sur les performances et l’analyse de compromis d’alimentation à l’aide de charges de travail suivantes. Pour les utilisateurs qui souhaitent plus fine paramétrer l’efficacité de l’alimentation avec certaines exigences de contrat SLA, reportez-vous à [les considérations sur les performances du matériel serveur](../power.md).

>[!Note]
> Pour des recommandations supplémentaires et des informations sur l’utilisation des modes d’alimentation pour paramétrer les charges de travail virtualisées, lisez [Configuration Hyper-v](../../role/hyper-v-server/configuration.md)

## <a name="specpower--java-workload"></a>SPECpower – la charge de travail JAVA

[SPECpower\_ssj2008](http://spec.org/power_ssj2008/), le plus populaire référence SPEC standard de l’industrie des caractéristiques de puissance et les performances de serveur, est utilisé pour vérifier l’impact de l’alimentation. Dans la mesure où il utilise uniquement des **débit** en tant que mesure de performances, la valeur par défaut **équilibré** gestion de l’alimentation fournit la meilleure efficacité énergétique.

La modification du paramètre proposé consomme power légèrement supérieure à la lumière (par exemple, < = 20 %) niveaux de charge. Mais avec la plus élevée accroissement de la charge de niveau, la différence, et cela commence à consommer la même puissance que la **hautes performances** après le niveau de 60 % de la charge de l’alimentation. Pour utiliser les paramètres de la modification proposée, les utilisateurs doivent connaître le coût d’électricité aux moyennes et les niveaux de charge élevée lors de leur planification de la capacité de rack.

## <a name="geekbench-3"></a>GeekBench 3

[GeekBench 3](http://www.geekbench.com/geekbench3/) est un banc d’essai de processeur d’inter-plateformes qui sépare les scores de performances à cœur unique et multicœur. Il simule un jeu de charges de travail, y compris les charges de travail entière (chiffrements compressions, traitement d’image, etc.), les charges de travail point flottant (modélisation, fractale, netteté d’image, Flou de l’image, etc.) et de charges de travail de mémoire (streaming).

**Temps de réponse** est une mesure majeure dans son calcul de score. Dans notre système testé, la valeur par défaut **équilibré** de l’alimentation a environ 18 % régression de régression de tests et d’environ 40 % à cœur unique dans les tests d’unité centrale multicœur par rapport à la **hautes performances** de l’alimentation. La modification proposée supprime ces régressions.

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd) est un outil de ligne de commande de stockage des tests de performances développés par Microsoft. Il est largement utilisé pour générer un ensemble de demandes adressées à des systèmes de stockage pour l’analyse de performances de stockage.

Nous avons défini un [cluster de basculement] et Diskspd permet de générer aléatoire et séquentiel et de lecture et de sorties d’écriture pour les systèmes de stockage local et distant avec différentes tailles d’e/s. Nos tests montrent que le temps de réponse d’e/s est très sensible à la fréquence du processeur sous différents modes d’alimentation. Le **équilibré** de l’alimentation peut doubler le temps de réponse de celui de la **hautes performances** sous certaines charges de travail de l’alimentation. La modification proposée supprime la plupart des régressions.

>[!Important]
>À partir de processeurs Intel [Broadwell] exécutant Windows Server 2016, la plupart des décisions de gestion d’alimentation processeur est effectuée dans le processeur plutôt qu’au niveau du système d’exploitation pour obtenir une adaptation rapide aux changements de charge de travail. Les paramètres PPM hérités utilisés par le système d’exploitation ont un impact minimal sur les décisions de fréquence réelle, à l’exception indiquant le processeur si elle doit privilégier alimentation ou les performances ou limitant les fréquences minimale et maximale. Par conséquent, la modification du paramètre PPM proposée vise uniquement pour les systèmes de pre-Broadwell.

## <a name="see-also"></a>Voir aussi
- [Les considérations sur les performances du matériel serveur](../index.md)
- [Considérations sur le serveur matériel Power](../power.md)
- [Alimentation et réglage des performances](power-performance-tuning.md)
- [Paramétrage de la gestion de l’alimentation de processeur](processor-power-management-tuning.md)
- [Cluster de basculement](https://technet.microsoft.com/library/cc725923.aspx)
