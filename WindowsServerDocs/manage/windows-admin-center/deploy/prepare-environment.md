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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864970"
---
# <a name="prepare-your-environment-for-windows-admin-center"></a>Préparer votre environnement pour Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Certaines versions de Server nécessitent une préparation supplémentaire pour être prêtes à gérer avec Windows Admin Center :

- [Windows Server 2012 et 2012 R2](#prepare-windows-server-2012-and-2012-r2)
- [Windows Server 2008 R2](#prepare-windows-server-2008-r2)
- [Microsoft Hyper-V Server 2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-V Server 2012 R2](#prepare-microsoft-hyper-v-server-2012-r2)

## <a name="prepare-windows-server-2012-and-2012-r2"></a>Préparer Windows Server 2012 et 2012 R2

### <a name="install-wmf-version-51-or-higher"></a>Installer WMF version 5.1 ou ultérieure

Windows Admin Center requiert des fonctionnalités de PowerShell qui ne sont pas incluses par défaut dans Windows Server 2012 et 2012 R2. Pour gérer Windows Server 2012 ou 2012 R2 avec Windows Admin Center, vous devez installer WMF version 5.1 ou ultérieure sur ces serveurs.

Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure.

S’il n’est pas installé, vous pouvez [télécharger et installer WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

## <a name="prepare-windows-server-2008-r2"></a>Préparer Windows Server 2008 R2

### <a name="install-wmf-version-51-or-higher"></a>Installer WMF version 5.1 ou ultérieure

Windows Admin Center requiert des fonctionnalités de PowerShell qui ne sont pas incluses par défaut dans Windows Server 2008 R2. Pour gérer Windows Server 2008 R2 avec Windows Admin Center, vous devez installer WMF version 5.1 ou ultérieure sur ces serveurs. 

Vérifiez que [.NET Framework 4.5.2 ou version ultérieure](https://docs.microsoft.com/dotnet/framework/install/on-windows-7) est déjà installé sur votre ordinateur.

Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure.

S’il n’est pas installé, vous pouvez [télécharger et installer WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

Exécutez `Enable-PSRemoting –force` dans une console PowerShell pour activer la connexion à distance Powershell. 

### <a name="enable-remote-desktop"></a>Activer le Bureau à distance

Pour utiliser le Bureau à distance dans Windows Admin Center, vous devez activer le Bureau à distance sur votre serveur Windows Server 2008 R2.

Dans le **Gestionnaire de serveur**, accédez à **Configurer le Bureau à distance**. Activez l'option du Bureau à distance « Autoriser la connexion des ordinateurs exécutant n'importe quelle version de Bureau à distance ».

## <a name="prepare-microsoft-hyper-v-server-2016"></a>Préparer Microsoft Hyper-V Server 2016

Pour pouvoir gérer Microsoft Hyper-V Server 2016 avec Windows Admin Center, vous devez activer certains rôles de serveur.

### <a name="to-manage-microsoft-hyper-v-server-2016-with-windows-admin-center"></a>Pour gérer Microsoft Hyper-V Server 2016 avec Windows Admin Center :

1. Activez la gestion à distance.
2. Activez le rôle de serveur de fichiers.
3. Activer le module Hyper-V pour PowerShell.

### <a name="step-1-enable-remote-management"></a>**Étape 1 :** Activer la gestion à distance

Pour activer la gestion à distance dans Hyper-V Server :

1. Connectez-vous à Hyper-V Server.
2. Dans l’outil de **Configuration du serveur** (SCONFIG), tapez **4** pour configurer la gestion à distance.
3. Tapez **1** pour permettre la gestion à distance.
4. Tapez **4** pour revenir au menu principal.

### <a name="step-2-enable-file-server-role"></a>**Étape 2 :** Activer le rôle de serveur de fichiers

Pour activer le rôle de serveur de fichiers pour une gestion à distance et un partage de fichiers de base :

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Services de fichiers et de stockage** et cochez **Services de fichiers et iSCSI** et **Serveur de fichiers** :

![](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-3-enable-hyper-v-module-for-powershell"></a>**Étape 3 :** Activer le module Hyper-V pour PowerShell

Pour activer le module Hyper-V pour les fonctionnalités de PowerShell :

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Outils d’administration de serveur distant** et cochez **Outils d’administration de rôles** et **Module Hyper-V pour PowerShell** :

![](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2016 est prêt pour la gestion avec Windows Admin Center.

## <a name="prepare-microsoft-hyper-v-server-2012-r2"></a>Préparer Microsoft Hyper-V Server 2012 R2

Pour pouvoir gérer Microsoft Hyper-V Server 2012 R2 avec Windows Admin Center, vous devez activer certains rôles de serveur.  En outre, vous devez installer WMF version 5.1 ou ultérieure.

### <a name="to-manage-microsoft-hyper-v-server-2012-r2-with-windows-admin-center"></a>Pour gérer Microsoft Hyper-V Server 2012 R2 avec Windows Admin Center :

1. Installer Windows Management Framework (WMF) version 5.1 ou ultérieure
2. Activer la gestion à distance
3. Activer le rôle de serveur de fichiers
4. Activer le module Hyper-V pour PowerShell

### <a name="step-1-install-windows-management-framework-51"></a>**Étape 1 :** Installez Windows Management Framework 5.1

Windows Admin Center requiert des fonctionnalités de PowerShell qui ne sont pas incluses par défaut dans Microsoft Hyper-V Server 2012 R2. Pour gérer Microsoft Hyper-V Server 2012 R2 avec Windows Admin Center, vous devez installer WMF version 5.1 ou ultérieure.

Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure. 

S’il n’est pas installé, vous pouvez [télécharger WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

### <a name="step-2-enable-remote-management"></a>**Étape 2 :** Activer la gestion à distance 

Pour activer la gestion à distance d’Hyper-V Server :

1. Connectez-vous à Hyper-V Server.
2. Dans l’outil de **Configuration du serveur** (SCONFIG), tapez **4** pour configurer la gestion à distance.
3. Tapez **1** pour permettre la gestion à distance.
4. Tapez **4** pour revenir au menu principal.

### <a name="step-3-enable-file-server-role"></a>Étape 3 : Activer le rôle de serveur de fichiers

Pour activer le rôle de serveur de fichiers pour une gestion à distance et un partage de fichiers de base :

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Services de fichiers et de stockage** et cochez **Services de fichiers et iSCSI** et **Serveur de fichiers** :

![](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-4-enable-hyper-v-module-for-powershell"></a>Étape 4 : Activer le module Hyper-V pour PowerShell ##

Pour activer le module Hyper-V pour les fonctionnalités de PowerShell :

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Outils d’administration de serveur distant** et cochez **Outils d’administration de rôles** et **Module Hyper-V pour PowerShell** :

![](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2012 R2 est prêt pour la gestion avec Windows Admin Center.

> [!Tip]
> Prêt à installer Windows Admin Center ? [Télécharger maintenant](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)