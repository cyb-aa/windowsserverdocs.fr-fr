---
title: FAQ System Insights
description: FAQ System Insights
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: 9d6ddd682def579796089266065be7d39ce361d1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856252"
---
# <a name="system-insights-faq"></a>FAQ System Insights

>S'applique à : Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>Comment utiliser System Insights avec Azure Monitor ou System Center Operations Manager ?

[Azure Monitor](https://azure.microsoft.com/services/monitor/) et [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) fournissent des informations opérationnelles sur vos déploiements pour vous aider à gérer votre infrastructure. System Insights, en revanche, est une fonctionnalité de Windows Server qui introduit des fonctionnalités d’analyse prédictive locales. Ensemble, System Insights et Azure Monitor ou SCOM peuvent aider à faire face aux prédictions sur un remplissage d’appareils :

 Azure Monitor ou SCOM peut déclencher la frappe des événements créés par System Insights, car System Insights génère le résultat de chaque prédiction dans le journal des événements. Ils peuvent faire face à ces prédictions spécifiques aux machines sur une flotte de serveurs Windows, ce qui vous permet d’avoir une vue unifiée de ces prédictions sur un groupe d’instances de serveur. 
 
 Consultez les ID de canal et d’événement pour chaque prédiction [ici](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results).

## <a name="how-does-system-insights-relate-to-windows-ml"></a>Quel est le lien entre System Insights et Windows ML ?

[Windows ml](https://docs.microsoft.com/windows/uwp/machine-learning/) est une plateforme qui permet aux développeurs d’importer et de noter des modèles de machine learning pré-formés sur des appareils Windows. Ces modèles tirent parti de l’accélération matérielle et peuvent être évalués localement. 

System Insights est une fonctionnalité de Windows Server 2019 qui offre des fonctionnalités prédictives locales, ainsi qu’une expérience de gestion complète, notamment PowerShell et l’intégration du centre d’administration Windows. 

## <a name="can-i-use-system-insights-for-my-cluster"></a>Puis-je utiliser System Insights pour mon cluster ? 

Oui. System Insights peut s’exécuter indépendamment sur chaque nœud de cluster de basculement individuel, et le comportement par défaut de System Insights prévoit l’utilisation dans le stockage local, le volume, le processeur et la mise en réseau. **Vous pouvez également activer la prévision pour le stockage en cluster**, de sorte que les fonctionnalités de prévision de stockage prédisent l’utilisation des volumes et du stockage en cluster. 

Vous pouvez gérer ces paramètres dans le centre d’administration Windows ou PowerShell, et des informations plus détaillées sur cette fonctionnalité sont disponibles [ici](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/).
 

## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>Quel est le coût de l’exécution des fonctionnalités par défaut ?

Chaque capacité par défaut est peu coûteuse à s’exécuter. L’exécution de chaque fonctionnalité prendra plus de temps, car vous recueillerez davantage de données, mais elles devraient généralement s’exécuter en quelques secondes seulement. 

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur System Insights, utilisez les ressources suivantes :

- [Vue d’ensemble de System Insights](overview.md)
- [Présentation des fonctionnalités](understanding-capabilities.md)
- [Gestion des fonctionnalités](managing-capabilities.md)
- [Ajout et développement de fonctionnalités](adding-and-developing-capabilities.md)
