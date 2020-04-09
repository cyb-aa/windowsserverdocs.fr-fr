---
title: Considérations relatives à l’alimentation matérielle du serveur
description: Considérations relatives à l’alimentation matérielle du serveur
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: qizha;tristanb
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 865899e5f33bde97dff97efaff6010b95aafd3e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851982"
---
# <a name="server-hardware-power-considerations"></a>Considérations relatives à l’alimentation matérielle du serveur

Il est important de reconnaître l’importance croissante de l’efficacité énergétique dans les environnements d’entreprise et de centre de données. Les performances élevées et l’utilisation faible de l’énergie sont souvent des objectifs conflictuels, mais en sélectionnant soigneusement les composants serveur, vous pouvez obtenir un équilibre correct entre eux. Les sections suivantes répertorient les indications relatives aux caractéristiques de l’alimentation et aux fonctionnalités des composants matériels du serveur.

## <a name="processor-recommendations"></a>Recommandations concernant le processeur

La fréquence, la tension de fonctionnement, la taille du cache et la technologie de processus affectent la consommation d’énergie des processeurs. Les processeurs ont une évaluation TDP (Thermal Design point) qui donne une indication de base de la consommation d’énergie par rapport à d’autres modèles.

En général, optez pour le processeur TDP le plus bas qui répondra à vos objectifs en matière de performances. En outre, les générations plus récentes de processeurs sont généralement plus efficaces et peuvent exposer davantage d’États d’alimentation pour les algorithmes de gestion de l’alimentation Windows, ce qui permet une meilleure gestion de l’alimentation à tous les niveaux de performances. Ils peuvent également utiliser certaines des nouvelles techniques de gestion de l’alimentation « coopérative » que Microsoft a développées en partenariat avec les fabricants de matériel.

Pour plus d’informations sur les techniques de gestion de l’alimentation coopérative, consultez la section contrôle des performances du processeur collaboratif dans la [spécification Advanced Configuration and Power Interface](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf).


## <a name="memory-recommendations"></a>Recommandations de mémoire
Comptes mémoire pour une fraction plus grande de la puissance totale du système. De nombreux facteurs affectent la consommation d’énergie d’un module de mémoire DIMM, telle que la technologie de mémoire, le code de correction d’erreur (ECC), la fréquence de bus, la capacité, la densité et le nombre de rangs. Par conséquent, il est préférable de comparer les évaluations de puissance attendues avant d’acheter de grandes quantités de mémoire.

La mémoire insuffisante est désormais disponible, mais vous devez prendre en compte les performances et les compromis des coûts. Si votre serveur sera paginé, vous devez également prendre en compte le coût énergétique des disques de pagination.


## <a name="disks-recommendations"></a>Recommandations relatives aux disques
Un RPM plus élevé signifie une consommation énergétique accrue. Les lecteurs SSD sont plus efficaces que les lecteurs de rotation. En outre, les lecteurs 2,5 pouces nécessitent généralement moins d’énergie que les lecteurs de 3,5 pouces.

## <a name="network-and-storage-adapter-recommendations"></a>Recommandations concernant les adaptateurs réseau et du stockage
Certains adaptateurs réduisent la consommation d’énergie pendant les périodes d’inactivité. Il s’agit d’un point important à prendre en compte pour les cartes réseau de 10 Go et les liens de stockage à bande passante élevée (4-8 Go). Ces appareils peuvent consommer de grandes quantités d’énergie.


## <a name="power-supply-recommendations"></a>Recommandations relatives à l’alimentation
L’amélioration de l’efficacité de l’alimentation est un excellent moyen de réduire la consommation d’énergie sans affecter les performances. Les alimentations à haut rendement peuvent économiser plusieurs kilowatts/heures par an, par serveur.


## <a name="fan-recommendations"></a>Recommandations relatives aux ventilateurs
Les ventilateurs, comme les alimentations, sont une zone où vous pouvez réduire la consommation d’énergie sans affecter les performances du système. Les ventilateurs à vitesse variable peuvent réduire les RPM lorsque la charge du système diminue, éliminant ainsi toute consommation d’énergie inutile.


## <a name="usb-devices-recommendations"></a>Recommandations relatives aux périphériques USB
Windows Server 2016 permet la suspension sélective des périphériques USB par défaut. Toutefois, un pilote de périphérique mal écrit peut toujours perturber l’efficacité énergétique du système par une marge considérable. Pour éviter les problèmes potentiels, déconnectez les périphériques USB, désactivez-les dans le BIOS ou choisissez des serveurs qui ne nécessitent pas de périphériques USB.


## <a name="remotely-managed-power-strip-recommendations"></a>Recommandations de Power Strip gérées à distance
Les barrettes d’alimentation ne font pas partie intégrante du matériel serveur, mais elles peuvent faire une grande différence dans le centre de données. Les mesures montrent que les serveurs de volume qui sont branchés, mais qui ont été soi-disants hors tension, peuvent toujours nécessiter jusqu’à 30 watts de puissance.

Pour éviter de gaspiller de l’électricité, vous pouvez déployer une bande d’alimentation gérée à distance pour chaque rack de serveurs afin de déconnecter par programmation l’alimentation de serveurs spécifiques.

## <a name="processor-terminology"></a>terminologie du processeur
La terminologie du processeur utilisée dans cette rubrique reflète la hiérarchie des composants disponibles dans la figure suivante. Les termes utilisés du plus grand au plus petit niveau de granularité des composants sont les suivants :

-   Socket de processeur
-   nœud NUMA
-   Standard
-   Processeur logique

![terminologie du processeur](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>Voir aussi
- [Considérations relatives aux performances matérielles du serveur](index.md)
- [Réglage de la puissance et des performances](power/power-performance-tuning.md)
- [Réglage de la gestion de la puissance du processeur](power/processor-power-management-tuning.md)
- [Paramètres de planification équilibrée recommandés](power/recommended-balanced-plan-parameters.md)
