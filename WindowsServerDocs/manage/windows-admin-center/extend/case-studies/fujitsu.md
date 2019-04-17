---
title: Étude de cas SDK du centre d’administration de Windows - Fujitsu
description: Étude de cas SDK du centre d’administration de Windows - Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2052373"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Extensions d’intégrité de Fujitsu ServerView et RAID

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Importation de visibilité de bout en bout, à partir du système d’exploitation pour le matériel, dans le centre d’administration de Windows

Fujitsu est une informations japonaises et société de technologie de communication et le fabricant de produits serveur [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) et [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) . La [suite de gestion Fujitsu ServerView](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) fournit un ensemble complet d’outils pour le serveur de gestion du cycle de vie, y compris un agent côté serveur qui fournit une interface CIM et PowerShell pour la gestion du matériel.

Fujitsu constaté permet d’intégrer facilement avec le centre d’administration de Windows comme il fourni CIM et PowerShell interfaces qui peuvent communiquer avec les agents côté serveur. L’équipe de développement à Fujitsu a pu facilement mettre en œuvre les appels CIM qu’ils ont été familiarisés avec l’agent et visualiser les informations dans le centre d’administration de Windows à l’aide de composants d’interface utilisateur disponibles.

![Extension Fujitsu - affichage de l’arborescence d’intégrité](../../media/extend-case-study-fujitsu/health-tree.png)

Une fois que l’équipe s’est familiarisé avec le Kit de développement du centre d’administration Windows, ajout de l’interface utilisateur pour exposer des informations sur le matériel supplémentaire a souvent été simplement quelques lignes de code HTML et qu’ils ont été rapidement en mesure de développer à partir d’un seul outil à l’affichage d’un affichage Résumé du composant matériel d’intégrité, de vues détaillées pour les journaux des événements système, moniteur pilote, séparez les affichages de processeur, de mémoire, ventilateurs, alimentations, température et tension et même un outil supplémentaire pour la gestion de RAID. À l’aide de contrôles d’interface utilisateur disponibles dans le Kit de développement telles que l’arborescence, les contrôles de volet de la grille et détail activé l’équipe pour générer rapidement une interface utilisateur et d’obtenir une conception visuel et interaction très similaire au reste du centre d’administration de Windows.

![Extension Fujitsu - arborescence RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Afficher les volumes RAID extension Fujitsu-](../../media/extend-case-study-fujitsu/raid-volumes.png)

Le partenariat entre Fujitsu et l’équipe du centre d’administration de Windows indique clairement la valeur de l’intégration dans le centre d’administration de Windows, permettant aux clients d’avoir un aperçu de bout en bout dans les rôles serveur et services, le système d’exploitation et gestion du matériel .