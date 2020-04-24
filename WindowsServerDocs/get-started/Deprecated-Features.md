---
title: Fonctionnalités supprimées ou déconseillées dans Windows Server 2016
description: Une liste des fonctionnalités de Windows Server 2016 qui ont été supprimées de la version actuelle du produit ou qui vont l’être dans les versions ultérieures (déconseillées). Ce produit s’adresse aux professionnels de l’informatique qui mettent à jour des systèmes d’exploitation dans un environnement commercial.
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
ms.date: 08/22/2019
ms.assetid: 5d10c5f9-ebac-49a0-b808-c0b1702e0437
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 5e13886395040619a7509c3cf896112288c48115
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "74945208"
---
# <a name="features-removed-or-deprecated-in--windows-server-2016"></a>Fonctionnalités supprimées ou déconseillées dans Windows Server 2016

>S'applique à : Windows Server 2016

Voici la liste des fonctionnalités de Windows Server 2016 qui ont été supprimées de la version actuelle du produit ou qui vont l’être dans les versions ultérieures (déconseillées). Ce produit s’adresse aux professionnels de l’informatique qui mettent à jour des systèmes d’exploitation dans un environnement commercial. Cette liste peut faire l’objet de modifications dans des versions ultérieures. Il est donc possible que certaines fonctionnalités déconseillées n’y figurent plus. Pour plus d’informations sur une fonctionnalité spécifique et la fonctionnalité qui la remplace, voir la documentation de la fonctionnalité.

> [!TIP]
> Pour obtenir des informations sur ce qui a été supprimé ou déconseillé récemment, consultez [Fonctionnalités supprimées ou dont le remplacement est prévu dans Windows Server](../get-started-19/removed-features.md).

## <a name="features-removed-from-windows-server-2016"></a>Fonctionnalités supprimées de Windows Server 2016

Les fonctionnalités suivantes ont été supprimées de cette version de Windows Server 2016. À moins que vous n’utilisiez une autre méthode, les applications, le code et les modes d’utilisation qui dépendent de ces fonctionnalités ne seront pas opérationnels dans cette version.  

> [!NOTE]  
> Si vous passez à Windows Server 2016 depuis une version antérieure à Windows Server 2012 R2 ou Windows Server 2012, vous devez également consultez [Fonctionnalités supprimées ou déconseillées dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx) et [Fonctionnalités supprimées ou déconseillées dans Windows Server 2012](https://technet.microsoft.com/library/hh831568.aspx).  

### <a name="share-and-storage-management"></a>Gestion du partage et du stockage

Le composant logiciel enfichable Gestion du partage et du stockage pour Microsoft Management Console a été supprimé. À la place, procédez de l’une des manières suivantes :  

-   Si l’ordinateur à gérer exécute un système d’exploitation antérieur à Windows Server 2016, connectez-vous avec le Bureau à distance et utilisez la version locale du composant logiciel enfichable Gestion du partage et du stockage.  

-   Sur un ordinateur exécutant Windows 8.1 ou une version antérieure, utilisez le composant logiciel enfichable Gestion du partage et du stockage des Outils d’administration de serveur distant pour afficher l’ordinateur à gérer.  

-   Utilisez Hyper-V sur un ordinateur client pour exécuter une machine virtuelle exécutant Windows 7, Windows 8 ou Windows 8.1 qui possède le composant logiciel enfichable Gestion du partage et du stockage dans les Outils d’administration de serveur distant.  

### <a name="journaldll"></a>Journal.dll

Journal.dll est supprimé de Windows Server 2016. Il n’existe aucun remplacement.  

### <a name="security-configuration-wizard"></a>Assistant Configuration de la sécurité

L’Assistant Configuration de la sécurité est supprimé. En revanche, les fonctionnalités sont sécurisées par défaut. Si vous avez besoin de contrôler des paramètres de sécurité spécifiques, vous pouvez utiliser une stratégie de groupe ou [Microsoft Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx).  

### <a name="sqm"></a>SQM

Les composants d’adhésion qui gèrent la participation au programme d’amélioration de l’expérience utilisateur ont été supprimés. 

### <a name="windows-update"></a>Windows Update

La commande **wuauclt.exe /detectnow** a été supprimée et n’est plus prise en charge. Pour déclencher une recherche des mises à jour, effectuez l’une des opérations suivantes :

- Exécutez ces commandes PowerShell :
    ````powershell
    $AutoUpdates = New-Object -ComObject "Microsoft.Update.AutoUpdate"
    $AutoUpdates.DetectNow()
    ````

- Vous pouvez également utiliser le VBScript suivant :
    ````vb
    Set automaticUpdates = CreateObject("Microsoft.Update.AutoUpdate")
    automaticUpdates.DetectNow()
    ````

## <a name="features-deprecated-starting-with-windows-server-2016"></a>Fonctionnalités déconseillées à compter de Windows Server 2016

Les fonctionnalités suivantes, qui sont déconseillées à compter de cette version, vont être entièrement supprimées du produit. Elles restent disponibles dans cette version, bien qu’amputées parfois de certaines fonctions. Nous vous recommandons dès à présent de faire le nécessaire pour trouver d’autres méthodes à employer si vos applications, votre code ou des modes d’utilisation dépendent de ces fonctionnalités.  

### <a name="configuration-tools"></a>Outils de configuration  

-   **Scregedit.exe** est déconseillé. Si vous avez des scripts qui dépendent de Scregedit.exe, ajustez-les pour utiliser les méthodes Reg.exe ou Windows PowerShell.  

-   **Sconfig.exe** est déconseillé. Utilisez plutôt [Sconfig. cmd](https://docs.microsoft.com/windows-server/get-started/sconfig-on-ws2016). 

### <a name="netcfg-custom-apis"></a>API personnalisées NetCfg

L’installation de PrintProvider, NetClient et RNIS à l’aide d’API personnalisées NetCfg est déconseillée.  

### <a name="remote-management"></a>Gestion à distance  

WinRM.vbs est déconseillé. Utilisez plutôt les fonctionnalités du fournisseur WinRM de Windows PowerShell.  

### <a name="smb"></a>SMB

SMB 2+ sur NetBT est déconseillé. Implémentez plutôt SMB sur TCP ou RDMA. 
