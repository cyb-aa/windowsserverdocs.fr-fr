---
title: Windows Admin Center liés à des solutions de gestion
description: Comment Windows Admin Center compare avec et complète les autres Microsoft analyse et la gestion des solutions/produits (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 385a066cb828f58d698c2ca47e0553e996a77733
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847320"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center et des solutions de gestion liées à partir de Microsoft

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

[Windows Admin Center](windows-admin-center.md) est l’évolution de serveur traditionnel de zone dans les outils de gestion pour les situations où vous avez peut-être utilisé RDP (Remote Desktop) pour se connecter à un serveur pour le dépannage ou de configuration. Il n’est pas destiné à remplacer les autres solutions de gestion Microsoft existantes ; au lieu de cela, il complète ces solutions, comme décrit ci-dessous.

## <a name="remote-server-administration-tools-rsat"></a>Outils d’administration de serveur distant (RSAT)

[Outils d’Administration de serveur distant (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) est un ensemble d’outils de l’interface graphique utilisateur et PowerShell pour gérer les rôles facultatifs et des fonctionnalités dans Windows Server. RSAT comporte de nombreuses fonctionnalités qui n’a pas de Windows Admin Center. Nous pouvons ajouter certains des outils plus couramment utilisés dans RSAT à Windows Admin Center à l’avenir. Une fonctionnalité qui requiert une interface graphique utilisateur pour la gestion ou un nouveau rôle de serveur de Windows sera dans Windows Admin Center.

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) est un service de gestion de mobilité d’entreprise-cloud qui vous permet de gérer des appareils iOS, Android, Windows et macOS, selon un ensemble de stratégies. Intune se concentre sur ce qui vous permet de sécuriser les informations de l’entreprise en contrôlant la façon dont votre personnel y accède et partage les informations. En revanche, Windows Admin Center n’est pas pilotée par des stratégies, mais permet la gestion ad hoc des systèmes Windows 10 et Windows Server, à l’aide de PowerShell et WMI à distance via WinRM.

## <a name="azure-stack"></a>Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) est une plateforme de cloud hybride qui vous permet de fournir des services Azure à partir de votre centre de données. Azure Stack est géré à l’aide de PowerShell ou le portail administrateur, qui est similaire au portail Azure classique utilisé pour accéder et gérer des services Azure traditionnels. Windows Admin Center n’est pas destinée à gérer l’infrastructure Azure Stack, mais vous pouvez l’utiliser pour [gérer des machines virtuelles Azure IaaS](../configure/manage-azure-vms.md) (exécutant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012) ou de dépanner serveurs physiques individuels déployés dans votre environnement Azure Stack.

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) est une solution de gestion de centre de données sur site pour le déploiement, configuration, gestion, surveillance de votre centre de données. System Center vous permet de voir l’état de tous les systèmes dans votre environnement, tandis que Windows Admin Center vous permet d’accéder à un serveur spécifique pour gérer ou de résoudre le problème avec des outils plus précis.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Outils & repensée » dans « plate-forme** | **Surveillance et gestion de centre de données** |
| Inclus avec licence de Windows Server – **sans coût supplémentaire**, comme MMC et d’autres outils de l’emploi traditionnels | **Complète** suite de solutions pour une valeur supplémentaire dans votre environnement et les plateformes |
| **Lightweight**, basée sur navigateur de gestion à distance des instances de Windows Server, **n’importe où**; l’autre pour le protocole RDP | Gérer et surveiller **hétérogènes** systèmes **à grande échelle**, notamment Hyper-V, VMware et Linux |
|**Deep** monoserveur & unique-cluster zoom pour le dépannage, configuration et maintenance|Infrastructure d’approvisionnement ; automatisation et libre-service ;  infrastructure et la surveillance de la charge de travail **transversales**|
|Optimisé la gestion de **individuels** 2 à 4 nœuds **HCL** clusters, l’intégration de Hyper-V, espaces de stockage Direct et SDN|Déployer et gérer Hyper-V, Windows Server clusters à **mise à l’échelle du centre de données** de **un système nu** avec SCVMM|
|**Surveillance de HCL** uniquement ; le service de contrôle d’intégrité de cluster stocke l’historique. Une plate-forme extensible pour le 1er et le 3e partie **extensions de l’outil administrateur**|**Extensible** & **analyse évolutive** plate-forme dans SCOM, avec la génération d’alertes, notifications, par des tiers de charge de travail de surveillance ; SQL pour l’historique|
|Pont le plus simple pour **hybride**; intégrer et utiliser divers services Azure pour la protection des données, la réplication, les mises à jour et bien plus encore|**Intégrés** protection des données, réplication, les mises à jour (DPM/VMM/SCCM). Intégration hybride avec Analytique de journal et Service Map|
|**Fonctionnalités de la plateforme s’allume** de Windows Server : Insights de système de Service de Migration, le réplica de stockage, stockage, etc.|**Plateformes supplémentaires**: Automation dans Orchestrator/SMA. Intégrations avec SCSM & autres outils de gestion de service|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>Chacun d'entre eux de manière indépendante ; propose la valeur ciblée **mieux ensemble** avec des fonctionnalités complémentaires.
