---
title: Personnaliser le titre des Services Bureau à distance « ressources de travail » à l'aide de PowerShell sur Windows Server
description: Décrit comment modifier le nom de l’espace de travail à partir de la valeur par défaut dans Windows Server.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 43837826a6cddc2c3c4c7c1af874334718a3a067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826710"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>Personnaliser le titre des Services Bureau à distance « ressources de travail » à l'aide de PowerShell sur Windows Server

Lorsque vous utilisez Windows Server pour accéder à RemoteApps ou des postes de travail via WebAccess du Bureau à distance ou de la nouvelle application de bureau à distance, vous avez peut-être remarqué que l’espace de travail est intitulé « Ressources de travail » par défaut.  Vous pouvez facilement modifier le titre à l’aide des applets de commande PowerShell.

Pour modifier le titre, ouvrez une nouvelle fenêtre PowerShell sur le serveur du service broker et importer le module RemoteDesktop avec la commande suivante.

```powershell
    Import-Module RemoteDesktop
```

Ensuite, utilisez la commande Set-RDWorkspace pour modifier le nom de l’espace de travail.

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

Par exemple, vous pouvez utiliser la commande suivante pour modifier le nom de l’espace de travail pour « Contoso RemoteApps » :

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

Si vous exécutez plusieurs agents de connexion en mode haute disponibilité, vous devez exécuter cela sur le service broker actif. Vous pouvez utiliser cette commande :

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

Pour plus d’informations sur l’applet de commande Set-RDWorkspace, consultez le [Set-RDSWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps) référence.