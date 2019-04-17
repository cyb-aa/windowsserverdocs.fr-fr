---
title: Windows Admin Center liées à des solutions de gestion
description: Comment Windows Admin Center compare avec et complète les autres surveillance et la gestion des solutions/produits Microsoft (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7bf0e32b1156fe361c79ac4ccd0e3536df767e2
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296711"
---
# Windows Admin Center et des solutions de gestion associés à partir de Microsoft

>S’applique à: Windows Admin Center, Windows Admin Center Preview

[Windows Admin Center](windows-admin-center.md) est l’évolution des outils de gestion de serveur de boîte aux lettres traditionnel pour les situations où vous avez peut-être utilisé Bureau à distance (RDP) pour vous connecter à un serveur pour la résolution des problèmes ou de configuration. Elle n’a pas vocation à remplacer les autres solutions de gestion Microsoft existantes; au lieu de cela, il complète ces solutions, comme décrit ci-dessous.

## Outils d’administration de serveur distant (RSAT)

[Outils d’Administration de serveur distant (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) est un ensemble d’outils d’interface utilisateur graphique et PowerShell pour gérer des rôles facultatifs et les fonctionnalités de Windows Server. Serveur distant a de nombreuses fonctionnalités qui n’a pas Windows Admin Center. Nous ajouterons par certains des outils plus couramment utilisés dans le serveur distant pour Windows Admin Center à l’avenir. Tout nouveau rôle de serveur Windows ou une fonctionnalité qui nécessite une interface graphique utilisateur pour la gestion sera dans Windows Admin Center.

## Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) est un service de gestion de mobilité entreprise basées sur le cloud qui vous permet de gérer les appareils iOS, Android, Windows et Mac OS, basées sur un ensemble de stratégies. Intune se concentre sur ce qui vous permet de sécuriser les informations de la société en contrôlant comment votre personnel accède à et partage des informations. En revanche, Windows Admin Center n’est pas basées sur la stratégie, mais permet la gestion ad hoc des systèmes Windows 10 et Windows Server, à l’aide à distance PowerShell et WMI sur WinRM.

## Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) est une plateforme cloud hybride qui vous permet de fournir des services Azure à partir de votre centre de données. Azure Stack est géré à l’aide de PowerShell ou le portail de l’administrateur, qui est similaire au portail Azure classique utilisé pour accéder et gérer les services Azure classiques. Windows Admin Center n’est pas destiné à gérer l’infrastructure Azure Stack, mais vous pouvez utiliser pour [gérer des machines virtuelles de Azure IaaS](../azure/manage-azure-vms.md) (exécutant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012) ou résoudre les problèmes physiques individuels serveurs déployés dans votre environnement Azure Stack.

## System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) est une solution de gestion de centre de données local pour le déploiement, la configuration, la gestion, la surveillance de votre centre de données. System Center vous permet d’afficher l’état de tous les systèmes dans votre environnement, tandis que Windows Admin Center vous permet d’Explorer un serveur spécifique pour gérer ou de le résoudre avec les outils plus précis.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Outils de & pensé plateforme «intégrées»** | **Gestion de & du centre de données** |
| Inclus avec la licence Windows Server – **sans coût supplémentaire**, tout comme la console MMC et d’autres outils de boîte aux lettres traditionnels | Suite **complète** de solutions pour la valeur supplémentaire sur votre environnement et les plateformes |
| **Légère**, basée sur navigateur gestion à distance des instances de Windows Server, **n’importe où**; alternative à RDP | Gérer les & moniteur **hétérogènes** systèmes **à l’échelle**, notamment Hyper-V et VMware Linux |
|**Approfondie** & de serveur unique single-cluster exploration vers le bas pour la résolution des problèmes, gestion de la configuration &|Infrastructure de mise en service; automatisation et libre-service;  infrastructure et la charge de travail surveillance **étendue**|
|Optimisées de la gestion de clusters **HCI** nœud **individuel** 2 à 4, intégration Hyper-V, espaces de stockage Direct et SDN|Déployer & gérer Hyper-V, les clusters de Windows Server à **l’échelle du centre de données** à partir du **système nu** avec SCVMM|
|La **surveillance sur HCI** uniquement; service d’intégrité cluster stocke l’historique. Plateforme extensible pour les **extensions d’outil administrateur** 1er et 3e tiers|**Extensible** & plateforme de**surveillance évolutif** dans SCOM, avec des alertes, notifications, tiers de charge de travail surveillance; SQL pour l’historique|
|Pont plus simple à **hybride**; intégrer et utiliser une variété de services Azure pour la protection des données, la réplication, les mises à jour et bien plus encore|Protection des données **intégrée** , la réplication, mises à jour (DPM/VMM/SCCM). Intégration hybride Analytique du journal et la carte de Service|
|**Mette les fonctionnalités de la plateforme** de Windows Server: Service de Migration du stockage, le réplica de stockage, Insights système, etc..|**Plateformes supplémentaires**: Automation dans Orchestrator/SMA. Intégration avec SCSM & autres outils de gestion du service|

#### Chaque offre ciblée valeur indépendamment; **plus efficaces ensemble** avec des fonctionnalités complémentaires.
