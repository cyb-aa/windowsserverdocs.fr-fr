---
title: Solutions d’administration liées à Windows Admin Center
description: Comparez Windows Admin Center avec les autres solutions/produits d’administration et de supervision de Microsoft et découvrez en quoi ils sont complémentaires (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d681e5007cd3ae3c14de774df0bc85abc23b51d7
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323531"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center et autres solutions d’administration de Microsoft

>S'applique à : Windows Admin Center, Windows Admin Center Preview

[Windows Admin Center](windows-admin-center.md) est l’évolution des outils d’administration de serveur existants, fournis pour les situations où vous deviez utiliser le protocole RDP (Remote Desktop Protocol) pour vous connecter à un serveur à des fins de dépannage ou de configuration. Cette solution n’a pas vocation à remplacer les autres solutions d’administration existantes proposées par Microsoft. Elle vient en complément de ces solutions, comme cela est décrit ci-dessous.

## <a name="remote-server-administration-tools-rsat"></a>Outils d’administration de serveur distant (RSAT)

Les [Outils d’administration de serveur distant (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) constituent un ensemble d’outils GUI et PowerShell qui permettent de gérer les rôles et fonctionnalités facultatifs dans Windows Server. RSAT présente beaucoup de capacités que n’a pas Windows Admin Center. Nous ajouterons peut-être certains des outils les plus couramment utilisés dans RSAT à Windows Admin Center dans une version future. Les nouvelles fonctionnalités ou nouveaux rôles Windows Server dont la gestion nécessite une interface graphique utilisateur seront inclus dans Windows Admin Center.

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) est un service cloud de gestion d’Enterprise Mobility qui vous permet de gérer des appareils iOS, Android, Windows et macOS en utilisant un ensemble de stratégies. L’une des priorités d’Intune est de vous permettre de sécuriser les informations de l’entreprise en contrôlant la façon dont le personnel partage les informations et y accède. En revanche, Windows Admin Center n’est pas basé sur des stratégies. Il permet une administration ad hoc des systèmes Windows 10 et Windows Server en utilisant PowerShell et WMI à distance par le biais de WinRM.

## <a name="azure-stack"></a>Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) est une plateforme cloud hybride sur laquelle vous pouvez fournir des services Azure de votre centre de données. L’administration d’Azure Stack s’effectue à l’aide de PowerShell ou du portail de l’administrateur, qui est similaire au portail Azure classique utilisé pour administrer les services Azure standard et y accéder. Windows Admin Center n’a pas été conçu pour l’administration de l’infrastructure Azure Stack, mais vous pouvez l’utiliser pour [gérer des machines virtuelles Azure IaaS](../azure/manage-azure-vms.md) (sur Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012) ou pour résoudre les problèmes avec les serveurs physiques individuels déployés dans votre environnement Azure Stack.

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) est une solution de gestion de centre de données en local qui vous permet de déployer, configurer, gérer et superviser votre centre de données dans son intégralité. Avec System Center, vous supervisez l’état de tous les systèmes installés dans votre environnement, alors qu’avec Windows Admin Center, vous avez une vue détaillée de chaque serveur, que vous pouvez ainsi administrer et dépanner à l’aide d’outils granulaires.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Outils et plateforme fournis repensés** | **Supervision et gestion de centre de données** |
| Inclus avec la licence Windows Server, **sans coût supplémentaire**, comme MMC et d’autres outils fournis standard | Suite de solutions **complète** qui apporte des capacités supplémentaires à votre environnement et vos plateformes |
| Solution **légère**, basée sur navigateur, qui permet de gérer à distance, **de n’importe où**, les instances Windows Server. Autre solution possible à la place du protocole RDP | Gestion et supervision de systèmes **hétérogènes** **à grande échelle**, comme Hyper-V, VMware et Linux |
|Administration **granulaire** de chaque serveur et chaque cluster pour le dépannage, la configuration et la maintenance|Provisionnement de l’infrastructure ; automatisation et libre-service ; supervision de l’infrastructure et des charges de travail sur l’**étendue**|
|Gestion optimisée des clusters **individuels** **HCI** de deux à quatre nœuds ; intégration d’Hyper-V, de Storage Spaces Direct et de SDN|Déploiement et gestion de clusters Windows Server Hyper-V à l’**échelle du centre de données** à partir du **système nu** avec SCVMM|
|**Supervision sur HCI** uniquement ; historique des magasins du service de contrôle d’intégrité des clusters. Plateforme extensible pour les première et troisième **extensions des outils d’administration** tiers|Plateforme de **supervision scalable** & **extensible** dans SCOM, avec des alertes, des notifications, une solution tierce de supervision des charges de travail ; SQL pour l’historique|
|Voie la plus simple vers un environnement **hybride** ; intégration et utilisation de divers services Azure pour la protection des données, la réplication, les mises à jour et bien plus encore|**Intégration** de la protection des données, de la réplication et des mises à jour (DPM/VMM/SCCM). Intégration hybride avec Log Analytics et Service Map|
|**Mise en avant des fonctionnalités de la plateforme** Windows Server : Service de migration de stockage, Réplica de stockage, Insights système, etc.|**Plateformes supplémentaires** : Automatisation dans Orchestrator/SMA. Intégrations avec SCSM et d’autres outils de gestion de service|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>Chacune de ces solutions offre ses propres avantages. **Utilisez-les conjointement** pour bénéficier de capacités complémentaires.
