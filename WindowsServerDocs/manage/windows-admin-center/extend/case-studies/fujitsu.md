---
title: Étude de cas du kit de développement logiciel du centre d’administration Windows-Fujitsu
description: Étude de cas du kit de développement logiciel du centre d’administration Windows-Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9acfa873e4ce7d3e91a23abff726836f0e11ce59
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357218"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Extensions Fujitsu ServerView Health et RAID

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Intégration de la visibilité de bout en bout, du système d’exploitation au matériel, au centre d’administration Windows

Fujitsu est une société de technologie de communication et d’information japonaise de pointe, ainsi qu’un fabricant de produits [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) et [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) Server. [Fujitsu Serverview Management Suite](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) fournit un ensemble d’outils complet pour la gestion du cycle de vie du serveur, y compris un agent côté serveur qui fournit une interface CIM et PowerShell pour la gestion du matériel.

Fujitsu a vu une opportunité de s’intégrer facilement au centre d’administration Windows, car elle offrait des interfaces CIM et PowerShell qui pouvaient communiquer avec les agents côté serveur. L’équipe de développement de Fujitsu était en mesure de mettre en œuvre facilement les appels CIM qu’elle utilisait avec l’agent et de visualiser les informations dans le centre d’administration Windows à l’aide des composants d’interface utilisateur disponibles.

![Extension Fujitsu-vue arborescence d’intégrité](../../media/extend-case-study-fujitsu/health-tree.png)

Une fois que l’équipe s’est familiarisée avec le kit de développement logiciel (SDK) du centre d’administration Windows, l’ajout d’une interface utilisateur pour exposer des informations matérielles supplémentaires était souvent un petit nombre de lignes de code HTML et peut être rapidement développé à partir d’un seul outil pour afficher une vue récapitulative du composant matériel. intégrité, affichages détaillés pour les journaux des événements système, moniteur de pilote, vues distinctes pour le processeur, la mémoire, les ventilateurs, alimentations, températures et tensions, et même un outil supplémentaire pour la gestion RAID. L’utilisation de contrôles d’interface utilisateur disponibles dans le kit de développement logiciel (SDK), tels que les contrôles d’arborescence, de grille et de volet d’informations, permettait à l’équipe de générer rapidement une interface utilisateur et d’obtenir une conception visuelle et d’interaction très similaire au reste du centre d’administration Windows.

![Extension Fujitsu-vue de l’arborescence RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Extension Fujitsu-vue volumes RAID](../../media/extend-case-study-fujitsu/raid-volumes.png)

Le partenariat entre Fujitsu et l’équipe du centre d’administration Windows montre clairement la valeur de l’intégration au sein du centre d’administration Windows, ce qui permet aux clients d’obtenir des informations de bout en bout sur les rôles de serveur et les services, sur le système d’exploitation et sur la gestion du matériel. .