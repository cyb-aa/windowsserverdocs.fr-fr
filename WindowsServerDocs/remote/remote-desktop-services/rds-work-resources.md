---
title: Personnaliser le titre « Ressources de travail » des services Bureau à distance à l’aide de PowerShell sur Windows Server
description: Décrit la procédure de remplacement du nom de l’espace de travail par défaut dans Windows Server.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: ea7e1505f705c87e58145e7a306ed355ab3acf7f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71387050"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>Personnaliser le titre « Ressources de travail » des services Bureau à distance à l’aide de PowerShell sur Windows Server

Si vous utilisez Windows Server pour accéder à des programmes RemoteApp ou des postes de travail via l’accès web des services Bureau à distance ou la nouvelle application Bureau à distance, vous avez peut-être remarqué que le titre par défaut de l’espace de travail est « Ressources de travail ».  Vous pouvez facilement changer le titre à l’aide des applets de commande PowerShell.

Pour changer le titre, ouvrez une nouvelle fenêtre PowerShell sur le serveur du service Broker pour les connexions, puis importez le module RemoteDesktop à l’aide de la commande suivante.

```powershell
    Import-Module RemoteDesktop
```

Utilisez ensuite la commande Set-RDWorkspace pour changer le nom de l’espace de travail.

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

Par exemple, vous pouvez utiliser la commande suivante pour remplacer le nom de l’espace de travail par « Programmes RemoteApp Contoso » :

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

Si vous avez plusieurs serveurs du service Broker pour les connexions en mode haute disponibilité, vous devez exécuter cette commande sur le serveur du service broker actif. Vous pouvez utiliser la commande suivante :

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

Pour plus d’informations sur l’applet de commande Set-RDWorkspace, consultez les informations de référence sur [Set-RDSWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps).