---
title: Processeur gestion de l’alimentation (PPM) de Windows Server à charge équilibrée alimentation de paramétrage
description: Processeur gestion de l’alimentation (PPM) de Windows Server à charge équilibrée alimentation de paramétrage
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 8a2ef4fd39554446aaac686e142ad24f53b4efaa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863050"
---
# <a name="processor-power-management-ppm-tuning-for-the-windows-server-balanced-power-plan"></a>Processeur gestion de l’alimentation (PPM) de Windows Server à charge équilibrée alimentation de paramétrage

À compter de Windows Server 2008, Windows Server fournit trois modes d’alimentation : **Équilibré**, **hautes performances**, et **de veille de l’alimentation**. Le **équilibré** gestion de l’alimentation est le choix par défaut qui vise à donner à l’efficacité énergétique pour un jeu de charges de travail de serveur classique. Cette rubrique décrit les charges de travail qui ont été utilisées pour déterminer les paramètres par défaut pour le **équilibré** schéma pour les cours des dernières versions de Windows.

Si vous exécutez un système de serveur qui a les caractéristiques des charges de travail considérablement différent ou performances en matière de puissance que ces charges de travail, vous souhaiterez peut-être prendre en compte le réglage des paramètres d’alimentation par défaut (par exemple, créer un mode d’alimentation personnalisé). Une source d’informations de paramétrage utiles est la [considérations sur le serveur matériel Power](../power.md). Alternativement, vous pouvez décider que le **hautes performances** de l’alimentation est le bon choix pour votre environnement, reconnaissant que prendra probablement une quantité importante d’énergie d’accès en échange d’un niveau de réactivité accrue.

>[!Important]
> Vous devez tirer parti les stratégies d’alimentation qui sont inclus avec Windows Server, sauf si vous avez besoin pour créer un personnalisé et avoir une très bonne compréhension que vos résultats peuvent varier selon les caractéristiques de votre charge de travail spécifique.

## <a name="windows-processor-power-tuning-methodology"></a>Méthodologie de réglage de puissance Windows processeur


### <a name="tested-workloads"></a>Charges de travail testés

Charges de travail sont sélectionnés pour couvrir un ensemble de meilleur effort de « typical ? Charges de travail de Windows Server. Évidemment, ce jeu n’est pas destiné à être représentative de l’éventail complet des environnements de serveurs du monde réel.

Le réglage dans chaque stratégie d’alimentation est piloté par les cinq charges de travail suivantes :

-   **Charge de travail de serveur Web IIS**

    Un test d’évaluation Microsoft interne appelée notions de base de Web est utilisé pour optimiser l’efficacité énergétique de plates-formes de serveur Web IIS en cours d’exécution. Le programme d’installation contient un serveur web et plusieurs clients qui simulent le trafic d’accès web. La distribution dynamique, statique à chaud (en mémoire) et à froid statique (accès disque requis) des pages web est basée sur l’étude statistique de serveurs de production. Pour transmettre des cœurs de processeur du serveur à pleine utilisation (une extrémité du spectre testé), le programme d’installation a besoin de ressources de réseau et de disque suffisamment rapides.

-   **Charge de travail de base de données SQL Server**

    Le [TPC-E](http://www.tpc.org/tpce/default.asp) banc d’essai est un test d’évaluation populaires pour l’analyse des performances de base de données. Il est utilisé pour générer une charge de travail OLTP pour les optimisations de paramétrage PPM. Cette charge de travail a des e/s disque importantes et par conséquent elle possède une exigence de haute performance pour la taille de mémoire et de système de stockage.

-   **Charge de travail**

    Un test d’évaluation de Microsoft a développé appelé [FSCT](http://www.snia.org/sites/default/files2/sdc_archives/2009_presentations/tuesday/BartoszNyczkowski-JianYan_FileServerCapacityTool.pdf) est utilisé pour générer une charge de travail du serveur de fichiers SMB. Il crée un fichier volumineux sur le serveur et utilise de nombreux systèmes client (réelles ou virtualisés) à générer le fichier ouvrir, fermer, lire et écrire des opérations. La combinaison de l’opération est basée sur l’étude statistique de serveurs de production. Il souligne les UC, disque et les ressources réseau.

-   **SPECpower – la charge de travail JAVA**

    [SPECpower\_ssj2008](http://spec.org/power_ssj2008/) est la première référence SPEC standard du secteur qui prend la valeur conjointement les caractéristiques de puissance et de performances. Il s’agit d’une charge de travail Java côté serveur avec différents niveaux de charge du processeur. Il ne nécessite pas beaucoup de ressources disque ou réseau, mais elle implique certaines exigences de bande passante de mémoire. Presque toute l’activité du processeur est effectuée en mode utilisateur ; activité de noyau n’a pas d’impact sur la puissance des bancs d’essai et des caractéristiques de performances à l’exception les décisions de gestion d’alimentation.

-   **Charge de travail Application Server**

    Le [SAP SD](http://global.sap.com/campaigns/benchmark/index.epx) banc d’essai est utilisé pour générer une charge de travail de serveur d’application. Une configuration à deux niveaux est utilisée avec la base de données et le serveur d’applications sur le même hôte de serveur. Cette charge de travail utilise également le temps de réponse comme une mesure de performance, ce qui diffère d’autres charges de travail testés. Par conséquent, il est utilisé pour vérifier l’impact des paramètres PPM sur la réactivité. Néanmoins, il n'est pas destinée à être représentative de toutes les charges de travail de production de sensibles à la latence.

Tous les tests d’évaluation, à l’exception SPECpower ont été conçus pour l’analyse des performances et ont été créés par conséquent, pour s’exécuter à des niveaux de charge de pointe. Toutefois, des niveaux de taille moyenne à faible charge sont plus courant pour les serveurs de production réel et sont plus intéressant pour **équilibré** planifier les optimisations. Nous exécutons intentionnellement les bancs d’essai à différents niveaux de charge à partir de 100 % à 10 % (par incréments de 10 %) à l’aide de différentes méthodes de limitation (par exemple, en réduisant le nombre d’utilisateurs/clients actifs).

### <a name="hardware-configurations"></a>Configurations matérielles

Pour chaque version de Windows, les serveurs de production plus récente sont utilisés dans le processus d’analyse et l’optimisation de plan d’alimentation. Dans certains cas, les tests ont été effectuées sur les systèmes de préproduction ceux-ci dont planification du déclenchement de la prochaine version de Windows.

Étant donné que la plupart des serveurs sont vendus avec des sockets de processeur de 1 à 4, et étant donné que les serveurs de montée en charge sont moins susceptibles d’avoir une préoccupation essentielle de l’efficacité énergétique, les tests de l’optimisation du plan d’alimentation sont principalement exécutés sur des systèmes 2 et 4 sockets. La quantité de RAM, disque et les ressources réseau pour chaque test sont choisis pour autoriser chaque système de s’exécuter jusqu'à sa capacité maximale, tout en prenant en compte les restrictions de coût qui seraient normalement en place pour les environnements de serveur du monde réel, telles que de conserver le configurations raisonnables.

>[!Important]
> Bien que le système peut exécuter sa charge de pointe, nous optimisons généralement pour les niveaux de charge inférieurs, étant donné que les serveurs qui s’exécutent de manière cohérente à leurs niveaux de charge de pointe serait bien avisés d’utiliser le **hautes performances** gestion de l’alimentation, sauf si l’énergie l’efficacité est une priorité élevée.

### <a name="metrics"></a>Mesures

Toutes ces bancs d’essai testés utilisent débit en tant que les mesures de performance. Temps de réponse est considéré comme une exigence de contrat SLA pour ces charges de travail (à l’exception de SAP, où il est une mesure principale). Par exemple, un banc d’essai est considéré comme « valide ? Si la moyenne ou le temps de réponse maximal est inférieur à une certaine valeur.

Par conséquent, le PPM également l’analyse du paramétrage utilise débit comme ses mesures de performance.  Niveau le plus élevé de charge (100 % processeur), notre objectif est que le débit ne diminuent pas plus que quelques pourcents en raison des optimisations de gestion d’alimentation. Mais la considération principale est d’optimiser l’efficacité énergétique (tel que défini ci-dessous) à des niveaux de charge moyenne et basse.

![formule de l’efficacité d’alimentation](../../media/serverperf-ppm-formula.jpg)

Les cœurs de processeur en cours d’exécution à une fréquence inférieure réduit la consommation d’énergie. Toutefois, basses fréquences de réduire le débit en général et augmentent le temps de réponse. Pour le **équilibré** gestion de l’alimentation, il existe un compromis entre intentionnelle de réactivité et efficacité énergétique. Les tests de charge de travail SAP, ainsi que le temps de réponse SLA sur les autres charges de travail, assurez-vous que l’augmentation de temps de réponse ne dépasse pas certain seuil (% 5 par exemple) pour ces charges de travail spécifiques.

>[!Note]
> Si la charge de travail utilise le temps de réponse en tant que les mesures de performance, le système doit vous passez à la **hautes performances** de l’alimentation ou de modification **équilibré** de l’alimentation comme suggéré dans [ Recommandé de paramètres de Plan d’alimentation à charge équilibrée pour le temps de réponse rapide](recommended-balanced-plan-parameters.md).

### <a name="tuning-results"></a>Résultats du paramétrage

À compter de Windows Server 2008, Microsoft a travaillé avec Intel et AMD pour optimiser les paramètres PPM pour les processeurs de serveur plus récentes pour chaque version de Windows. Un nombre considérable de combinaisons de paramètres PPM ont été testé sur chacun des charges de travail décrit précédemment pour trouver la meilleure efficacité power charge différents niveaux. En tant que logiciel algorithmes ont été affinées et sous forme d’architectures de puissance de matériel ont évolué, chaque nouveau serveur Windows toujours eu mieux ou égale efficacité énergétique à des versions précédentes sur l’ensemble des charges de travail testés.

L’illustration suivante donne un exemple de l’efficacité énergétique sous différents niveaux de charge TPC-E sur un serveur de production de 4 sockets exécutant Windows Server 2008 R2. Il montre une amélioration de 8 % aux niveaux de charge moyenne par rapport à Windows Server 2008.

![comparaison de l’efficacité d’alimentation](../../media/serverperf-ppm-figure1.jpg)

## <a name="customized-tuning-suggestions"></a>Suggestions de réglage personnalisées

Si vos caractéristiques de charge de travail principale diffèrent de façon significative les cinq charges de travail utilisés pour la valeur par défaut **équilibré** gestion de l’alimentation PPM réglage, vous pouvez expérimenter en modifiant un ou plusieurs paramètres PPM pour trouver la mieux adaptée à votre environnement.

En raison du nombre et la complexité des paramètres, cela peut être une tâche difficile, mais si vous cherchez le meilleur compromis entre l’efficacité de charge de travail et la consommation d’énergie pour votre environnement spécifique, il peut être en vaut-il la peine.

 Vous trouverez l’ensemble complet des paramètres PPM ajustables dans [processeur réglage](https://msdn.microsoft.com/windows/hardware/gg566941.aspx). Certains des paramètres d’alimentation la plus simple pour commencer est peut-être :

-   **Seuil d’augmenter la Performance processeur et le temps d’augmenter la Performance processeur** – plus grandes valeurs ralentissement la réponse de performances à l’augmentation de l’activité

-   **Seuil de diminution de performances de processeur** – grandes valeurs quicken la réponse de l’alimentation pour les périodes d’inactivité

-   **Temps de diminuer de performances de processeur** – plus grandes valeurs diminuent plus progressivement les performances pendant les périodes d’inactivité

-   **Stratégie d’augmentation des performances processeur** – la valeur « simple ? stratégie ralentit la réponse de performances à l’activité accrue et maintenue ; le « Rocket ? stratégie réagit rapidement à une activité accrue

-   **Stratégie de baisse de performances de processeur** – la valeur « simple ? stratégie plus diminue perf passe périodes d’inactivité plus ; le « Rocket ? stratégie supprime power très rapidement lors de la saisie d’une période d’inactivité

>[!Important]
> Avant de commencer les expériences, vous devez d’abord comprendre vos charges de travail, ce qui vous aideront à bien choisir de paramètre PPM et réduire les efforts de réglage.

### <a name="understand-high-level-performance-and-power-requirements"></a>Comprendre les performances de haut niveau et de consommation

Si votre charge de travail est « en temps réel ? (par exemple, vulnérable aux problèmes ou autres visible par l’utilisateur a un impact sur) ou d’exigence réactivité très précise (par exemple, courtage), et si la consommation d’énergie n’est pas un critère principal pour votre environnement, vous devriez probablement simplement passer à la **Hautes performances** de l’alimentation. Sinon, vous devez comprendre les exigences de temps de réponse de vos charges de travail et ensuite le paramétrer les paramètres PPM pour une meilleure efficacité possibles de l’alimentation qui toujours répond à ces exigences.

### <a name="understand-underlying-workload-characteristics"></a>Comprendre les caractéristiques de charge de travail sous-jacent

Vous devez connaître vos charges de travail et les jeux de paramètres d’expérience pour le paramétrage de la conception. Par exemple, si les fréquences des cœurs de processeur doivent être très dès fast (vous avez peut-être une charge de travail en rafale avec des périodes d’inactivité importantes, mais vous avez besoin de réactivité très rapide lorsqu’une nouvelle transaction arrive), puis les performances de processeur augmenter la stratégie peut-être être définie sur « rocket ? (qui, comme son nom l’indique, enverra la fréquence de cœur de processeur à sa valeur maximale et non pas intéressées sur une période de temps).

Si votre charge de travail est très en rafale, l’intervalle de vérification PPM peut être réduite pour rendre la fréquence du processeur de démarrer l’exécution pas à pas plus tôt après l’arrivée d’une rafale. Si votre charge de travail n’a pas d’accès concurrentiel de threads élevé, puis l’immobilisation de cœur peut être activée pour forcer la charge de travail à exécuter sur un plus petit nombre de cœurs, ce qui pourrait également potentiellement améliorer les taux d’accès au cache de processeur.

Si vous souhaitez simplement augmenter la fréquence du processeur sur les niveaux d’utilisation moyenne (par exemple, les niveaux de charge de travail légère pas), les seuils d’augmenter/diminuer la performance processeur peuvent être ajustées pour ne réagir pas jusqu'à ce que certains niveaux d’activité est observées.

### <a name="understand-periodic-behaviors"></a>Comprendre les comportements périodiques

Il peut y avoir des exigences de performances différentes pour la journée et nuit ou via les week-ends, ou il peut y avoir différentes charges de travail qui s’exécutent à des moments différents. Dans ce cas, un ensemble de paramètres PPM peut-être pas optimal pour toutes les périodes de temps. Étant donné que plusieurs modes d’alimentation personnalisés peuvent être prévues, il est possible même paramétrer pour différentes périodes et basculer entre les modes d’alimentation via des scripts ou d’autres moyens de configuration du système dynamique.

Là encore, cela accroît la complexité du processus d’optimisation, par conséquent, il est une question d’évaluer la valeur sera être acquise à partir de ce type de réglage, qui doivent probablement être répétées lorsqu’il existe des mises à niveau matérielles importantes ou des modifications de la charge de travail.

C’est pourquoi Windows fournit un **équilibré** gestion de l’alimentation en premier lieu, car dans de nombreux cas il vaut sans doute pas la peine de réglage disponible pour une charge de travail spécifique sur un serveur spécifique.

## <a name="see-also"></a>Voir aussi
- [Les considérations sur les performances du matériel serveur](../index.md)
- [Considérations sur le serveur matériel Power](../power.md)
- [Alimentation et réglage des performances](power-performance-tuning.md)
- [Paramétrage de la gestion de l’alimentation de processeur](processor-power-management-tuning.md)
- [Recommandé des paramètres de Plan à charge équilibrée](recommended-balanced-plan-parameters.md)
