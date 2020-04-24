---
title: Préparer votre environnement pour Windows Admin Center
description: Préparer votre environnement pour Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 7a4dacd611741942e874e831fd9598aeda5e97b3
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "81269276"
---
# <a name="prepare-your-environment-for-windows-admin-center"></a>Préparer votre environnement pour Windows Admin Center

> S'applique à : Windows Admin Center, Windows Admin Center Preview

Certaines versions Server nécessitent une préparation supplémentaire pour pouvoir être gérées avec Windows Admin Center :

- [Windows Server 2012 et 2012 R2](#prepare-windows-server-2012-and-2012-r2)
- [Microsoft Hyper-V Server 2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-V Server 2012 R2](#prepare-microsoft-hyper-v-server-2012-r2)

Il existe également des scénarios dans lesquels il peut être nécessaire de modifier la [configuration du port sur le serveur cible](#port-configuration-on-the-target-server) avant la gestion avec Windows Admin Center.

## <a name="prepare-windows-server-2012-and-2012-r2"></a>Préparer Windows Server 2012 et 2012 R2

### <a name="install-wmf-version-51-or-higher"></a>Installer WMF version 5.1 ou ultérieure

Windows Admin Center nécessite des fonctionnalités de PowerShell qui ne sont pas incluses par défaut dans Windows Server 2012 et 2012 R2. Pour gérer Windows Server 2012 ou 2012 R2 avec Windows Admin Center, vous devez installer WMF version 5.1 ou ultérieure sur ces serveurs.

Tapez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure.

S’il n’est pas installé, vous pouvez [télécharger et installer WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure).

## <a name="prepare-microsoft-hyper-v-server-2016"></a>Préparer Microsoft Hyper-V Server 2016

Pour pouvoir gérer Microsoft Hyper-V Server 2016 avec Windows Admin Center, vous devez activer certains rôles serveur.

### <a name="to-manage-microsoft-hyper-v-server-2016-with-windows-admin-center"></a>Pour gérer Microsoft Hyper-V Server 2016 avec Windows Admin Center

1. Activez l’Administration à distance.
2. Activez le rôle Serveur de fichiers.
3. Activez le module Hyper-V pour PowerShell.

### <a name="step-1-enable-remote-management"></a>**Étape 1 :** Activer l’Administration à distance

Pour activer l’Administration à distance dans Hyper-V Server :

1. Connectez-vous à Hyper-V Server.
2. Dans l’outil de **Configuration du serveur** (SCONFIG), tapez **4** pour configurer l’Administration à distance.
3. Tapez **1** pour activer l’Administration à distance.
4. Tapez **4** pour revenir au menu principal.

### <a name="step-2-enable-file-server-role"></a>**Étape 2 :** Activer le rôle Serveur de fichiers

Pour activer le rôle Serveur de fichiers pour l’Administration à distance et le partage de fichiers de base

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Services de fichiers et de stockage** et cochez **Services de fichiers et iSCSI** et **Serveur de fichiers** :

![Capture d’écran de Rôles et fonctionnalités montrant le rôle Services de fichiers et iSCSI sélectionné](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-3-enable-hyper-v-module-for-powershell"></a>**Étape 3 :** Activer le module Hyper-V pour PowerShell

Pour activer le module Hyper-V pour les fonctionnalités PowerShell

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Outils d’administration de serveur distant** et cochez **Outils d’administration de rôles** et **Module Hyper-V pour PowerShell** :

![Capture d’écran de Rôles et fonctionnalités montrant les rôles Hyper-V sélectionnés](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2016 est prêt pour la gestion avec Windows Admin Center.

## <a name="prepare-microsoft-hyper-v-server-2012-r2"></a>Préparer Microsoft Hyper-V Server 2012 R2

Pour pouvoir gérer Microsoft Hyper-V Server 2012 R2 avec Windows Admin Center, vous devez activer certains rôles serveur.  Vous devez aussi installer WMF version 5.1 ou ultérieure.

### <a name="to-manage-microsoft-hyper-v-server-2012-r2-with-windows-admin-center"></a>Pour gérer Microsoft Hyper-V Server 2012 R2 avec Windows Admin Center

1. Installez Windows Management Framework (WMF) version 5.1 ou ultérieure.
2. Activez l’Administration à distance.
3. Activez le rôle Serveur de fichiers.
4. Activez le module Hyper-V pour PowerShell.

### <a name="step-1-install-windows-management-framework-51"></a>Étape 1 : Installer Windows Management Framework 5.1

Windows Admin Center nécessite des fonctionnalités PowerShell qui ne sont pas incluses par défaut dans Microsoft Hyper-V Server 2012 R2. Pour gérer Microsoft Hyper-V Server 2012 R2 avec Windows Admin Center, vous devez installer WMF version 5.1 ou ultérieure.

Tapez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure. 

S’il n’est pas installé, vous pouvez [télécharger WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure).

### <a name="step-2-enable-remote-management"></a>Étape 2 : Activez l’Administration à distance.

Pour activer l’Administration à distance Hyper-V Server

1. Connectez-vous à Hyper-V Server.
2. Dans l’outil de **Configuration du serveur** (SCONFIG), tapez **4** pour configurer l’Administration à distance.
3. Tapez **1** pour activer l’Administration à distance.
4. Tapez **4** pour revenir au menu principal.

### <a name="step-3-enable-file-server-role"></a>Étape 3 : Activez le rôle Serveur de fichiers.

Pour activer le rôle Serveur de fichiers pour l’Administration à distance et le partage de fichiers de base

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Services de fichiers et de stockage** et cochez **Services de fichiers et iSCSI** et **Serveur de fichiers** :

![Capture d’écran de Rôles et fonctionnalités montrant le rôle Services de fichiers et iSCSI sélectionné](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-4-enable-hyper-v-module-for-powershell"></a>Étape 4 : Activez le module Hyper-V pour PowerShell.

Pour activer le module Hyper-V pour les fonctionnalités PowerShell

1. Cliquez sur **Rôles et fonctionnalités** dans le menu **Outils**.
2. Dans **Rôles et fonctionnalités**, recherchez **Outils d’administration de serveur distant** et cochez **Outils d’administration de rôles** et **Module Hyper-V pour PowerShell** :

![Capture d’écran de Rôles et fonctionnalités montrant les outils d’administration de serveur distant Hyper-V sélectionnés](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2012 R2 est prêt pour la gestion avec Windows Admin Center.

## <a name="port-configuration-on-the-target-server"></a>Configuration du port sur le serveur cible

Windows Admin Center utilise le protocole de partage de fichiers SMB pour certaines tâches de copie de fichiers, par exemple lors de l’importation d’un certificat sur un serveur distant. Pour que ces opérations de copie de fichiers aboutissent, le pare-feu sur le serveur distant doit autoriser les connexions entrantes sur le port 445.  Vous pouvez utiliser l’outil Pare-feu dans Windows Admin Center pour vérifier que la règle entrante « Administration à distance du serveur de fichiers (SMB-In) » est définie de façon à autoriser l’accès sur ce port.

> [!Tip]
> Prêt à installer Windows Admin Center ? [Télécharger maintenant](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)
