---
title: Qu'est-ce que Windows Admin Center?
description: Qu'est-ce que Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d374704768d06aaeb7ee6bc9c984cc78d49cb4b0
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080906"
---
# Qu'est-ce que Windows Admin Center?

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center est un ensemble d’outils de gestion, basé sur un navigateur et déployé localement, qui vous permet d’administrer vos serveurs Windows, sans dépendance à Azure ou au cloud. Windows Admin Center vous donne un contrôle total sur tous les aspects de votre infrastructure de serveurs. Il s’avère particulièrement utile pour la gestion de réseaux privés qui ne sont pas connectés à Internet.

Windows Admin Center est l’évolution moderne des outils de gestion «intégrés» tels que le Gestionnaire de serveur et MMC. Il complète System Center: il n’est pas un remplacement.

![](../media/wac-complements.png)

## Comment fonctionne Windows Admin Center?

Windows Admin Center s’exécute dans un navigateur web et gère Windows Server2016, Windows Server2012R2, Windows Server2012, Windows Server2008R2, Windows10, etc., via la **passerelle Windows Admin Center** installée sur Windows Server2016 ou Windows10. La passerelle gère les serveurs à l’aide de Remote PowerShell et WMI sur WinRM. La passerelle est incluse dans Windows Admin Center dans un package .msi unique léger que vous pouvez [télécharger](https://aka.ms/windowsadmincenter).

Lorsqu'elle est publiée dans DNS et qu'elle dispose d'un accès à travers les pare-feu d'entreprise correspondants, la passerelle Windows Admin Center permet de se connecter en toute sécurité à vos serveurs et de les gérer depuis n’importe où avec Microsoft Edge ou Google Chrome.

![](../media/architecture.png)

## Découvrez comment Windows Admin Center améliore votre environnement de gestion

### **Fonctionnalité familière**

Windows Admin Center est l’évolution de plateformes de gestion connues de longue date, comme la console MMC (Microsoft Management Console). Il est entièrement conçu pour les modes actuels de conception et de gestion des systèmes. Windows Admin Center contient de nombreux outils familiers que vous utilisez actuellement pour gérer les serveurs et les clients Windows.

### **Facile à installer et utiliser**

[Installez-le](../deploy/install.md) sur un ordinateur Windows10 et commencez la gestion en quelques minutes, ou installez-le sur un serveur Windows2016 servant de passerelle pour permettre à toute votre organisation de gérer des ordinateurs à partir d'un navigateur web.

### **Complément de solutions existantes** 

Windows Admin Center fonctionne avec les solutions telles que System Center et Azure gestion et de sécurité, ajoutant à leurs fonctionnalités des tâches détaillées des tâches de gestion d’ordinateur unique.

### **Gérer depuis n’importe où**

Publiez votre serveur de passerelle Windows Admin Center sur le réseau Internet public. Vous pouvez ainsi vous connecter à des serveurs et les gérer depuis n’importe où et ce, de manière sécurisée.

### **Sécurité améliorée pour votre plateforme de gestion**

Windows Admin Center propose de nombreuses améliorations qui rendent votre plateforme de gestion [plus sécurisée](../plan/user-access-options.md). Le contrôle d’accès en fonction du rôle vous permet de choisir avec précision les administrateurs qui ont accès à certaines fonctions de gestion. Les options d’authentification de passerelle sont notamment les groupes locaux, Active Directory basé sur un domaine local et Azure Active Directory en Cloud.  En outre, [obtenez des informations](../use/logging.md) sur les actions de gestion effectuées dans votre environnement.

### **Intégration Azure**

Windows Admin Center offre plusieurs points d' [intégration avec les services Azure](../plan/azure-integration-options.md), y compris Azure Active Directory, sauvegarde Azure, Azure Site Recovery et bien plus encore.

### **Gérer les clusters hyperconvergés**

Windows Admin Center offre la meilleure expérience de [gestion des clusters hyperconvergés](../use/manage-hyper-converged.md), notamment les composants de calcul virtualisé, de stockage et réseau.

### **Extensibilité**

Windows Admin Center a été conçu dès le départ pour permettre l'extensibilité, en offrant aux développeurs Microsoft et tiers la possibilité de créer des outils et des solutions au-delà des offres actuelles. Microsoft propose un [SDK](../extend/extensibility-overview.md) qui permet aux développeurs de créer leurs propres outils pour Windows Admin Center.

> [!Tip]
> Prêt à installer Windows Admin Center? [Télécharger maintenant](https://aka.ms/windowsadmincenter)
