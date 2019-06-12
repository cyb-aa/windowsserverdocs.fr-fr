---
title: Configuration d’Hyper-V
description: Considérations relatives à la configuration d’Hyper-V pour le réglage des performances
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: baea091482818c581414ba1d9c1c01db2a52e3d7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435667"
---
# <a name="hyper-v-configuration"></a>Configuration d’Hyper-V

## <a name="hardware-selection"></a>Sélection du matériel

Les considérations matérielles pour les serveurs exécutant Hyper-V généralement ressemblent à celles des serveurs non virtualisé, mais les serveurs exécutant Hyper-V peuvent présenter une utilisation accrue du processeur consommer davantage de mémoire et avez besoin de la bande passante d’e/s en raison de la consolidation des serveurs.

-   **Processeurs**

    Hyper-V dans Windows Server 2016 présente les processeurs logiques sous la forme d’un ou plusieurs processeurs virtuels à chaque machine virtuelle active. Hyper-V nécessite désormais les processeurs qui prennent en charge les technologies SLAT Second Level Address Translation () comme Extended Page Tables (EPT) ou Nested Page Tables (NPT).

-   **Cache**

    Hyper-V peut tirer parti des caches processeur supérieure, en particulier pour les charges qui ont un grand travail définie en mémoire et dans les configurations d’ordinateur virtuel dans lequel le taux de processeurs virtuels avec des processeurs logiques est élevé.

-   **Mémoire**

    Le serveur physique requiert suffisamment de mémoire pour les partitions de la racine et l’enfant. La partition racine requiert de mémoire pour effectuer efficacement les e/s pour le compte d’ordinateurs virtuels et des opérations telles que d’un instantané de machine virtuelle. Hyper-V permet de s’assurer que suffisamment de mémoire est disponible pour la partition racine et permet la quantité restante de mémoire à affecter aux partitions enfants. Partitions enfants doivent être dimensionnées en fonction des besoins de la charge prévue pour chaque machine virtuelle.

-   **Stockage**

    Le matériel de stockage doit avoir suffisamment de bande passante d’e/s et la capacité à répondre aux besoins actuels et futurs du serveur virtuel d’ordinateurs qui les hôtes de serveur physique. Considérez les spécifications lorsque vous sélectionnez des contrôleurs de stockage et les disques et que vous choisissez la configuration RAID. Placer des machines virtuelles avec des charges de travail sollicitant fortement le disque sur différents disques physiques est probablement améliorer les performances globales. Par exemple, si quatre machines virtuelles partagent un seul disque et l’utilisez activement, chaque machine virtuelle peut générer que 25 % de la bande passante de ce disque.

## <a name="power-plan-considerations"></a>Considérations concernant le plan d’alimentation

Comme une technologie de base, la virtualisation est un outil puissant utile en augmentant la densité de la charge de travail de serveur, réduisant le nombre de serveurs physiques requis dans votre centre de données, augmentant l’efficacité opérationnelle et en réduisant la consommation d’énergie. Gestion de l’alimentation est essentielle pour la gestion des coûts. 

Dans un environnement de centre de données idéal, la consommation d’énergie est gérée par la consolidation de travail sur les ordinateurs jusqu'à ce qu’elles sont principalement occupés et arrêtez puis machines inactives. Si cette approche n’est pas pratique, les administrateurs peuvent tirer parti de l’alimentation sur les hôtes physiques pour vous assurer qu’ils ne consomment pas plus de puissance que nécessaire. 

Techniques de gestion d’alimentation de serveur sont fournis avec un coût, en particulier en tant que client les charges de travail ne sont pas approuvés afin de dicter la stratégie sur l’infrastructure physique de l’hébergeur. Le logiciel de la couche hôte reste à déduire comment optimiser le débit tout en réduisant la consommation d’énergie. Dans les machines principalement inactif, cela peut entraîner l’infrastructure physique de conclure que le dessin power modérée est approprié, ce qui entraîne des charges de travail de locataire individuel exécutées plus lentement qu’ils peuvent le cas contraire.

Windows Server utilise la virtualisation dans un large éventail de scénarios. À partir d’un serveur IIS peu chargé vers un serveur SQL modérément occupé, à un hôte de nuage avec Hyper-V exécutant des centaines de machines virtuelles par serveur. Chacun de ces scénarios peut avoir des exigences uniques de matériel, logiciels et de performances. Par défaut, Windows Server utilise et recommande la **équilibré** en fonction de gestion de l’alimentation qui permet d’économie d’énergie par la mise à l’échelle des performances du processeur sur l’utilisation du processeur.

Avec le **équilibré** gestion de l’alimentation, les États d’alimentation le plus élevés (et des latences de réponse le plus bas dans les charges de travail clientes) sont appliqués uniquement lorsque l’hôte physique est relativement occupé. Si vous privilégiez la réponse déterministe et à faible latence pour toutes les charges de travail de locataire, vous devez envisager de passer de la valeur par défaut **équilibré** de l’alimentation pour le **hautes performances** de l’alimentation. Le **hautes performances** de l’alimentation exécute les processeurs à pleine vitesse tout le temps, en désactivant efficacement en fonction de la demande de basculement, ainsi que d’autres techniques de gestion d’alimentation et optimiser les performances sur les économies d’énergie.

Pour les clients, ce qui conviennent à la réduction des coûts à partir de la réduction du nombre de serveurs physiques et souhaitez garantir qu’ils atteignent des performances maximales pour leurs charges de travail virtualisées, vous devez envisager d’utiliser le **hautes performances** gestion de l’alimentation.

Pour des recommandations supplémentaires et des informations sur l’utilisation des modes d’alimentation d’optimiser votre infrastructure, lisez [recommandé à charge équilibrée Power planifier les paramètres pour les temps de réponse rapide](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>Option d’installation minimale

Fonctionnalité de Windows Server 2016 l’option d’installation Server Core. Server Core offre un environnement minimal pour l’hébergement d’un ensemble de rôles de serveur, notamment Hyper-V. Il comprend un encombrement de disque pour l’hôte du système d’exploitation et une attaque plus petits et maintenance de surface. Par conséquent, nous recommandons vivement que les serveurs de virtualisation Hyper-V utilisent l’option d’installation Server Core.

Une installation Server Core offre une fenêtre de console uniquement lorsque l’utilisateur est connecté, mais Hyper-V expose les fonctionnalités de gestion à distance, y compris [Windows Powershell](https://technet.microsoft.com/library/hh848559.aspx) pour les administrateurs peuvent gérer à distance.

## <a name="dedicated-server-role"></a>Rôle de serveur dédié

La partition racine doit être dédiée à Hyper-V. Rôles serveur supplémentaires en cours d’exécution sur un serveur exécutant Hyper-V peut nuire aux performances du serveur de virtualisation, en particulier si elles consomment beaucoup de bande passante du processeur, la mémoire ou d’e/s. En réduisant les rôles de serveur dans la partition racine a des avantages supplémentaires tels que la réduction de la surface d’attaque.

Les administrateurs système doivent bien réfléchir à quel logiciel est installé dans la partition racine, car certains logiciels peuvent nuire aux performances globales du serveur exécutant Hyper-V.

## <a name="guest-operating-systems"></a>Systèmes d’exploitation invités

Hyper-V prend en charge et a été paramétré pour un nombre de systèmes d’exploitation invités différents. Le nombre de processeurs virtuels qui sont pris en charge par invité varie selon le système d’exploitation invité. Pour obtenir la liste des systèmes d’exploitation invités pris en charge, consultez [vue d’ensemble d’Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="cpu-statistics"></a>Statistiques du processeur

Hyper-V publie les compteurs de performances pour aider à définir le comportement du serveur de virtualisation et de signaler l’utilisation des ressources. Le jeu d’outils pour afficher les compteurs de performances dans Windows standard inclut l’Analyseur de performances et Logman.exe, qui peuvent afficher et connecter les compteurs de performances d’Hyper-V. Les noms des objets compteurs appropriés sont précédés **Hyper-V**.

Vous devriez toujours mesurer l’utilisation du processeur du système physique en utilisant les compteurs de performances de processeur logique de l’hyperviseur Hyper-V. Ce rapport de Gestionnaire des tâches et l’Analyseur de performances dans la racine des compteurs de l’utilisation du processeur et les partitions enfants ne reflètent pas l’utilisation du processeur physique réelle. Pour surveiller les performances, utilisez les compteurs de performances suivants :

- **Processeur logique de l’hyperviseur Hyper-V (\*)\\% temps d’exécution Total** le temps d’activité total des processeurs logiques

- **Processeur logique de l’hyperviseur Hyper-V (\*)\\% durée d’exécution invité** le temps passé à cycles en cours d’exécution au sein d’un invité ou de l’hôte

- **Processeur logique de l’hyperviseur Hyper-V (\*)\\% durée d’exécution hyperviseur** le temps passé en cours d’exécution au sein de l’hyperviseur

- **Processeur virtuel racine de l’hyperviseur Hyper-V (\*)\\\\** * mesure l’utilisation du processeur de la partition racine

- **Processeur virtuel de l’hyperviseur Hyper-V (\*)\\\\** * mesure l’utilisation du processeur de partitions de l’invité


## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Performances du processeur Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Performances E/S du stockage Hyper-V](storage-io-performance.md)

-   [Performances E/S du réseau Hyper-V](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Ordinateurs virtuels Linux](linux-virtual-machine-considerations.md)
