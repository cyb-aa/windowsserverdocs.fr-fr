---
title: Considérations sur le serveur matériel Power
description: Considérations sur le serveur matériel Power
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5261c856a0a29f9f58526e4f9580a16bbed5be56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874300"
---
# <a name="server-hardware-power-considerations"></a>Considérations sur le serveur matériel Power

Il est important de reconnaître l’importance croissante de l’efficacité énergétique dans les environnements de centres de données et entreprise. Performances élevées et faible énergie utilisation sont souvent des objectifs contradictoires, mais en sélectionnant soigneusement les composants serveur, vous pouvez obtenir l’équilibre correct entre eux. Les sections suivantes répertorient les instructions pour les fonctionnalités des composants matériels de serveur et les caractéristiques de l’alimentation.

## <a name="processor-recommendations"></a>Recommandations de processeur

Fréquence, la tension de fonctionnement, taille du cache et technologie de traitement affecte la consommation d’énergie de processeurs. Les processeurs ont une conception thermique de point de contrôle d’accès (TDP) qui donne une indication de base de consommation d’énergie par rapport à d’autres modèles.

En règle générale, opter pour le processeur TDP la plus basse qui répond à vos objectifs de performances. En outre, des générations plus récente de processeurs sont généralement plus efficace d’énergie, et ils risquent d’exposer plusieurs États d’alimentation pour les algorithmes de gestion d’alimentation de Windows, ce qui permet une meilleure gestion de l’alimentation à tous les niveaux de performances. Ou ils peuvent utiliser certains de la nouvelle « coopérative ? techniques de gestion d’alimentation développés par Microsoft en partenariat avec les fabricants de matériel.

Pour plus d’informations sur les techniques de gestion d’alimentation coopérative, consultez la section nommée Collaborative contrôle de performances de processeur dans le [Advanced Configuration and Power Interface Specification](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf).


## <a name="memory-recommendations"></a>Recommandations de mémoire
Comptes de la mémoire pour une fraction d’augmentation de la puissance totale du système. De nombreux facteurs affectent la consommation d’énergie d’une mémoire DIMM, telles que la technologie de mémoire, code de correction d’erreur (ECC), fréquence de bus, capacité, densité et nombre de rangs. Par conséquent, il est préférable de comparer des évaluations power attendu avant d’acheter de grandes quantités de mémoire.

Mémoire de faible puissance est désormais disponible, mais vous devez prendre en compte les performances et coût compromis. Si votre serveur sera être la pagination, vous devez également tenir compte le coût des disques de la pagination.


## <a name="disks-recommendations"></a>Recommandations de disques
VITESSE est élevée, la consommation d’énergie accrue. Lecteurs SSD sont plus efficaces d’énergie que les disques de rotation. En outre, les lecteurs de 2,5 pouces nécessitent généralement moins d’énergie que les disques de 3,5 pouces.

## <a name="network-and-storage-adapter-recommendations"></a>Réseau et des recommandations de carte de stockage
Certains adaptateurs de diminuer la consommation d’énergie pendant les périodes d’inactivité. Il s’agit d’une considération importante pour les cartes réseau de 10 Go et les liens de stockage de bande passante élevée (du 4 au 8 Go). Ces appareils peuvent consommer de grandes quantités d’énergie.


## <a name="power-supply-recommendations"></a>Recommandations de fourniture de Power
Amélioration de l’efficacité de puissance électrique est un excellent moyen de réduire la consommation d’énergie sans affecter les performances. Alimentations haute efficacité peuvent enregistrer plusieurs mégawattheures par an et par serveur.


## <a name="fan-recommendations"></a>Recommandations de ventilateur
Ventilateurs, telles que des alimentations, correspondent à une zone où vous pouvez réduire la consommation d’énergie sans affecter les performances du système. Les fans de vitesse variable peuvent réduire tr/min en tant que la diminution de charge système, en éliminant la consommation d’énergie inutiles dans le cas contraire.


## <a name="usb-devices-recommendations"></a>Périphériques USB recommandations
Windows Server 2016 Active la suspension sélective pour les périphériques USB par défaut. Toutefois, un pilote de périphérique mal écrite peut interrompre toujours l’efficacité énergétique du système par une marge importante. Pour éviter les problèmes potentiels, déconnectez les périphériques USB, les désactiver dans le BIOS ou choisir les serveurs qui ne nécessitent pas de périphériques USB.


## <a name="remotely-managed-power-strip-recommendations"></a>Recommandations de bande Power gérés à distance
Barrettes d’alimentation ne sont pas partie intégrante du matériel de serveur, mais ils peuvent faire une grande différence dans le centre de données. Mesures font apparaître que les serveurs de volume qui sont connectés, mais qui ont été ostensiblement hors tension, peuvent toujours nécessiter jusqu'à 30 watts de puissance.

Pour éviter de gaspiller d’électricité, vous pouvez déployer un onduleur gérés à distance pour chaque rack de serveurs par programmation déconnexion d’alimentation à partir des serveurs spécifiques.

## <a name="processor-terminology"></a>Terminologie de processeur
La terminologie de processeur utilisée tout au long de cette rubrique reflète la hiérarchie des composants disponibles dans la figure suivante. Termes utilisés du plus grand au plus petit niveau de granularité des composants sont les suivants :

-   Socket de processeur
-   nœud NUMA
-   Standard
-   processeur logique

![terminologie de processeur](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>Voir aussi
- [Les considérations sur les performances du matériel serveur](index.md)
- [Alimentation et réglage des performances](power/power-performance-tuning.md)
- [Paramétrage de la gestion de l’alimentation de processeur](power/processor-power-management-tuning.md)
- [Recommandé des paramètres de Plan à charge équilibrée](power/recommended-balanced-plan-parameters.md)
