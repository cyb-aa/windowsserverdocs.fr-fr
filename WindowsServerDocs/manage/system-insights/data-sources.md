---
title: Sources de données System Insights
description: Lorsque vous écrivez une nouvelle fonctionnalité dans System Insights, vous pouvez spécifier des sources de données existantes ou nouvelles à collecter localement et à analyser. Cette rubrique décrit les sources de données que vous pouvez choisir lors de l’inscription d’une nouvelle fonctionnalité.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 920c5fa5919fd2c35edb99cc4e724745715091c7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858422"
---
# <a name="system-insights-data-sources"></a>Sources de données System Insights

>S'applique à : Windows Server 2019

System Insights introduit la fonctionnalité de collecte de données extensible. Lorsque vous écrivez une nouvelle fonctionnalité, vous pouvez spécifier des sources de données existantes ou nouvelles à collecter localement et à analyser. Cette rubrique décrit les sources de données que vous pouvez choisir lors de l’inscription d’une nouvelle fonctionnalité.

## <a name="data-sources"></a>Sources de données
Lors de l’écriture d’une nouvelle fonctionnalité, vous devez identifier les sources de données spécifiques à collecter pour chaque fonctionnalité. Les sources de données que vous spécifiez sont collectées et conservées directement sur votre ordinateur, et vous pouvez choisir parmi trois types de sources de données :

- **Compteurs de performances**: 
    - Spécifiez le chemin d’accès, le nom et les instances du compteur, et System Insights collecte les données pertinentes signalées par ces compteurs de performances. 

- **Événements système**:
    - Spécifiez le nom du canal et l’ID de l’événement, et System Insights enregistrera le nombre de fois où l’événement s’est produit.

- **Série bien connue**
    - System Insights collecte des informations de base sur votre machine pour quelques ressources bien définies. Ces séries sont utilisées pour les fonctionnalités par défaut, mais elles peuvent également être utilisées par n’importe quelle fonctionnalité personnalisée. Celles-ci recueillent les informations suivantes :

        - **Disque**: 
            - *Propriétés*: GUID
            - *Données*: taille
        - **Volume**:
            - *Propriétés*: UniqueId, DriveLetter, FileSystemLabel, Size
            - *Données*: taille utilisée
        - **Carte réseau**:
            - *Propriétés*: InterfaceGuid, InterfaceDescription, Speed
            - *Données*: octets reçus/s, octets envoyés/s, total des octets/s
        - **UC**: 
            - *Propriétés*:-
            - *Données*:% temps processeur

    - Spécifiez une série bien connue, et System Insights renverra les données collectées par cette série. 


## <a name="retention-timelines-and-collection-intervals"></a>Chronologies de rétention et intervalles de collecte
Les sources de données ci-dessus ont des chronologies de rétention et des intervalles de collecte différents. Le tableau ci-dessous indique la durée et la fréquence de collecte de chaque source de données :

| Source de données | Chronologie de rétention | Intervalle de collecte |
| --------------- | --------------- | ----------- |
| Compteurs de performances | 3 mois | 15 minutes |
| Événements système | 3 mois | 15 minutes |
| Série bien connue | 1 an | 1 heure |


### <a name="aggregation-types"></a>Types d’agrégation
Étant donné que chaque série enregistre un seul point de données pour chaque intervalle de collecte, chaque série a un type d’agrégation afin. Le tableau ci-dessous décrit la source de données et le type d’agrégation correspondant :

>[!NOTE]
>Pour les compteurs de performances, vous pouvez choisir parmi différents types d’agrégation.

| Source de données | Types d’agrégation |
| --------------- | --------------- |
| Compteurs de performances | Sum, Average, Max, min |
| Événements système | Nombre |
| Série bien connue du disque | Dernière (valeur la plus récente dans l’intervalle de collecte) |
| Série bien connue du volume | Dernière (valeur la plus récente dans l’intervalle de collecte) |
| Série bien connue de l’UC | Moyenne |
| Série bien connue du réseau | Moyenne |

## <a name="data-footprint"></a>Encombrement des données

System Insights collecte toutes les données localement sur votre lecteur C (C :). En général, l’encombrement des données System Insights est modeste. Il dépend directement du type et du nombre de sources de données que chaque fonction spécifie, et la table ci-dessous détaille l’utilisation du stockage pour chaque type de données :

| Source de données | Encombrement maximal |
| --------------- | --------------- |
| Compteurs de performances | 240 KO |
| Événements système | 200 Ko |
| Série bien connue du disque | 200 Ko par disque |
| Série bien connue du volume | 300 Ko par volume |
| Série bien connue de l’UC | 100 KO |
| Série bien connue du réseau | 300 Ko par carte réseau |

>[!NOTE]
>**Pour les fonctionnalités de prévision par défaut, l’encombrement maximal doit être inférieur à 10 Mo pour la plupart des ordinateurs autonomes.** 

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur System Insights, utilisez les ressources suivantes :

- [Vue d’ensemble de System Insights](overview.md)
- [Présentation des fonctionnalités](understanding-capabilities.md)
- [Gestion des fonctionnalités](managing-capabilities.md)
- [Ajout et développement de fonctionnalités](adding-and-developing-capabilities.md)
- [FAQ System Insights](faq.md)
