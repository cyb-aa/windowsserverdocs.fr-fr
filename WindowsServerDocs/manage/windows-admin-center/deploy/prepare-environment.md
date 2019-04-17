---
title: Préparer votre environnement pour Windows Admin Center
description: Préparer votre environnement pour Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 598eeae64925d24ec6d97b59da9cae1e2d10585d
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339587"
---
# Préparer votre environnement pour Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Certaines versions de Server nécessitent une préparation supplémentaire pour être prêtes à gérer avec Windows Admin Center:

- [Windows Server2012 et 2012R2](#prepare-windows-server-2012-and-2012-r2)
- [Windows Server2008R2](#prepare-windows-server-2008-r2)
- [Microsoft Hyper-V Server2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-V Server2012R2](#prepare-microsoft-hyper-v-server-2012-r2)

## Préparer Windows Server2012 et 2012R2

### Installer WMF version5.1 ou ultérieure

Windows Admin Center requiert des fonctionnalités de PowerShell qui ne sont pas incluses par défaut dans Windows Server2012 et 2012R2. Pour gérer Windows Server2012 ou 2012R2 avec Windows Admin Center, vous devez installer WMFversion 5.1 ou ultérieure sur ces serveurs.

Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé et que sa version est5.1 ou ultérieure.

S’il n’est pas installé, vous pouvez [télécharger et installer WMF5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

## Préparer Windows Server2008R2

### Installer WMF version5.1 ou ultérieure

Windows Admin Center requiert des fonctionnalités de PowerShell qui ne sont pas incluses par défaut dans Windows Server2008R2. Pour gérer Windows Server2008R2 avec Windows Admin Center, vous devez installer WMF version5.1 ou ultérieure sur ces serveurs. 

Vérifiez que [.NET Framework 4.5.2 ou version ultérieure](https://docs.microsoft.com/dotnet/framework/install/on-windows-7) est déjà installée sur votre ordinateur.

Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé et que sa version est5.1 ou ultérieure.

S’il n’est pas installé, vous pouvez [télécharger et installer WMF5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

Exécutez `Enable-PSRemoting –force` dans une console PowerShell pour activer la connexion à distance Powershell. 

### Activer le Bureau à distance

Pour utiliser le Bureau à distance dans Windows Admin Center, vous devez activer le Bureau à distance sur votre serveur Windows Server2008R2.

Dans le **Gestionnaire de serveur**, accédez à **Configurer le Bureau à distance**. Activez l'option du Bureau à distance «Autoriser la connexion des ordinateurs exécutant n'importe quelle version de Bureau à distance».

## Préparer Microsoft Hyper-V Server2016

Pour pouvoir gérer Microsoft Hyper-V Server2016 avec Windows Admin Center, vous devez activer certains rôles de serveur.

### Pour gérer Microsoft Hyper-V Server2016 avec Windows Admin Center:

1. Activez la gestion à distance.
2. Activez le rôle de serveur de fichiers.
3. Activer le module Hyper-V pour PowerShell.

### **Étape1:** activer la gestion à distance

Pour activer la gestion à distance dans Hyper-V Server:

1. Connectez-vous à Hyper-V Server.
2. Dans l’outil de **Configuration du serveur** (SCONFIG), tapez **4** pour configurer la gestion à distance.
3. Tapez **1** pour permettre la gestion à distance.
4. Tapez **4** pour revenir au menu principal.

### **Étape 2:** activer le rôle de serveur de fichiers

Pour activer le rôle de serveur de fichiers pour une gestion à distance et un partage de fichiers de base:

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Services de fichiers et de stockage** et cochez **Services de fichiers et iSCSI** et **Serveur de fichiers**:

![](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### **Étape 3:** activer le module Hyper-V pour PowerShell

Pour activer le module Hyper-V pour les fonctionnalités de PowerShell:

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Outils d’administration de serveur distant** et cochez **Outils d’administration de rôles** et **Module Hyper-V pour PowerShell**:

![](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server2016 est prêt pour la gestion avec Windows Admin Center.

## Préparer Microsoft Hyper-V Server2012R2

Pour pouvoir gérer Microsoft Hyper-V Server2012R2 avec Windows Admin Center, vous devez activer certains rôles de serveur.  En outre, vous devez installer WMF version5.1 ou ultérieure.

### Pour gérer Microsoft Hyper-V Server2012R2 avec Windows Admin Center:

1. Installer Windows Management Framework (WMF) version5.1 ou ultérieure
2. Activer la gestion à distance
3. Activer le rôle de serveur de fichiers
4. Activer le module Hyper-V pour PowerShell

### **Étape 1:** installer Windows Management Framework5.1

Windows Admin Center requiert des fonctionnalités de PowerShell qui ne sont pas incluses par défaut dans Microsoft Hyper-V Server2012R2. Pour gérer Microsoft Hyper-V Server2012R2 avec Windows Admin Center, vous devez installer WMF version5.1 ou ultérieure.

Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF EST installé, et que sa version est5.1 ou ultérieure. 

S’il n’est pas installé, vous pouvez [télécharger WMF5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

### **Étape2:** activer la gestion à distance 

Pour activer la gestion à distance d’Hyper-V Server:

1. Connectez-vous à Hyper-V Server.
2. Dans l’outil **Configuration du serveur** (SCONFIG), tapez **4** pour configurer la gestion à distance.
3. Tapez **1** pour permettre la gestion à distance.
4. Tapez **4** pour revenir au menu principal.

### Étape 3: activer le rôle de serveur de fichiers

Pour activer le rôle de serveur de fichiers pour une gestion à distance et un partage de fichiers de base:

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Services de fichiers et de stockage** et cochez **Services de fichiers et iSCSI** et **Serveur de fichiers**:

![](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### Étape 4: activer le module Hyper-V pour PowerShell ##

Pour activer le module Hyper-V pour les fonctionnalités de PowerShell:

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Outils d’administration de serveur distant** et cochez **Outils d’administration de rôles** et **Module Hyper-V pour PowerShell**:

![](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server2012R2 est prêt pour la gestion avec Windows Admin Center.

> [!Tip]
> Prêt à installer Windows Admin Center? [Télécharger maintenant](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)