---
title: Présentation des fonctionnalités
description: Cette rubrique définit le concept de fonctionnalités dans les informations système et présente les fonctions par défaut disponibles dans Windows Server 2019.
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
ms.date: 6/05/2018
ms.openlocfilehash: 21932a3e45d7fc6711400eb30c63c3412bbc116e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830970"
---
# <a name="understanding-capabilities"></a>Présentation des fonctionnalités

>S'applique à : Windows Server 2019

Cette rubrique définit le concept de fonctionnalités dans les informations système et présente les fonctions par défaut disponibles dans Windows Server 2019. 

Cette rubrique décrit également les sources de données, chronologies de prédiction et les États de prédiction utilisées pour les fonctions par défaut. 

## <a name="capability-overview"></a>Vue d’ensemble de la fonctionnalité
Une fonctionnalité système Insights est un apprentissage ou le modèle statistique qui analyse les données système pour vous aider dans augmentée pour connaître le fonctionnement de votre déploiement. Système Insights présente un ensemble initial de fonctionnalités par défaut, et il permet d’ajouter de nouvelles fonctionnalités dynamiquement, sans avoir à mettre à jour le système d’exploitation. 

>[!NOTE]
>Une documentation détaillée qui explique comment créer, ajouter, et les fonctionnalités de mise à jour est disponible [ici](adding-and-developing-capabilities.md), et [le document de fonctionnalités de gestion](managing-capabilities.md) fournit plus d’informations générales à ce sujet fonctionnalité.

En outre, chaque fonctionnalité s’exécute localement sur une instance de Windows Server, et chaque fonctionnalité peut être gérée individuellement.

### <a name="capability-outputs"></a>Sorties de fonctionnalité
Lorsqu’une fonctionnalité est appelée, elle fournit une sortie pour expliquer le résultat de son analyse ou la prédiction. Chaque sortie doit contenir un **état** et un **Description de l’état** pour décrire la prédiction et chaque résultat peut éventuellement contenir des données spécifiques à la fonctionnalité associées à la prédiction. Le **Description de l’état** fournit une explication contextuelle pour le **état**, et la fonctionnalité de rapports soit un **OK**, **avertissement**, ou **critique** état. En outre, une fonctionnalité peut utiliser un **erreur** ou **aucun** état si aucune prévision n’a été effectuée. Ensemble, voici les États de fonctionnalité et leur signification de base : 

- **OK** -tout semble correct.
- **Avertissement** : aucune attention immédiate ne requise, mais vous devriez jeter un œil. 
- **Critique** -vous devriez jeter un œil bientôt. 
- **Erreur** -un problème inconnu a provoqué la possibilité d’échouer. 
- **Aucun** -aucune prévision n’a été effectuée. Cela peut être dû à un manque de données ou toute autre raison spécifique à la fonctionnalité pour ne pas effectuer une prédiction. 

En outre, toutes les données spécifiques à la fonctionnalité contenues dans le résultat seront placées dans un fichier JSON accessible à l’utilisateur et le chemin d’accès du fichier [est accessible à l’aide de PowerShell](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results). 

## <a name="default-capabilities"></a>Fonctions par défaut
Dans Windows Server 2019, système Insights introduit quatre fonctions par défaut axées sur la prévision de capacité :

- **Prévisions de capacité processeur** -utilisation de l’UC de prévisions. 
- **Prévisions de capacité de mise en réseau** -utilisation pour chaque carte réseau du réseau de prévisions. 
- **Prévision de la consommation de stockage total** -nombre total de prévisions consommation du stockage sur tous les lecteurs locaux. 
- **Prévision de la consommation de volume** -prévoit la consommation de stockage pour chaque volume.

Chaque fonctionnalité analyse les données historiques pour prédire l’utilisation future, passées et **toutes les fonctionnalités de prévisions sont conçus pour prévoir les tendances à long terme au lieu du comportement à court terme**, aidant les administrateurs correctement configurer le matériel et ajuster leurs charges de travail pour éviter les conflits de ressources futures. Étant donné que ces fonctionnalités vous concentrer sur l’utilisation à long terme, ces fonctionnalités analyser les données quotidiennes. 

### <a name="forecasting-model"></a>Modèle de prévision
Les fonctions par défaut utilisent un modèle de prévision pour prédire l’utilisation future, et pour chaque prédiction, l’apprentissage du modèle localement sur les données de votre ordinateur. Ce modèle est conçu pour aider à détecter plus long terme des tendances et reformation sur chaque instance de Windows Server permet de s’adapter au comportement spécifique et des nuances de l’utilisation de chaque machine.

>[!NOTE]
>Déterminer quel type de modèle à utiliser requis de nombreux modèles à l’aide d’un jeu de données contenant des dizaines de milliers d’ordinateurs de test. Après l’analyse et au peaufinage de ces modèles, nous avons décidé d’utiliser un modèle de prévision AUTORÉGRESSIF, car il génère des prédictions extrêmement précis et visuellement intuitives tout en nécessitant ne pas beaucoup trop de temps pour effectuer l’apprentissage. Ce modèle, toutefois, requiert que trois semaines de données d’apprentissage, pour chaque fonctionnalité utilise une tendance linéaire base jusqu'à trois semaines de données sont disponibles.

### <a name="forecasting-timelines"></a>Prévision des chronologies
Les fonctions par défaut prévoir un certain nombre de jours dans le futur selon le nombre de jours pour lequel les données ont été collectées. Le tableau suivant présente les délais de prédiction de ces fonctionnalités :

| Taille des données d’entrée | Longueur de la prévision |
| --------------- | --------------- |
| 0-5 jours | Aucune prévision n’est effectuée. |
| 6-180 jours | 1/3 * taille des données d’entrée |
| 180-365 jours | 60 jours | 

### <a name="forecasting-data"></a>Données de prévision
Chaque fonctionnalité analyse quotidien des données pour prévoir une utilisation ultérieure. Processeur, de la mise en réseau et même l’utilisation du stockage, toutefois, pouvez changent fréquemment pendant la journée, ajuster de façon dynamique pour les charges de travail sur l’ordinateur. Étant donné que l’utilisation n’est pas constante pendant la journée, il est important de représenter correctement l’utilisation quotidienne dans un seul point de données. Le tableau ci-dessous répertorie les points de données spécifique et la façon dont les données sont traitées :


| Nom de fonctionnalité | Sources de données | Logique de filtrage |
| --------------- | -------------- | ---------------- |
 Prévision de la consommation de volume          | Taille du volume                    | Utilisation quotidienne maximale              
 Prévision de la consommation de stockage total   | Somme des tailles de volume, somme des tailles de disque              | Utilisation quotidienne maximale             
 Prévisions de capacité du processeur                | % Processor Time  | Moyenne de 2 heures maximum par jour   
 Prévisions de capacité de mise en réseau         | Total des octets/s         | Moyenne de 2 heures maximum par jour  

Lorsque vous évaluez la logique de filtrage ci-dessus, ses importante de noter que chaque fonctionnalité vise à informer les administrateurs lorsque l’utilisation future dépassera concrètement la capacité disponible : bien que le processeur atteindre momentanément une utilisation de 100 %, l’utilisation du processeur ne peut pas avoir a provoqué le conflit de ressources ou une dégradation des performances significative. Pour le processeur et la mise en réseau, puis, il doit être maintenu une utilisation élevée plutôt que des pics momentanées. Calculer la moyenne du processeur et de mise en réseau de l’utilisation pendant la journée entière, toutefois, perdrait informations d’utilisation important, que quelques heures du processeur ou de mise en réseau de l’utilisation élevée peut affecter significativement les performances de vos charges de travail critiques. La moyenne maximale de 2 heures chaque jour évite ces extrêmes et génère toujours des données significatives pour chaque capacité à analyser.

Pour le volume et l’utilisation du stockage total, toutefois, l’utilisation du stockage ne peut pas dépasser la capacité disponible, même momentanément, donc l’utilisation quotidienne maximale est utilisée pour ces fonctionnalités. 

### <a name="forecasting-statuses"></a>États de prévisions
Toutes les fonctionnalités système Insights doivent sortie un état associé à chaque prédiction. Chaque fonctionnalité par défaut utilise la logique suivante pour définir l’état de chaque prédiction :
- **OK**: La prévision ne dépasse pas la capacité disponible.
- **Avertissement** : La prévision dépasse la capacité disponible au cours des 30 prochains jours. 
- **Critique** : La prévision dépasse la capacité disponible dans les 7 prochains jours. 
- **Erreur** : La fonctionnalité a rencontré une erreur inattendue. 
- **Aucun** : Il n’est pas suffisamment de données pour élaborer une prédiction. Cela peut être dû à un manque de données ou parce qu’aucune donnée n’a été signalée récemment.

>[!NOTE]
>Si un prévisions de capacité sur plusieurs instances - telles que plusieurs volumes ou des cartes réseau - l’état reflète l’état les plus graves dans toutes les instances. États individuels pour chaque carte réseau ou de volume sont visibles dans Windows Admin Center ou dans les données contenues dans la sortie de chaque fonctionnalité. Pour obtenir des instructions sur la façon d’analyser la sortie JSON des fonctionnalités par défaut, visitez [ce billet de blog](https://aka.ms/systeminsights-mitigationscripts). 


## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur les informations système, utilisez les ressources suivantes :

- [Vue d’ensemble des informations système](overview.md)
- [Gérer des fonctionnalités](managing-capabilities.md)
- [Ajout et le développement de fonctionnalités](adding-and-developing-capabilities.md)
- [Système Insights Forum aux questions](faq.md)
