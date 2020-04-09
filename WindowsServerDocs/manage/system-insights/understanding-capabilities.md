---
title: Présentation des fonctionnalités
description: Cette rubrique définit le concept de fonctionnalités dans System Insights et présente les fonctionnalités par défaut disponibles dans Windows Server 2019.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: 9b0f043aab5773773785afc7fb48ba0295a76865
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858742"
---
# <a name="understanding-capabilities"></a>Présentation des fonctionnalités

>S'applique à : Windows Server 2019

Cette rubrique définit le concept de fonctionnalités dans System Insights et présente les fonctionnalités par défaut disponibles dans Windows Server 2019. 

Cette rubrique décrit également les sources de données, les chronologies de prédiction et les États de prédiction utilisés pour les fonctionnalités par défaut. 

## <a name="capability-overview"></a>Vue d’ensemble des fonctionnalités
Une fonctionnalité System Insights est un modèle de Machine Learning ou de statistiques qui analyse les données système pour vous aider à mieux comprendre le fonctionnement de votre déploiement. System Insights introduit un ensemble initial de fonctionnalités par défaut et vous permet d’ajouter de nouvelles fonctionnalités de manière dynamique, sans avoir à mettre à jour le système d’exploitation. 

>[!NOTE]
>Une documentation détaillée expliquant comment créer, ajouter et mettre à jour des fonctionnalités est disponible [ici](adding-and-developing-capabilities.md), et [le document gestion des fonctionnalités](managing-capabilities.md) fournit des informations de haut niveau sur cette fonctionnalité.

En outre, chaque fonctionnalité s’exécute localement sur une instance de Windows Server, et chaque fonctionnalité peut être gérée individuellement.

### <a name="capability-outputs"></a>Sorties de capacité
Lorsqu’une fonctionnalité est appelée, elle fournit une sortie pour vous aider à expliquer le résultat de son analyse ou de sa prédiction. Chaque sortie doit contenir un **État** et une **Description** de l’État pour décrire la prédiction, et chaque résultat peut éventuellement contenir des données spécifiques à la fonctionnalité associées à la prédiction. La **Description** de l’état vous aide à fournir une explication contextuelle de l' **État**, et la fonctionnalité signale un état **OK**, **Avertissement**ou **critique** . En outre, une fonctionnalité peut utiliser une **erreur** ou un état **aucun** si aucune prédiction n’a été effectuée. Ensemble, Voici les États de fonctionnalité et leurs significations de base : 

- **OK** -tout semble correct.
- **Avertissement** : aucune intervention immédiate n’est requise, mais vous devez jeter un œil. 
- **Critique** : vous devez jeter un coup d’œil. 
- **Erreur** -un problème inconnu est à l’origine de l’échec de la fonctionnalité. 
- **Aucun** : aucune prédiction n’a été effectuée. Cela peut être dû à un manque de données ou à toute autre raison spécifique à la fonctionnalité de ne pas faire de prédiction. 

En outre, toutes les données spécifiques aux fonctionnalités contenues dans le résultat seront placées dans un fichier JSON accessible par l’utilisateur, et le chemin d’accès du fichier [peut être trouvé à l’aide de PowerShell](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results). 

## <a name="default-capabilities"></a>Fonctionnalités par défaut
Dans Windows Server 2019, System Insights introduit quatre fonctionnalités par défaut axées sur la prévision de la capacité :

- **Prévision** de la capacité de l’UC-prévisions d’utilisation de l’UC. 
- **Prévision de la capacité réseau** -prévoit l’utilisation du réseau pour chaque carte réseau. 
- **Prévision de la consommation totale du stockage** -prévoit la consommation totale du stockage sur tous les lecteurs locaux. 
- **Prévision de consommation de volume** -prévoit la consommation de stockage pour chaque volume.

Chaque fonctionnalité analyse les données historiques passées pour prédire l’utilisation future, et **toutes les fonctionnalités de prévision sont conçues pour prévoir des tendances à long terme plutôt qu’un comportement à long terme**, aidant les administrateurs à approvisionner correctement le matériel et à ajuster leurs charges de travail pour éviter les conflits de ressources futurs. Étant donné que ces fonctionnalités se concentrent sur l’utilisation à long terme, ces fonctionnalités analysent les données quotidiennes. 

### <a name="forecasting-model"></a>Modèle de prévision
Les fonctionnalités par défaut utilisent un modèle de prévision pour prédire l’utilisation future et, pour chaque prédiction, le modèle est formé localement sur les données de votre ordinateur. Ce modèle est conçu pour faciliter la détection des tendances à long terme, et la reformation sur chaque instance de Windows Server permet de s’adapter au comportement et aux nuances spécifiques de l’utilisation de chaque ordinateur.

>[!NOTE]
>Déterminer le type de modèle à utiliser pour tester de nombreux modèles à l’aide d’un jeu de données contenant des dizaines de milliers de machines. Après l’analyse et la modification de ces modèles, nous avons décidé d’utiliser un modèle de prévision régressif automatique, car il produit des prédictions très précises et visuellement intuitives, tout en nécessitant trop de temps pour former. Toutefois, ce modèle nécessite trois semaines de données d’apprentissage. par conséquent, chaque fonctionnalité utilise une tendance linéaire de base jusqu’à ce que trois semaines de données soient disponibles.

### <a name="forecasting-timelines"></a>Chronologies de prévisions
Les fonctionnalités par défaut prévoient un certain nombre de jours dans le futur en fonction du nombre de jours pendant lesquels les données ont été collectées. Le tableau suivant présente les chronologies de prédiction de ces fonctionnalités :

| Taille des données d’entrée | Longueur de la prévision |
| --------------- | --------------- |
| 0-5 jours | Aucune prédiction n’est effectuée. |
| 6-180 jours | 1/3 * taille des données d’entrée |
| 180-365 jours | 60 jours | 

### <a name="forecasting-data"></a>Prévision des données
Chaque fonctionnalité analyse les données quotidiennes pour prévoir l’utilisation future. Toutefois, l’utilisation de l’UC, de la mise en réseau et même du stockage peut changer fréquemment pendant la journée, en ajustant de façon dynamique les charges de travail sur l’ordinateur. Étant donné que l’utilisation n’est pas constante tout au long de la journée, il est important de représenter correctement l’utilisation quotidienne dans un point de données unique. Le tableau ci-dessous détaille les points de données spécifiques et la façon dont les données sont traitées :


| Nom de fonctionnalité | Source (s) de données | Logique de filtrage |
| --------------- | -------------- | ---------------- |
 Prévision de consommation de volume          | Taille du volume                    | Utilisation maximale quotidienne              
 Prévision de la consommation totale du stockage   | Somme des tailles de volume, somme des tailles de disque              | Utilisation maximale quotidienne             
 Prévision de la capacité de l’UC                | % de temps processeur  | Moyenne sur 2 heures maximum par jour   
 Prévision de la capacité réseau         | Total des octets/s         | Moyenne sur 2 heures maximum par jour  

Lors de l’évaluation de la logique de filtrage ci-dessus, il est important de noter que chaque fonctionnalité cherche à informer les administrateurs lorsque l’utilisation future dépassera significativement la capacité disponible, même si le processeur atteint momentanément 100% d’utilisation, l’utilisation du processeur peut ne pas avoir entraîné une dégradation significative des performances ou une contention des ressources. Pour le processeur et la mise en réseau, alors il doit y avoir une utilisation intensive élevée plutôt que des pics momentanés. La moyenne de l’utilisation de l’UC et de la mise en réseau tout au long de la journée, toutefois, risque de perdre des informations d’utilisation importantes, car quelques heures d’utilisation intensive du processeur ou de la mise en réseau pourraient avoir un impact significatif sur les performances de vos charges de travail critiques. La moyenne de 2 heures au maximum pour chaque jour évite ces extrêmes et produit toujours des données significatives pour chaque fonctionnalité à analyser.

Toutefois, pour l’utilisation du volume et du stockage total, l’utilisation du stockage ne peut pas dépasser la capacité disponible, même momentanément, l’utilisation maximale quotidienne est donc utilisée pour ces fonctionnalités. 

### <a name="forecasting-statuses"></a>Prévisions d’États
Toutes les fonctionnalités de System Insights doivent générer un état associé à chaque prédiction. Chaque fonction par défaut utilise la logique suivante pour définir chaque État de prédiction :
- **OK**: la prévision ne dépasse pas la capacité disponible.
- **Avertissement**: la prévision dépasse la capacité disponible dans les 30 prochains jours. 
- **Critique**: la prévision dépasse la capacité disponible dans les 7 prochains jours. 
- **Erreur**: la fonctionnalité a rencontré une erreur inattendue. 
- **None**: il n’y a pas assez de données pour effectuer une prédiction. Cela peut être dû à un manque de données ou au fait qu’aucune donnée n’a été signalée récemment.

>[!NOTE]
>Si une capacité prévoit plusieurs instances, telles que plusieurs volumes ou cartes réseau, l’État reflète l’état le plus grave dans toutes les instances. Les États individuels de chaque volume ou carte réseau sont visibles dans le centre d’administration Windows ou dans les données contenues dans la sortie de chaque fonctionnalité. Pour obtenir des instructions sur la façon d’analyser la sortie JSON des fonctionnalités par défaut, consultez [ce blog](https://aka.ms/systeminsights-mitigationscripts). 


## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur System Insights, utilisez les ressources suivantes :

- [Vue d’ensemble de System Insights](overview.md)
- [Gestion des fonctionnalités](managing-capabilities.md)
- [Ajout et développement de fonctionnalités](adding-and-developing-capabilities.md)
- [FAQ System Insights](faq.md)
