---
title: Système Insights Forum aux questions
description: Système Insights Forum aux questions
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
ms.date: 5/23/2018
ms.openlocfilehash: 13767e1336d1ff729d1fbbe6cae3ed57d68cefc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851060"
---
# <a name="system-insights-faq"></a>Système Insights Forum aux questions

>S'applique à : Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>Comment utiliser les informations système avec Azure Monitor ou System Center Operations Manager ?

[Azure Monitor](https://azure.microsoft.com/services/monitor/) et [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) fournissent des informations opérationnelles sur vos déploiements afin de vous aider à gérer votre infrastructure. Informations système, en revanche, est une fonctionnalité de Windows Server qui présente les fonctionnalités d’analytique prédictive local. Ensemble, système Insights et Azure Monitor ou SCOM peut aider à faire apparaître les prédictions sur une population d’appareils :

 Azure Monitor SCOM peut de clé ou désactiver les événements créés par le système, Insights, comme système Insights renvoie le résultat de chaque prédiction au journal des événements. Ils peuvent se manifester ces prédictions spécifiques à l’ordinateur dans un parc de serveurs Windows, ce qui vous permet de disposer d’une vue unifiée de ces prédictions sur un groupe d’instances de serveur. 
 
 Consultez les ID de canal et d’événement pour chaque prédiction [ici](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results).

## <a name="how-does-system-insights-relate-to-windows-ml"></a>Comment les informations système concerne-t-il ML de Windows ?

[Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/) est une plateforme qui permet aux développeurs d’importer et noter les modèles d’apprentissage automatique préformé sur les appareils Windows. Ces modèles de bénéficient de l’accélération matérielle, et ils peuvent être évaluées localement. 

Système Insights est une fonctionnalité de Windows Server 2019 qui offre des fonctionnalités prédictives locales avec une expérience de gestion complète, y compris l’intégration de PowerShell et Windows Admin Center. 

## <a name="can-i-use-system-insights-for-my-cluster"></a>Puis-je utiliser les informations système pour mon cluster ? 

Oui. Système Insights peuvent exécuter indépendamment sur chaque nœud de cluster de basculement individuelles et le comportement par défaut de l’utilisation de prévisions système Insights sur le stockage local, le volume, le processeur et le réseau. **Vous pouvez également activer des prévisions pour le stockage en cluster**, de sorte que les capacités de prévision du stockage prédire l’utilisation de volumes en cluster et de stockage. 

Vous pouvez gérer ces paramètres dans Windows Admin Center ou PowerShell, et des informations plus détaillées sur cette fonctionnalité sont disponibles [ici](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/).
 

## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>Comment onéreux est qu’il s’exécute les fonctions par défaut ?

Chaque fonctionnalité par défaut est économique d’exécuter. Chaque fonctionnalité prendra plue de temps que vous collectez plus de données, mais elles doivent généralement s’achèvent dans un que quelques secondes. 

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur les informations système, utilisez les ressources suivantes :

- [Vue d’ensemble des informations système](overview.md)
- [Fonctionnalités de présentation](understanding-capabilities.md)
- [Gérer des fonctionnalités](managing-capabilities.md)
- [Ajout et le développement de fonctionnalités](adding-and-developing-capabilities.md)
