---
title: Étude de cas de kit de développement logiciel Windows Admin Center - Fujitsu
description: Étude de cas de kit de développement logiciel Windows Admin Center - Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814990"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Extensions de contrôle d’intégrité de Fujitsu ServerView et RAID

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Assure une visibilité de bout en bout, à partir du système d’exploitation sur un matériel, Windows Admin Center

Fujitsu est une entreprise de technologie d’informations et de communication japonais leader et le fabricant de [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) et [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) produits serveur. Le [suite de gestion Fujitsu ServerView](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) fournit un ensemble complet d’outils pour le serveur de gestion du cycle de vie, y compris un agent côté serveur qui fournit une interface CIM et PowerShell pour la gestion du matériel.

Fujitsu vu l’occasion d’intégrer facilement à Windows Admin Center telles qu’elles fournies interfaces CIM et PowerShell qui peuvent communiquer avec les agents côté serveur. L’équipe de développement de Fujitsu a pu facilement implémenter les appels CIM qu'ils étaient habitués à l’agent et visualiser les informations au sein de Windows Admin Center à l’aide de composants de l’interface utilisateur disponibles.

![Extension de Fujitsu - vue d’arborescence de contrôle d’intégrité](../../media/extend-case-study-fujitsu/health-tree.png)

Une fois que l’équipe s’est familiarisée avec le Kit de développement logiciel Windows Admin Center, l’ajout de l’interface utilisateur pour exposer des informations de matériel supplémentaire était souvent simplement quelques lignes de code HTML et ils pouvaient rapidement à développer à partir d’un seul outil à l’affichage d’une vue récapitulative de composant matériel contrôle d’intégrité, des vues détaillées pour les journaux des événements système, le moniteur de pilote, séparez les vues de processeur, mémoire, ventilateurs, blocs d’alimentation, températures et tensions et même un outil supplémentaire pour la gestion de RAID. À l’aide de contrôles d’interface utilisateur disponibles dans le Kit de développement telles que l’arborescence, les contrôles de volet Grille et de détail activée l’équipe à créer rapidement de l’interface utilisateur et de bénéficier d’une conception visuel et interaction très similaire à celui de Windows Admin Center.

![Extension de Fujitsu - arborescence RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Extension de Fujitsu - afficher les volumes RAID](../../media/extend-case-study-fujitsu/raid-volumes.png)

Le partenariat entre Fujitsu et de l’équipe Windows Admin Center montre clairement la valeur de l’intégration dans Windows Admin Center, permettant aux clients d’avoir un aperçu de bout en bout dans des rôles de serveur et des services, le système d’exploitation et gestion du matériel .