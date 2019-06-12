---
title: Qu'est-ce que Windows Admin Center ?
description: Qu'est-ce que Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 99f1a9a32ef69ba8322b2dba902003f8a750a4d2
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811646"
---
# <a name="what-is-windows-admin-center"></a>Qu'est-ce que Windows Admin Center ?

> S’applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Windows Admin Center est un ensemble d’outils de gestion, basé sur un navigateur et déployé localement, qui vous permet d’administrer vos serveurs Windows, sans dépendance à Azure ou au cloud. Windows Admin Center vous donne un contrôle total sur tous les aspects de votre infrastructure de serveurs. Il s’avère particulièrement utile pour la gestion de réseaux privés qui ne sont pas connectés à Internet.

Windows Admin Center est l’évolution moderne des outils de gestion « intégrés » tels que le Gestionnaire de serveur et MMC. Il complète de System Center : il n’est pas un remplacement.

![](../media/wac-complements.png)

## <a name="how-does-windows-admin-center-work"></a>Comment fonctionne Windows Admin Center ?

Windows Admin Center s’exécute dans un navigateur web et gère 2019 de serveur Windows, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 10 et plus encore, via le **passerelle de Windows Admin Center** installé sur Windows Server ou Windows 10. La passerelle gère les serveurs à l’aide de Remote PowerShell et WMI sur WinRM. La passerelle est incluse dans Windows Admin Center dans un package .msi unique léger que vous pouvez [télécharger](https://aka.ms/windowsadmincenter).

Lorsqu'elle est publiée dans DNS et qu'elle dispose d'un accès à travers les pare-feu d'entreprise correspondants, la passerelle Windows Admin Center permet de se connecter en toute sécurité à vos serveurs et de les gérer depuis n’importe où avec Microsoft Edge ou Google Chrome.

![](../media/architecture.png)

## <a name="learn-how-windows-admin-center-improves-your-management-environment"></a>Découvrez comment Windows Admin Center améliore votre environnement de gestion

### <a name="familiar-functionality"></a>**Fonctionnalité familière**

Windows Admin Center est l’évolution de plateformes de gestion connues de longue date, comme la console MMC (Microsoft Management Console). Il est entièrement conçu pour les modes actuels de conception et de gestion des systèmes. Windows Admin Center contient de nombreux outils familiers que vous utilisez actuellement pour gérer les serveurs et les clients Windows.

### <a name="easy-to-install-and-use"></a>**Facile à installer et utiliser**

[Installez-le](../deploy/install.md) sur un ordinateur Windows 10 et commencez la gestion en quelques minutes, ou installez-le sur un serveur Windows 2016 servant de passerelle pour permettre à toute votre organisation de gérer des ordinateurs à partir d'un navigateur web.

### <a name="complements-existing-solutions"></a>**Complément des solutions existantes**

Windows Admin Center fonctionne avec les solutions telles que la gestion de System Center et Azure et la sécurité, ajout à leurs fonctionnalités détaillées, des tâches de gestion d’ordinateur unique.

### <a name="manage-from-anywhere"></a>**Gérer à partir de n’importe où**

Publiez votre serveur de passerelle Windows Admin Center sur le réseau Internet public. Vous pouvez ainsi vous connecter à des serveurs et les gérer depuis n’importe où et ce, de manière sécurisée.

### <a name="enhanced-security-for-your-management-platform"></a>**Sécurité renforcée pour votre plateforme de gestion**

Windows Admin Center propose de nombreuses améliorations qui rendent votre plateforme de gestion [plus sécurisée](../plan/user-access-options.md). Le contrôle d’accès en fonction du rôle vous permet de choisir avec précision les administrateurs qui ont accès à certaines fonctions de gestion. Les options d’authentification de passerelle sont notamment les groupes locaux, Active Directory basé sur un domaine local et Azure Active Directory en Cloud.  En outre, [obtenez des informations](../use/logging.md) sur les actions de gestion effectuées dans votre environnement.

### <a name="azure-integration"></a>**Intégration d’Azure**

Windows Admin Center a de nombreux points de [intégration avec les services Azure](../plan/azure-integration-options.md), notamment Azure Active Directory, sauvegarde Azure, Azure Site Recovery et bien plus encore.

### <a name="manage-hyper-converged-clusters"></a>**Gérer les clusters hyperconvergés**

Windows Admin Center offre la meilleure expérience de [gestion des clusters hyperconvergés](../use/manage-hyper-converged.md), notamment les composants de calcul virtualisé, de stockage et réseau.

### <a name="extensibility"></a>**Extensibilité**

Windows Admin Center a été conçu dès le départ pour permettre l'extensibilité, en offrant aux développeurs Microsoft et tiers la possibilité de créer des outils et des solutions au-delà des offres actuelles. Microsoft propose un [SDK](../extend/extensibility-overview.md) qui permet aux développeurs de créer leurs propres outils pour Windows Admin Center.

> [!Tip]
> Prêt à installer Windows Admin Center ? [Télécharger maintenant](https://aka.ms/windowsadmincenter)
