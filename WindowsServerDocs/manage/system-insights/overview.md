---
title: Vue d’ensemble des informations système
description: Système Insights est une nouvelle fonctionnalité d’analytique prédictive dans Windows Server 2019. Les fonctionnalités prédictives système Insights - chaque s’appuyant sur un modèle d’apprentissage automatique - analyser localement les données de système de Windows Server, telles que les compteurs de performances et des événements, fournissant des informations sur le fonctionnement de vos serveurs et de vous aider à réduire le dépenses d’exploitation associés de manière réactive la gestion des problèmes dans vos déploiements.
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
ms.openlocfilehash: ca471e5e747c7aaa369f5725290e16deab7a6660
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887240"
---
# <a name="system-insights-overview"></a>Vue d’ensemble des informations système

>S'applique à : Windows Server 2019

Système Insights est une nouvelle fonctionnalité d’analytique prédictive dans Windows Server 2019. Les fonctionnalités prédictives système Insights - chaque s’appuyant sur un modèle d’apprentissage automatique - analyser localement les données de système de Windows Server, telles que les compteurs de performances et des événements, fournissant des informations sur le fonctionnement de vos serveurs et de vous aider à réduire le dépenses d’exploitation associés de manière réactive la gestion des problèmes dans vos déploiements. 

Dans Windows Server 2019, système Insights est fourni avec quatre fonctions par défaut axées sur la capacité de prévision, prédiction des futures ressources de calcul, de mise en réseau et de stockage en fonction de vos modèles d’utilisation. Les informations système est également livré avec un [infrastructure extensible](adding-and-developing-capabilities.md), de sorte que Microsoft et 3e parties peuvent ajouter de nouvelles fonctionnalités prédictives à système Insights sans mettre à jour le système d’exploitation. 

Vous pouvez gérer les informations système via intuitive [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) extension ou [directement par le biais de PowerShell](https://aka.ms/SystemInsightsPowerShell), ainsi que les informations système configurer chaque fonctionnalité prédictive séparément en fonction des besoins de votre déploiement. Tous les résultats de prédiction sont publiés dans le journal des événements, ce qui vous permet d’utiliser [Azure Monitor](https://azure.microsoft.com/services/monitor/) ou [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) pour agréger facilement et de visualiser les prédictions dans un groupe de machines.

![Extension Insights du système dans Windows Admin Center, montrant les prévisions de fonctionnalité avec un graphique de traçage de la prévision de capacité de processeur](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>Fonctionnalités locales
Les informations système s’exécute entièrement localement sur Windows Server. À l’aide de la nouvelle fonctionnalité introduite dans Windows Server 2019, toutes vos données est collectées persistantes et analysées directement sur votre ordinateur, ce qui vous permet de profiter des fonctionnalités d’analytique prédictive sans aucune connectivité de cloud.

Vos données système sont stockées sur votre ordinateur, et ces données sont analysées par les fonctionnalités prédictives qui ne nécessitent pas de REFORMATION dans le cloud. Avec les informations système, vous pouvez conserver vos données sur votre ordinateur et bénéficient toujours des fonctionnalités d’analytique prédictive. 

## <a name="get-started"></a>Prise en main

<iframe src="https://www.youtube-nocookie.com/embed/AJxQkx5WSaA" width="560" height="315" allowfullscreen></iframe>

>[!TIP]
>Regardez ces courtes vidéos pour en savoir plus les informations nécessaires à la prise en main et de gérer en toute confiance des Insights de système : [Bien démarrer avec les informations système dans 10 minutes](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

### <a name="requirements"></a>Configuration requise
Les informations système sont disponibles sur n’importe quelle instance de Windows Server 2019. Il s’exécute sur les machines hôtes et invités, sur un hyperviseur et dans n’importe quel cloud.

### <a name="install-system-insights"></a>Installez le système Insights
>[!IMPORTANT]
>Collecte des informations système et les stocke jusqu'à une année de données localement. Si vous souhaitez conserver vos données lors de la mise à niveau votre système d’exploitation, **Vérifiez que vous utilisez la mise à niveau sur Place**.

#### <a name="install-the-feature"></a>Installer la fonctionnalité
Vous pouvez installer les informations système à l’aide de l’extension dans Windows Admin Center :

![Expérience jour 0 pour l’extension Insights du système.](media/day-0-2.png)

Vous pouvez également installer le système Insights directement via le Gestionnaire de serveur en ajoutant le **système-Insights** fonctionnalité, ou à l’aide de PowerShell :

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>Fournir un commentaire
Nous aimerions avoir votre avis pour nous aider à améliorer cette fonctionnalité. Vous pouvez utiliser les canaux suivants pour envoyer des commentaires :
- **Hub de commentaires**: Utilisez l’outil de Hub de commentaires dans Windows 10 dans le pour fichier d’un bogue ou les commentaires. Ce cas, spécifiez :
    - **Catégorie** : Server 
    - **Sous-catégorie**: Insights système
- **UserVoice**: Soumettre des demandes de fonctionnalités via notre [page UserVoice](https://windowsserver.uservoice.com/forums/295071-management-tools). Partager avec vos collègues à voter pour les éléments qui vous intéressent.
- **Email** : Si vous souhaitez envoyer vos commentaires en privé à l’équipe de fonctionnalité, envoyez un e-mail à system-insights-feed@microsoft.com. Gardez à l’esprit que nous avons toujours demandions vous permettent d’utiliser le Hub de commentaires ou UserVoice.

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur les informations système, utilisez les ressources suivantes :

- [Fonctionnalités de présentation](understanding-capabilities.md)
- [Gérer des fonctionnalités](managing-capabilities.md)
- [Ajout et le développement de fonctionnalités](adding-and-developing-capabilities.md)
- [Système Insights Forum aux questions](faq.md)