---
title: Configuration d’Hyper-V
description: Considérations relatives à la configuration d’Hyper-V pour le réglage des performances
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e3c4fa32b97761ad05c88722ef090f96fff21cf3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851792"
---
# <a name="hyper-v-configuration"></a>Configuration d’Hyper-V

## <a name="hardware-selection"></a>Sélection du matériel

Les considérations matérielles pour les serveurs exécutant Hyper-V sont généralement similaires à celles des serveurs non virtualisés, mais les serveurs exécutant Hyper-V peuvent présenter une utilisation accrue du processeur, consommer plus de mémoire et nécessiter une bande passante d’e/s supérieure en raison de la consolidation des serveurs.

-   **Processeurs**

    Hyper-V dans Windows Server 2016 présente les processeurs logiques sous la forme d’un ou de plusieurs processeurs virtuels à chaque machine virtuelle active. Hyper-V nécessite désormais des processeurs prenant en charge les technologies de traduction d’adresses de second niveau (SLAT), telles que les tables de pages étendues (EPT) ou les tables de pages imbriquées (NPT).

-   **En**

    Hyper-V peut tirer parti de caches de processeur plus volumineux, en particulier pour les charges qui ont une plage de travail importante en mémoire et dans les configurations d’ordinateur virtuel dans lesquelles le rapport entre les processeurs virtuels et les processeurs logiques est élevé.

-   **Mémoire**

    Le serveur physique requiert suffisamment de mémoire pour les partitions racines et enfants. La partition racine requiert de la mémoire pour exécuter efficacement des e/s pour le compte des machines virtuelles et des opérations telles qu’une capture instantanée d’ordinateur virtuel. Hyper-V garantit que suffisamment de mémoire est disponible pour la partition racine et autorise l’affectation de la mémoire restante aux partitions enfants. Les partitions enfants doivent être dimensionnées en fonction des besoins de la charge prévue pour chaque machine virtuelle.

-   **Stockage**

    Le matériel de stockage doit disposer d’une bande passante d’e/s et d’une capacité suffisantes pour répondre aux besoins actuels et futurs des machines virtuelles que le serveur physique héberge. Tenez compte de ces exigences lorsque vous sélectionnez des contrôleurs de stockage et des disques, puis choisissez la configuration RAID. Le fait de placer des machines virtuelles avec des charges de travail nécessitant beaucoup de ressources disque sur différents disques physiques améliore probablement les performances globales. Par exemple, si quatre machines virtuelles partagent un seul disque et l’utilisent activement, chaque machine virtuelle ne peut obtenir que 25% de la bande passante de ce disque.

## <a name="power-plan-considerations"></a>Considérations relatives au mode de gestion de l’alimentation

En tant que technologie de base, la virtualisation est un outil puissant qui permet d’accroître la densité des charges de travail du serveur, ce qui réduit le nombre de serveurs physiques requis dans votre centre de vos centres de développement, ce qui améliore l’efficacité opérationnelle et réduit les coûts de consommation énergétique. La gestion de l’alimentation est essentielle pour la gestion des coûts. 

Dans un environnement de centre de centres idéal, la consommation d’énergie est gérée en consolidant le travail sur les machines jusqu’à ce qu’elles soient principalement occupées, puis en désactivant les ordinateurs inactifs. Si cette approche n’est pas pratique, les administrateurs peuvent tirer parti des modes de gestion de l’alimentation sur les hôtes physiques pour s’assurer qu’ils ne consomment pas plus de puissance que nécessaire. 

Les techniques de gestion de l’alimentation des serveurs ont un coût, en particulier lorsque les charges de travail des locataires ne sont pas approuvées pour dicter une stratégie sur l’infrastructure physique de l’hébergeur. Le logiciel de la couche hôte est laissé déduire comment maximiser le débit tout en minimisant la consommation d’énergie. Dans le cas d’ordinateurs peu inactifs, cela peut entraîner l’infrastructure physique à conclure qu’une alimentation modérée est appropriée, entraînant une exécution plus lente des charges de travail des locataires.

Windows Server utilise la virtualisation dans un large éventail de scénarios. D’un serveur IIS léger chargé à un SQL Server modérément occupé, sur un hôte Cloud avec Hyper-V exécutant des centaines de machines virtuelles par serveur. Chacun de ces scénarios peut avoir des exigences spécifiques en matière de matériel, de logiciels et de performances. Par défaut, Windows Server utilise et recommande le mode de gestion de l’alimentation **équilibré** qui permet la économie d’énergie en mettant à l’échelle les performances du processeur en fonction de l’utilisation du processeur.

Avec le mode de gestion de l’alimentation **équilibré** , les États d’alimentation les plus élevés (et les latences de réponse les plus faibles dans les charges de travail des locataires) sont appliqués uniquement lorsque l’hôte physique est relativement occupé. Si vous définissez la valeur déterministe, réponse à faible latence pour toutes les charges de travail client, vous devez envisager de passer du mode de gestion de l’alimentation **équilibré** par défaut au mode de gestion de l’alimentation à **hautes performances** . Le mode de gestion de l’alimentation **haute performance** exécutera les processeurs à pleine vitesse tout le temps, en désactivant efficacement la commutation basée sur la demande, avec d’autres techniques de gestion de l’alimentation et en optimisant les performances par rapport à l’économie d’énergie.

Pour les clients, qui sont satisfaites des économies de coût de la réduction du nombre de serveurs physiques et veulent s’assurer qu’ils atteignent des performances maximales pour leurs charges de travail virtualisées, vous devez envisager d’utiliser le mode de gestion de l’alimentation à **hautes performances** .

Pour obtenir des recommandations supplémentaires et des informations sur l’utilisation des modes de gestion de l’alimentation pour optimiser votre infrastructure, consultez [paramètres du mode de gestion de l’alimentation équilibré recommandés pour les temps de réponse rapides](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>Option d’installation minimale

Windows Server 2016 est doté de l’option d’installation Server Core. Server Core offre un environnement minimal pour l’hébergement d’un ensemble sélectionné de rôles de serveur, y compris Hyper-V. Il offre un encombrement de disque plus faible pour le système d’exploitation hôte et une surface d’attaque et de maintenance plus réduite. Par conséquent, nous recommandons vivement que les serveurs de virtualisation Hyper-V utilisent l’option d’installation Server Core.

Une installation Server Core n’offre une fenêtre de console que lorsque l’utilisateur a ouvert une session, mais Hyper-V expose les fonctionnalités de gestion à distance, y compris [Windows PowerShell](https://technet.microsoft.com/library/hh848559.aspx) , afin que les administrateurs puissent la gérer à distance.

## <a name="dedicated-server-role"></a>Rôle de serveur dédié

La partition racine doit être dédiée à Hyper-V. L’exécution de rôles serveur supplémentaires sur un serveur exécutant Hyper-V peut nuire aux performances du serveur de virtualisation, en particulier s’ils consomment beaucoup d’UC, de mémoire ou de bande passante d’e/s. La réduction des rôles de serveur dans la partition racine présente des avantages supplémentaires, tels que la réduction de la surface d’attaque.

Les administrateurs système doivent prendre en compte soigneusement les logiciels installés dans la partition racine, car certains logiciels peuvent nuire aux performances globales du serveur exécutant Hyper-V.

## <a name="guest-operating-systems"></a>Systèmes d’exploitation invités

Hyper-V prend en charge et a été réglé pour un certain nombre de systèmes d’exploitation invités différents. Le nombre de processeurs virtuels pris en charge par l’invité dépend du système d’exploitation invité. Pour obtenir la liste des systèmes d’exploitation invités pris en charge, consultez [vue d’ensemble d’Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="cpu-statistics"></a>Statistiques de l’UC

Hyper-V publie des compteurs de performances pour faciliter la caractérisation du comportement du serveur de virtualisation et signaler l’utilisation des ressources. L’ensemble standard d’outils d’affichage des compteurs de performance dans Windows comprend l’analyseur de performances et logman. exe, qui peuvent afficher et enregistrer les compteurs de performance Hyper-V. Les noms des objets de compteur pertinents sont préfixés par **Hyper-V**.

Vous devez toujours mesurer l’utilisation du processeur par le système physique à l’aide des compteurs de performance du processeur logique de l’hyperviseur Hyper-V. Les compteurs d’utilisation de l’UC que le gestionnaire des tâches et le rapport de l’analyseur de performances dans les partitions racines et enfants ne reflètent pas l’utilisation réelle de l’UC physique. Utilisez les compteurs de performances suivants pour surveiller les performances :

- **Processeur logique de l’hyperviseur Hyper-V (\*)\\% de la durée d’exécution totale** Durée totale d’inactivité des processeurs logiques

- **Processeur logique de l’hyperviseur Hyper-V (\*)\\% du temps d’exécution invité** Temps passé à exécuter des cycles au sein d’un invité ou de l’hôte

- **Processeur logique de l’hyperviseur Hyper-V (\*)\\% de la durée d’exécution de l’hyperviseur** Temps passé à s’exécuter au sein de l’hyperviseur

- Le **processeur virtuel racine de l’hyperviseur Hyper-V (\*)\\\\** * mesure l’utilisation du processeur de la partition racine

- **Processeur virtuel de l’hyperviseur Hyper-V (\*)\\\\** * mesure l’utilisation du processeur des partitions invitées


## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Performances du processeur Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Performances E/S du stockage Hyper-V](storage-io-performance.md)

-   [Performances E/S du réseau Hyper-V](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Ordinateurs virtuels Linux](linux-virtual-machine-considerations.md)
