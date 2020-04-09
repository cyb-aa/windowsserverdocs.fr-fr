---
title: Vue d’ensemble des insights système
description: System Insights est une nouvelle fonctionnalité d’analyse prédictive de Windows Server 2019. Les fonctionnalités prédictives de System Insights, chacune gérée par un modèle d’apprentissage automatique, analysent localement les données du système Windows Server, telles que les compteurs de performances et les événements, et fournissent des informations sur le fonctionnement de vos serveurs et vous aident à réduire les dépenses opérationnelles associées à la gestion réactive des problèmes dans vos déploiements.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: e2530ff9ecb4bcf69f2f9a3a452f51696b4466d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815942"
---
# <a name="system-insights-overview"></a>Vue d’ensemble des insights système

>S'applique à : Windows Server 2019

System Insights est une nouvelle fonctionnalité d’analyse prédictive de Windows Server 2019. Les fonctionnalités prédictives de System Insights, chacune gérée par un modèle d’apprentissage automatique, analysent localement les données du système Windows Server, telles que les compteurs de performances et les événements, et fournissent des informations sur le fonctionnement de vos serveurs et vous aident à réduire les dépenses opérationnelles associées à la gestion réactive des problèmes dans vos déploiements. 

Dans Windows Server 2019, System Insights est fourni avec quatre fonctionnalités par défaut axées sur la prévision de capacité, prévoyant les ressources futures pour le calcul, la mise en réseau et le stockage en fonction de vos modèles d’utilisation précédents. System Insights est également fourni avec une [infrastructure extensible](adding-and-developing-capabilities.md). par conséquent, Microsoft et les tiers peuvent ajouter de nouvelles fonctionnalités prédictives à System Insights sans mettre à jour le système d’exploitation. 

Vous pouvez gérer System Insights par le biais d’une extension intuitive du [Centre d’administration Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) ou [directement par le biais de PowerShell](https://aka.ms/SystemInsightsPowerShell), et System Insights vous permet de configurer chaque fonctionnalité prédictive séparément en fonction des besoins de votre déploiement. Tous les résultats de prédiction sont publiés dans le journal des événements, ce qui vous permet d’utiliser [Azure Monitor](https://azure.microsoft.com/services/monitor/) ou [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) pour agréger et afficher facilement des prédictions sur un groupe d’ordinateurs.

![Extension System Insights dans le centre d’administration Windows, qui montre la capacité de prévision de la capacité du processeur avec un graphique traçant la prévision](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>Fonctionnalité locale
System Insights s’exécute entièrement localement sur Windows Server. Grâce à la nouvelle fonctionnalité introduite dans Windows Server 2019, toutes vos données sont collectées, conservées et analysées directement sur votre ordinateur, ce qui vous permet de bénéficier de fonctionnalités d’analyse prédictive sans aucune connectivité au Cloud.

Vos données système sont stockées sur votre ordinateur, et elles sont analysées par des fonctionnalités prédictives qui ne nécessitent pas de reformation dans le Cloud. Avec System Insights, vous pouvez conserver vos données sur votre ordinateur tout en bénéficiant des fonctionnalités d’analyse prédictive. 

## <a name="get-started"></a>Prise en main

<iframe src=https://www.youtube-nocookie.com/embed/AJxQkx5WSaA width=560 height=315 allowfullscreen></iframe>

>[!TIP]
>Regardez ces courtes vidéos pour découvrir les informations dont vous avez besoin pour commencer et gérer en toute confiance System Insights : [prise en main de System Insights en 10 minutes](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

### <a name="requirements"></a>Configuration requise
System Insights est disponible sur n’importe quelle instance de Windows Server 2019. Il s’exécute sur les ordinateurs hôtes et les ordinateurs invités, sur tout hyperviseur et sur n’importe quel Cloud.

### <a name="install-system-insights"></a>Installer System Insights
>[!IMPORTANT]
>System Insights collecte et stocke les données d’une année en local. Si vous souhaitez conserver vos données lors de la mise à niveau de votre système d’exploitation, assurez **-vous d’utiliser la mise à niveau sur place**.

#### <a name="install-the-feature"></a>Installer la fonctionnalité
Vous pouvez installer System Insights à l’aide de l’extension dans le centre d’administration Windows :

![Expérience du jour 0 pour l’extension System Insights.](media/day-0-2.png)

Vous pouvez également installer System Insights directement par le biais de Gestionnaire de serveur en ajoutant la fonctionnalité **System-Insights** , ou à l’aide de PowerShell :

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>Fournir un commentaire
Nous aimerions nous faire part de vos commentaires pour nous aider à améliorer cette fonctionnalité. Vous pouvez utiliser les canaux suivants pour envoyer vos commentaires :
- **Hub de commentaires**: utilisez l’outil Hub de commentaires de Windows 10 pour signaler un bogue ou un commentaire. Dans ce cas, spécifiez :
    - **Catégorie**: serveur 
    - Sous- **catégorie**: System Insights
- **UserVoice**: envoyer des demandes de fonctionnalités via notre [page UserVoice](https://windowsserver.uservoice.com/forums/295071-management-tools). Partagez avec vos collègues pour voter des éléments qui sont importants pour vous.
- **Courrier électronique**: Si vous souhaitez envoyer vos commentaires à l’équipe des fonctionnalités, envoyez un e-mail à system-insights-feed@microsoft.com. N’oubliez pas que nous pouvons toujours vous demander d’utiliser le hub de commentaires ou UserVoice.

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur System Insights, utilisez les ressources suivantes :

- [Présentation des fonctionnalités](understanding-capabilities.md)
- [Gestion des fonctionnalités](managing-capabilities.md)
- [Ajout et développement de fonctionnalités](adding-and-developing-capabilities.md)
- [FAQ System Insights](faq.md)