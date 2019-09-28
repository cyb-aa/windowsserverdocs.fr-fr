---
title: Paramètres du mode de gestion de l’alimentation équilibré recommandés pour les temps de réponse rapides
description: Paramètres du mode de gestion de l’alimentation équilibré recommandés pour le temps de réponse rapide
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 96037a577c9f2a835e9c49bf9339ed8dc6da1a6b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383513"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>Paramètres du mode de gestion de l’alimentation équilibré recommandés pour les charges de travail nécessitant des temps de réponse rapides

Le mode **de gestion de** l’alimentation par défaut utilise le **débit** comme mesure de performances pour le paramétrage. Dans l’état stable, le **débit** ne change pas avec des utilisations variables jusqu’à ce que le système soit entièrement surchargé (environ 100% d’utilisation).  Par conséquent, le mode de gestion de l’alimentation **équilibré** favorise la puissance beaucoup avec la minimisation de la fréquence du processeur et l’optimisation de l’utilisation.

Toutefois, le **temps de réponse** peut augmenter de façon exponentielle avec l’augmentation de l’utilisation. De nos jours, la nécessité d’un temps de réponse rapide a considérablement augmenté. Bien que Microsoft ait suggéré aux utilisateurs de basculer vers le mode de gestion de l’alimentation **hautes performances** lorsqu’ils ont besoin de temps de réponse rapides, certains utilisateurs ne souhaitent pas perdre le bénéfice de l’alimentation pendant les niveaux de charge moyenne à moyenne. Par conséquent, Microsoft fournit l’ensemble suivant de modifications de paramètres suggérées pour les charges de travail nécessitant du temps de réponse rapide.


| Paramètre | Description | Valeur par défaut | Valeur proposée |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Seuil d’augmentation des performances du processeur | Seuil d’utilisation au-delà duquel la fréquence doit augmenter | 90 | 60 |
| Seuil de baisse des performances du processeur | Seuil d’utilisation en dessous duquel la fréquence doit diminuer | 80 | 40 |
| Temps d’augmentation des performances du processeur | Nombre de fenêtres de vérification des PPM avant l’augmentation de la fréquence | 3 | 1 |
| Stratégie d’augmentation des performances du processeur | Vitesse d’augmentation de la fréquence | Single | Durée idéale |

Pour définir les valeurs proposées, les utilisateurs peuvent exécuter les commandes suivantes dans une fenêtre avec l’administrateur :

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

Cette modification est basée sur les performances et l’analyse de la consommation d’énergie à l’aide des charges de travail suivantes. Pour les utilisateurs qui souhaitent affiner l’efficacité de la gestion de l’alimentation avec certaines exigences de contrat SLA, consultez [considérations relatives aux performances matérielles du serveur](../power.md).

>[!Note]
> Pour obtenir des recommandations supplémentaires et des informations sur l’utilisation des modes de gestion de l’alimentation pour paramétrer les charges de travail virtualisées, consultez [configuration Hyper-v](../../role/hyper-v-server/configuration.md)

## <a name="specpower--java-workload"></a>SPECpower – charge de travail JAVA

[SPECpower @ no__t-1ssj2008](http://spec.org/power_ssj2008/), le test de référence des spécifications standard le plus populaire pour les caractéristiques de performances et de puissance des serveurs, est utilisé pour vérifier l’impact de l’alimentation. Étant donné qu’elle utilise uniquement le **débit** comme mesure de performance, le mode de gestion de l’alimentation **équilibré** par défaut offre la meilleure efficacité énergétique.

La modification de paramètre proposée consomme une puissance légèrement supérieure à la lumière (c’est-à-dire < = 20%). niveaux de charge. Mais avec le niveau de charge plus élevé, la différence augmente, et elle commence à utiliser la même puissance que le mode de gestion de l’alimentation **hautes performances** après le niveau de charge de 60%. Pour utiliser les paramètres de modification proposés, les utilisateurs doivent être conscients du coût d’alimentation à des niveaux de charge moyen à élevé pendant la planification de la capacité du rack.

## <a name="geekbench-3"></a>GeekBench 3

[GeekBench 3](http://www.geekbench.com/geekbench3/) est un test de processeur multiplateforme qui sépare les scores pour les performances à noyau unique et à plusieurs cœurs. Il simule un ensemble de charges de travail, notamment des charges de travail d’entiers (chiffrements, compressions, traitement d’image, etc.), des charges de travail à virgule flottante (modélisation, fractale, renforcement d’image, flou d’image, etc.) et des charges de travail de mémoire (streaming).

Le **temps de réponse** est une mesure majeure dans son calcul de score. Dans notre système testé, le mode de gestion de **l’alimentation par** défaut a ~ 18% de régression dans les tests à noyau unique et à environ 40% de régression dans les tests à plusieurs cœurs par rapport au mode de gestion de l’alimentation **hautes performances** . La modification proposée supprime ces régressions.

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd) est un outil de ligne de commande pour le stockage développé par Microsoft. Il est largement utilisé pour générer une série de requêtes sur les systèmes de stockage pour l’analyse des performances de stockage.

Nous avons configuré un [cluster de basculement] et utilisé Diskspd pour générer des e/s aléatoires et séquentielles, et pour lire et écrire des e/s sur les systèmes de stockage locaux et distants avec différentes tailles d’e/s. Nos tests montrent que le temps de réponse d’e/s est très sensible à la fréquence du processeur dans différents modes de gestion de l’alimentation. Le mode de gestion de l’alimentation **équilibré** peut doubler le temps de réponse du mode de gestion de l’alimentation **hautes performances** sous certaines charges de travail. La modification proposée supprime la plupart des régressions.

>[!Important]
>À partir de processeurs Intel [Broadwell] exécutant Windows Server 2016, la plupart des décisions de gestion de l’alimentation du processeur sont prises au niveau du processeur au lieu du système d’exploitation pour obtenir une adaptation plus rapide aux modifications de la charge de travail. Les paramètres PPM hérités utilisés par le système d’exploitation ont un impact minimal sur les décisions de fréquence réelle, à l’exception du fait d’indiquer au processeur s’il doit privilégier l’alimentation ou les performances, ou limiter les fréquences minimales et maximales. Par conséquent, le paramètre proposé PPM change est uniquement ciblé sur les systèmes antérieurs à Broadwell.

## <a name="see-also"></a>Voir aussi
- [Considérations relatives aux performances matérielles du serveur](../index.md)
- [Considérations relatives à la puissance du matériel du serveur](../power.md)
- [Réglage de la puissance et des performances](power-performance-tuning.md)
- [Réglage de la gestion de la puissance du processeur](processor-power-management-tuning.md)
- [Cluster de basculement](https://technet.microsoft.com/library/cc725923.aspx)
