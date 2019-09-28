---
title: Qu’est-ce que Windows Admin Center ?
description: Qu’est-ce que Windows Admin Center ? (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: 110e1442b6660c24dc1e3fd9649138390117ffd6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406814"
---
# <a name="what-is-windows-admin-center"></a>Qu’est-ce que Windows Admin Center ?

> S’applique à : Windows Admin Center, Windows Admin Center Preview

Windows Admin Center est un nouvel ensemble d’outils de gestion, basé sur un navigateur et déployé localement, qui vous permet d’administrer vos serveurs Windows, sans dépendance à Azure ni au cloud. Windows Admin Center vous donne un contrôle total sur tous les aspects de votre infrastructure de serveurs. Il s’avère particulièrement utile pour la gestion des serveurs sur les réseaux privés qui ne sont pas connectés à Internet.

Windows Admin Center est l’évolution moderne des outils de gestion « intégrés » tels que le Gestionnaire de serveur et MMC. Il vient compléter System Center et non le remplacer.

![](../media/wac-complements.png)

## <a name="how-does-windows-admin-center-work"></a>Comment fonctionne Windows Admin Center ?

Windows Admin Center s’exécute dans un navigateur web et gère Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 10 etc., via la **passerelle Windows Admin Center** installée sur Windows Server ou Windows 10. La passerelle gère les serveurs à l’aide de Remote PowerShell et WMI sur WinRM. La passerelle est incluse dans Windows Admin Center dans un package .msi unique léger que vous pouvez [télécharger](https://aka.ms/windowsadmincenter).

Lorsqu’elle est publiée dans DNS et qu’elle dispose d’un accès à travers les pare-feu d’entreprise correspondants, la passerelle Windows Admin Center vous permet de vous connecter en toute sécurité à vos serveurs et de les gérer depuis n’importe où avec Microsoft Edge ou Google Chrome.

![](../media/architecture.png)

## <a name="learn-how-windows-admin-center-improves-your-management-environment"></a>Découvrez comment Windows Admin Center améliore votre environnement de gestion

### <a name="familiar-functionality"></a>**Fonctionnalité familière**

Windows Admin Center est l’évolution de plateformes de gestion connues de longue date, comme la console MMC (Microsoft Management Console). Il est entièrement conçu pour les modes actuels de conception et de gestion des systèmes. Windows Admin Center contient de nombreux outils familiers que vous utilisez actuellement pour gérer les serveurs et les clients Windows.

### <a name="easy-to-install-and-use"></a>**Facile à installer et utiliser**

[Installez-le](../deploy/install.md) sur un ordinateur Windows 10 et commencez la gestion en quelques minutes, ou installez-le sur un serveur Windows 2016 servant de passerelle pour permettre à toute votre organisation de gérer des ordinateurs à partir d’un navigateur web.

### <a name="complements-existing-solutions"></a>**Complément de solutions existantes**

Windows Admin Center fonctionne avec des solutions telles que System Center et la gestion et la sécurité Azure, en ajoutant à leurs fonctionnalités des tâches détaillées de gestion d’ordinateur unique.

### <a name="manage-from-anywhere"></a>**Gestion en tout lieu**

Publiez votre serveur de passerelle Windows Admin Center sur le réseau Internet public. Vous pouvez ainsi vous connecter à des serveurs et les gérer depuis n’importe où et ce, de manière sécurisée.

### <a name="enhanced-security-for-your-management-platform"></a>**Sécurité améliorée pour votre plateforme de gestion**

Windows Admin Center propose de nombreuses améliorations qui [renforcent la sécurité](../plan/user-access-options.md) de votre plateforme de gestion. Le contrôle d’accès en fonction du rôle vous permet de choisir avec précision les administrateurs qui ont accès à certaines fonctions de gestion. Les options d’authentification de passerelle incluent notamment les groupes locaux, Active Directory basé sur un domaine local et Azure Active Directory basé sur le cloud.  En outre, [obtenez des informations](../use/logging.md) sur les actions de gestion effectuées dans votre environnement.

### <a name="azure-integration"></a>**Intégration Azure**

Windows Admin Center offre plusieurs points [d’intégration avec les services Azure](../plan/azure-integration-options.md), notamment Azure Active Directory, Sauvegarde Azure, Azure Site Recovery, etc.

### <a name="manage-hyper-converged-clusters"></a>**Gestion des clusters hyperconvergés**

Windows Admin Center offre la meilleure expérience utilisateur de [gestion des clusters hyperconvergés](../use/manage-hyper-converged.md), notamment les composants de calcul virtualisé, de stockage et réseau.

### <a name="extensibility"></a>**Extensibilité**

Windows Admin Center a été conçu dès le départ pour permettre l’extensibilité, en offrant aux développeurs Microsoft et tiers la possibilité de créer des outils et des solutions au-delà des offres actuelles. Microsoft propose un [Kit de développement logiciel (SDK)](../extend/extensibility-overview.md) qui permet aux développeurs de créer leurs propres outils pour Windows Admin Center.

> [!Tip]
> Prêt à installer Windows Admin Center ? [Télécharger maintenant](https://aka.ms/windowsadmincenter)
