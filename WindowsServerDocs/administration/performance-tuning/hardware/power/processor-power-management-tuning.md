---
title: Réglage de la gestion de l’alimentation du processeur (PPM) pour le mode de gestion de l’alimentation équilibré de Windows Server
description: Réglage de la gestion de l’alimentation du processeur (PPM) pour le mode de gestion de l’alimentation équilibré de Windows Server
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 53399c1ff1d9fa60df992b922b99c82d119b2f58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355024"
---
# <a name="processor-power-management-ppm-tuning-for-the-windows-server-balanced-power-plan"></a>Réglage de la gestion de l’alimentation du processeur (PPM) pour le mode de gestion de l’alimentation équilibré de Windows Server

À compter de Windows Server 2008, Windows Server propose trois modes de gestion de l’alimentation : **Équilibre**, **hautes performances**et **économiseur d’énergie**. Le mode de gestion de l’alimentation **équilibré** est le choix par défaut qui vise à offrir la meilleure efficacité énergétique pour un ensemble de charges de travail serveur typiques. Cette rubrique décrit les charges de travail qui ont été utilisées pour déterminer les paramètres par défaut du schéma **équilibré** pour les versions précédentes de Windows.

Si vous exécutez un système serveur qui a des caractéristiques de charge de travail radicalement différentes ou des performances et des exigences d’alimentation supérieures à celles de ces charges de travail, vous souhaiterez peut-être ajuster les paramètres d’alimentation par défaut (c’est-à-dire créer un mode de gestion de l’alimentation personnalisé). L’une des sources d’informations de paramétrage utiles est l' [alimentation matérielle du serveur](../power.md). Vous pouvez également décider que le mode de gestion de l’alimentation à **hautes performances** est le bon choix pour votre environnement, en reconnaissant que vous aurez probablement un impact important sur l’énergie dans Exchange pour un certain niveau de réactivité accrue.

> [!IMPORTANT]
> Vous devez tirer parti des stratégies d’alimentation incluses dans Windows Server, sauf si vous avez besoin d’en créer une personnalisée et que vous comprenez parfaitement que vos résultats varient en fonction des caractéristiques de votre charge de travail.

## <a name="windows-processor-power-tuning-methodology"></a>Méthodologie de réglage de l’alimentation du processeur Windows


### <a name="tested-workloads"></a>Charges de travail testées

Les charges de travail sont sélectionnées pour couvrir un ensemble optimal de charges de travail Windows Server « typiques ». Évidemment, cet ensemble n’est pas destiné à être représentatif de l’ensemble des environnements de serveurs réels.

Le paramétrage de chaque stratégie d’alimentation est piloté par les données des cinq charges de travail suivantes :

-   **Charge de travail du serveur Web IIS**

    Un test d’évaluation interne de Microsoft appelé notions de base sur le Web est utilisé pour optimiser l’efficacité énergétique des plateformes qui exécutent le serveur Web IIS. Le programme d’installation de contient un serveur Web et plusieurs clients qui simulent le trafic d’accès Web. La distribution de pages Web dynamiques, statiques, à chaud (en mémoire) et à froid statiques (accès disque requis) est basée sur des études statistiques sur les serveurs de production. Pour pousser les cœurs d’UC du serveur vers une utilisation complète (une extrémité du spectre testé), le programme d’installation a besoin de ressources réseau et disque suffisamment rapides.

-   **Charge de travail de base de données SQL Server**

    Le test [TPC-E](http://www.tpc.org/tpce/default.asp) est un point de référence populaire pour l’analyse des performances des bases de données. Il est utilisé pour générer une charge de travail OLTP pour les optimisations de paramétrage de PPM. Cette charge de travail a des e/s disque importantes et a donc une exigence de performances élevée pour le système de stockage et la taille de la mémoire.

-   **Charge de travail du serveur de fichiers**

    Un test d’évaluation développé par Microsoft appelé [fsct](http://www.snia.org/sites/default/files2/sdc_archives/2009_presentations/tuesday/BartoszNyczkowski-JianYan_FileServerCapacityTool.pdf) est utilisé pour générer une charge de travail de serveur de fichiers SMB. Il crée un jeu de fichiers volumineux sur le serveur et utilise de nombreux systèmes clients (réels ou virtualisés) pour générer des opérations d’ouverture, de fermeture, de lecture et d’écriture de fichier. La combinaison d’opérations est basée sur des études statistiques sur les serveurs de production. Il souligne les ressources du processeur, du disque et du réseau.

-   **SPECpower – charge de travail JAVA**

    [SPECpower\_ssj2008](http://spec.org/power_ssj2008/) est le premier Benchmark des spécifications standard qui évalue conjointement les caractéristiques d’alimentation et de performances. Il s’agit d’une charge de travail Java côté serveur avec différents niveaux de charge processeur. Il ne nécessite pas beaucoup de ressources disque ou réseau, mais il a certaines exigences en matière de bande passante de mémoire. Presque toute l’activité de l’UC est effectuée en mode utilisateur ; l’activité en mode noyau n’a pas un impact important sur les caractéristiques d’alimentation et de performances des bancs d’essai, à l’exception des décisions relatives à la gestion de l’alimentation.

-   **Charge de travail du serveur d’applications**

    Le test [SAP-SD](http://global.sap.com/campaigns/benchmark/index.epx) est utilisé pour générer une charge de travail de serveur d’applications. Une installation à deux niveaux est utilisée, avec la base de données et le serveur d’applications sur le même hôte de serveur. Cette charge de travail utilise également le temps de réponse en tant que mesure de performance, qui diffère des autres charges de travail testées. Par conséquent, il est utilisé pour vérifier l’impact des paramètres PPM sur la réactivité. Toutefois, il n’est pas destiné à être représentatif de toutes les charges de travail de production sensibles à la latence.

Tous les tests d’évaluation, à l’exception de SPECpower, ont été conçus à l’origine pour l’analyse des performances et ont donc été créés pour s’exécuter à des niveaux de charge maximal. Toutefois, les niveaux de charge moyen à faible sont plus courants pour les serveurs de production réels et sont plus intéressants pour les optimisations de plan **équilibrées** . Nous exécutons intentionnellement les tests d’évaluation à des niveaux de charge variables de 100% de moins de 10% (en 10% d’étapes) à l’aide de différentes méthodes de limitation (par exemple, en réduisant le nombre d’utilisateurs/clients actifs).

### <a name="hardware-configurations"></a>Configurations matérielles

Pour chaque version de Windows, les serveurs de production les plus récents sont utilisés dans le processus d’analyse et d’optimisation du mode de gestion de l’alimentation. Dans certains cas, les tests ont été effectués sur des systèmes de pré-production dont la planification de publication correspondait à celle de la prochaine version de Windows.

Étant donné que la plupart des serveurs sont vendus avec des sockets de 1 à 4 processeurs, et étant donné que les serveurs de montée en puissance sont moins susceptibles d’avoir une efficacité énergétique comme préoccupation principale, les tests d’optimisation du mode de gestion de l’alimentation sont principalement exécutés sur les systèmes à 2 sockets et à 4 Sockets. La quantité de mémoire RAM, de disque et de ressources réseau pour chaque test est choisie pour permettre à chaque système de s’exécuter jusqu’à sa capacité maximale, tout en prenant en compte les restrictions de coût qui seraient normalement en place pour les environnements de serveurs réels, comme conserver le configurations raisonnables.

> [!IMPORTANT]
> Même si le système peut s’exécuter à sa charge maximale, nous optimisons généralement les niveaux de charge inférieurs, car les serveurs qui s’exécutent régulièrement à leurs pics de charge sont bien recommandés pour utiliser le mode de gestion de l’alimentation **hautes performances** , sauf si l’efficacité énergétique est élevée. importance.

### <a name="metrics"></a>metrics

Toutes les évaluations testées utilisent le débit comme mesure de performance. Le temps de réponse est considéré comme une exigence de contrat SLA pour ces charges de travail (à l’exception de SAP, où il s’agit d’une mesure principale). Par exemple, une exécution de benchmark est considérée comme « valide » si le temps de réponse moyen ou maximal est inférieur à une certaine valeur.

Par conséquent, l’analyse du paramétrage de PPM utilise également le débit comme mesure de performance.  Au niveau de charge le plus élevé (100% d’utilisation du processeur), notre objectif est que le débit ne doit pas diminuer de plus de quelques% en raison des optimisations de la gestion de l’alimentation. Mais la principale préoccupation est d’optimiser l’efficacité de l’alimentation (telle que définie ci-dessous) aux niveaux de charge moyenne et faible.

![formule d’efficacité énergétique](../../media/serverperf-ppm-formula.jpg)

L’exécution des cœurs de processeur à des fréquences inférieures réduit la consommation d’énergie. Toutefois, les fréquences inférieures réduisent généralement le débit et augmentent le temps de réponse. Pour le mode de gestion de l’alimentation **équilibré** , il existe un compromis intentionnel entre réactivité et efficacité énergétique. Les tests de charge de travail SAP, ainsi que les contrats SLA de temps de réponse sur les autres charges de travail, assurez-vous que l’augmentation du temps de réponse ne dépasse pas un certain seuil (5% comme exemple) pour ces charges de travail spécifiques.

> [!NOTE]
> Si la charge de travail utilise le temps de réponse comme mesure de performance, le système doit basculer vers le mode de gestion de l’alimentation **hautes performances** ou modifier le mode de gestion de l’alimentation **équilibré** comme indiqué dans [paramètres de mode d’alimentation équilibrée recommandés pour une réponse rapide Heure](recommended-balanced-plan-parameters.md).

### <a name="tuning-results"></a>Résultats du paramétrage

À compter de Windows Server 2008, Microsoft a travaillé avec Intel et AMD pour optimiser les paramètres PPM pour les processeurs de serveur les plus à jour pour chaque version de Windows. Un nombre énorme de combinaisons de paramètres en PPM ont été testées sur chacune des charges de travail précédemment décrites afin de trouver la meilleure efficacité énergétique à différents niveaux de charge. Étant donné que les algorithmes logiciels ont été améliorés et que les architectures de gestion de l’alimentation matérielle ont évolué, chaque nouveau Windows Server avait toujours une efficacité énergétique supérieure ou égale à celle des versions précédentes dans la gamme des charges de travail testées.

La figure suivante donne un exemple de l’efficacité énergétique sous différents niveaux de charge TPC-E sur un serveur de production à 4 sockets exécutant Windows Server 2008 R2. Il montre une amélioration de 8% des niveaux de charge moyenne par rapport à Windows Server 2008.

![Comparaison de l’efficacité énergétique](../../media/serverperf-ppm-figure1.jpg)

## <a name="customized-tuning-suggestions"></a>Suggestions de paramétrage personnalisées

Si les caractéristiques de votre charge de travail principale diffèrent considérablement des cinq charges de travail utilisées pour le réglage du plan **de gestion de** l’alimentation par défaut en ppm, vous pouvez faire des essais en modifiant un ou plusieurs paramètres ppm pour trouver la solution la mieux adaptée à votre environnement.

En raison du nombre et de la complexité des paramètres, il peut s’agir d’une tâche complexe, mais si vous recherchez le meilleur compromis entre la consommation d’énergie et l’efficacité de la charge de travail pour votre environnement particulier, il peut être utile de l’effort.

 L’ensemble complet de paramètres PPM réglables est disponible dans [réglage](https://msdn.microsoft.com/windows/hardware/gg566941.aspx)de la gestion de l’alimentation du processeur. Voici quelques-uns des paramètres d’alimentation les plus simples à utiliser :

-   Augmentation du **seuil des performances du processeur et augmentation des performances du processeur** : les valeurs les plus grandes ralentissent la réponse des performances à l’augmentation de l’activité

-   **Seuil de baisse des performances du processeur** : valeurs élevées Quicken de la réponse d’alimentation aux périodes d’inactivité

-   **Réduction des performances du processeur** : les valeurs les plus importantes diminuent progressivement les performances pendant les périodes d’inactivité.

-   **Stratégie d’augmentation des performances du processeur** : la stratégie « unique » ralentit la réponse des performances à une activité accrue et soutenue. la stratégie « Rocket » réagit rapidement pour augmenter l’activité

-   **Stratégie de réduction des performances du processeur** : la stratégie « unique » diminue progressivement les performances sur des périodes d’inactivité plus longues ; la stratégie « fusée » dépose l’énergie très rapidement lors de la saisie d’une période d’inactivité.

>[!Important]
> Avant de commencer les expériences, vous devez d’abord comprendre vos charges de travail, ce qui vous permet de choisir les choix de paramètres adéquats et de réduire l’effort de paramétrage.

### <a name="understand-high-level-performance-and-power-requirements"></a>Comprendre les performances et les exigences d’alimentation de haut niveau

Si votre charge de travail est en « temps réel » (par exemple, si elle est sujette à des problèmes ou à d’autres impacts visibles de l’utilisateur final) ou si elle a des exigences de réactivité très strictes (par exemple, une bourse d’actions) et si la consommation d’énergie n’est pas un critère principal pour votre environnement, vous devez probablement il vous suffit de basculer vers le mode de gestion de l’alimentation **hautes performances** . Dans le cas contraire, vous devez comprendre les exigences de temps de réponse de vos charges de travail, puis régler les paramètres PPM pour une efficacité énergétique optimale qui répond toujours à ces exigences.

### <a name="understand-underlying-workload-characteristics"></a>Comprendre les caractéristiques de charge de travail sous-jacentes

Vous devez connaître vos charges de travail et concevoir les jeux de paramètres d’expérimentation à des fins de paramétrage. Par exemple, si les fréquences des cœurs de processeur doivent être rampes très rapidement (peut-être que vous avez une charge de travail en rafale avec des périodes d’inactivité significatives, mais que vous avez besoin d’une réactivité très rapide lorsqu’une nouvelle transaction est utilisée), la stratégie d’augmentation des performances du processeur peut être défini sur « fusée » (ce qui, comme son nom l’indique, pousse la fréquence du cœur de l’UC à sa valeur maximale au lieu de l’exécuter sur une période donnée).

Si votre charge de travail est très intense, l’intervalle de vérification des PPM peut être réduit pour accélérer le démarrage de la fréquence d’UC après l’arrivée d’une rafale. Si votre charge de travail ne dispose pas d’une concurrence de thread élevée, le parking de base peut être activé pour forcer l’exécution de la charge de travail sur un plus petit nombre de cœurs, ce qui peut également éventuellement améliorer les taux d’accès au cache du processeur.

Si vous souhaitez simplement augmenter les fréquences d’UC à des niveaux d’utilisation moyenne (et non pas des niveaux de charge de travail légers), les seuils d’augmentation/baisse des performances du processeur peuvent être ajustés pour ne pas réagir tant que certains niveaux d’activité ne sont pas observés.

### <a name="understand-periodic-behaviors"></a>Comprendre les comportements périodiques

Il peut y avoir des exigences de performances différentes pour la journée et la nuit ou le week-end, ou il peut y avoir différentes charges de travail qui s’exécutent à des moments différents. Dans ce cas, un ensemble de paramètres PPM peut ne pas être optimal pour toutes les périodes. Étant donné que plusieurs modes de gestion de l’alimentation personnalisés peuvent être conçus, il est possible de régler les différentes périodes et de basculer entre les modes de gestion de l’alimentation via des scripts ou d’autres méthodes de configuration du système dynamique.

Là encore, cela augmente la complexité du processus d’optimisation. il s’agit donc d’une question de la valeur qui sera obtenue à partir de ce type de paramétrage, qui devra probablement être répétée en cas de mises à niveau matérielles importantes ou de modifications de charge de travail.

C’est pourquoi Windows fournit un mode de gestion de l’alimentation **équilibré** en premier lieu, car dans de nombreux cas, il n’est probablement pas utile de régler manuellement une charge de travail spécifique sur un serveur spécifique.

## <a name="see-also"></a>Voir aussi
- [Considérations relatives aux performances matérielles du serveur](../index.md)
- [Considérations relatives à la puissance du matériel du serveur](../power.md)
- [Réglage de la puissance et des performances](power-performance-tuning.md)
- [Réglage de la gestion de la puissance du processeur](processor-power-management-tuning.md)
- [Paramètres de planification équilibrée recommandés](recommended-balanced-plan-parameters.md)
