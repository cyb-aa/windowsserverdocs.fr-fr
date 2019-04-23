---
title: Sources de données système Insights
description: Lorsque vous écrivez une nouvelle fonctionnalité dans les informations système, vous pouvez spécifier les sources de données nouvelle ou existante pour collecter localement et à analyser. Cette rubrique décrit les sources de données que vous pouvez choisir lorsque vous inscrivez une nouvelle fonctionnalité.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 9b46db90787d24a173ffa472ec1ecb8eaffe054b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845410"
---
# <a name="system-insights-data-sources"></a>Sources de données système Insights

>S'applique à : Windows Server 2019

Système Insights introduit des fonctionnalités de collecte de données extensible. Lorsque vous écrivez une nouvelle fonctionnalité, vous pouvez spécifier les sources de données nouvelle ou existante pour collecter localement et à analyser. Cette rubrique décrit les sources de données que vous pouvez choisir lorsque vous inscrivez une nouvelle fonctionnalité.

## <a name="data-sources"></a>Sources de données
Lorsque vous écrivez une nouvelle fonctionnalité, vous devez identifier les sources de données spécifiques à collecter pour chaque fonctionnalité. Les sources de données que vous spécifiez seront collectées et persistantes directement sur votre ordinateur, et vous pouvez choisir parmi trois types de sources de données :

- **Compteurs de performances**: 
    - Spécifiez le chemin de compteur, nom et instances et système Insights collecte les données pertinentes signalées par ces compteurs de performance. 

- **Événements système**:
    - Spécifier le canal nom et ID d’événement et système Insights enregistre le nombre de fois que l’événement s’est produite.

- **Série Well-Known**
    - Système Insights collecte des informations de base sur votre ordinateur pour un peu, bien définies des ressources. Ces séries sont utilisés pour les fonctions par défaut, mais ils peuvent également être utilisés par la fonctionnalité de n’importe quel personnalisée. Ces collecter les informations suivantes :

        - **Disque**: 
            - *Propriétés* : GUID
            - *Données*: Size
        - **Volume**:
            - *Propriétés* : UniqueId, DriveLetter, FileSystemLabel, Size
            - *Données*: Taille utilisée
        - **Carte réseau**:
            - *Propriétés* : InterfaceGuid, InterfaceDescription, Speed
            - *Données*: Octets reçus par seconde, octets envoyés/s, Total des octets/s
        - **PROCESSEUR**: 
            - *Propriétés*:-
            - *Données*: % temps processeur

    - Spécifier une série bien connue et système Insights renvoie les données collectées par cette série. 


## <a name="retention-timelines-and-collection-intervals"></a>Intervalles de collecte et les chronologies de rétention
Les sources de données ci-dessus ont des intervalles de rétention différentes chronologies et de la collection. Le tableau ci-dessous révèle souvent comment long et la façon dont chaque source de données est collectée :

| Source de données | Chronologie de rétention | Intervalle de collecte |
| --------------- | --------------- | ----------- |
| Compteurs de performances | 3 derniers mois | 15 minutes |
| Événements du système | 3 derniers mois | 15 minutes |
| Série Well-Known | 1 an | 1 heure |


### <a name="aggregation-types"></a>Types d’agrégation
Étant donné que chaque série n'enregistrer qu’un seul point de données pour chaque intervalle de collection, chaque série a un compte de type d’agrégation il. Le tableau ci-dessous décrit la source de données et le type d’agrégation correspondante :

>[!NOTE]
>Compteurs de performances, vous pouvez choisir à partir de quelques différents types d’agrégation.

| Source de données | Types d’agrégation |
| --------------- | --------------- |
| Compteurs de performances | SUM, min, max, moyenne, |
| Événements du système | Nombre |
| Série bien connu de disque | Dernier (valeur la plus récente dans l’intervalle de collecte) |
| Série bien connu de volume | Dernier (valeur la plus récente dans l’intervalle de collecte) |
| Série bien connu du processeur | Moyenne |
| Série bien connu de réseau | Moyenne |

## <a name="data-footprint"></a>Encombrement des données

Système Insights collecte toutes les données localement sur votre lecteur C (c). En règle générale, l’encombrement de données système Insights est modeste. Il dépend directement le type et le nombre de sources de données spécifie de chaque fonctionnalité, et le tableau ci-dessous décrit en détail l’utilisation du stockage pour chaque type de données :

| Source de données | Encombrement maximale |
| --------------- | --------------- |
| Compteurs de performances | 240 KO |
| Événements du système | 200 Ko |
| Série bien connu de disque | 200 Ko par disque |
| Série bien connu de volume | 300 Ko par volume |
| Série bien connu du processeur | 100 KO |
| Série bien connu de réseau | 300 Ko par carte réseau |

>[!NOTE]
>**Pour la valeur par défaut de capacités de prévision, l’encombrement maximale doit être inférieure à 10 Mo pour la plupart des ordinateurs autonomes.** 

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur les informations système, utilisez les ressources suivantes :

- [Vue d’ensemble des informations système](overview.md)
- [Fonctionnalités de présentation](understanding-capabilities.md)
- [Gérer des fonctionnalités](managing-capabilities.md)
- [Ajout et le développement de fonctionnalités](adding-and-developing-capabilities.md)
- [Système Insights Forum aux questions](faq.md)
