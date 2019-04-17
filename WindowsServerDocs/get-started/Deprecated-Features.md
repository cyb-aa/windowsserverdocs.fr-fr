---
title: Fonctionnalités supprimées ou déconseillées dans Windows Server2016
description: Fonctionnalités supprimées ou dont la suppression est prévue dans les prochaines versions.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/02/2017
ms.assetid: 5d10c5f9-ebac-49a0-b808-c0b1702e0437
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 20178a3be14c076623f647fa139e013528de9a69
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133885"
---
# Fonctionnalités supprimées ou déconseillées dans Windows Server2016

>S’applique à Windows Server2016

Voici la liste des fonctionnalités de Windows Server2016 qui ont été supprimées de la version actuelle du produit ou qui vont l’être dans les versions ultérieures (déconseillées). Ce produit s’adresse aux professionnels de l’informatique qui mettent à jour des systèmes d’exploitation dans un environnement commercial. Cette liste peut faire l’objet de modifications dans des versions ultérieures. Il est donc possible que certaines fonctionnalités déconseillées n’y figurent plus. Pour plus d’informations sur une fonctionnalité spécifique et la fonctionnalité qui la remplace, voir la documentation de la fonctionnalité.  

## Fonctionnalités supprimées de Windows Server2016 
Les fonctionnalités suivantes ont été supprimées de cette version de Windows Server2016. À moins que vous n’utilisiez une autre méthode, les applications, le code et les modes d’utilisation qui dépendent de ces fonctionnalités ne seront pas opérationnels dans cette version.  

> [!NOTE]  
> Si vous passez à Windows Server2016 depuis une version antérieure à Windows Server2012 R2 ou Windows Server2012, vous devez également consultez [Fonctionnalités supprimées ou déconseillées dans Windows Server2012 R2](https://technet.microsoft.com/library/dn303411.aspx) et [Fonctionnalités supprimées ou déconseillées dans Windows Server2012](https://technet.microsoft.com/library/hh831568.aspx).  


### Serveur de fichiers  
Le composant logiciel enfichable Gestion du partage et du stockage pour Microsoft Management Console a été supprimé. À la place, procédez de l’une des manières suivantes:  

-   Si l’ordinateur à gérer exécute un système d’exploitation antérieur à Windows Server2016, connectez-vous avec le Bureau à distance et utilisez la version locale du composant logiciel enfichable Gestion du partage et du stockage.  

-   Sur un ordinateur exécutant Windows8.1 ou une version antérieure, utilisez le composant logiciel enfichable Gestion du partage et du stockage des Outils d’administration de serveur distant pour afficher l’ordinateur à gérer.  

-   Utilisez Hyper-V sur un ordinateur client pour exécuter une machine virtuelle exécutant Windows7, Windows8 ou Windows8.1 qui possède le composant logiciel enfichable Gestion du partage et du stockage dans les Outils d’administration de serveur distant.  

### Journal.dll  
Journal.dll est supprimé de Windows Server2016. Il n’existe aucun remplacement.  

### Assistant Configuration de la sécurité  
L’Assistant Configuration de la sécurité est supprimé. En revanche, les fonctionnalités sont sécurisées par défaut. Si vous avez besoin de contrôler des paramètres de sécurité spécifiques, vous pouvez utiliser une stratégie de groupe ou [Microsoft Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx).  

### SQM  
Les composants d’adhésion qui gèrent la participation au programme d’amélioration de l’expérience utilisateur ont été supprimés. 

### WindowsUpdate
La commande **wuauclt.exe /detectnow** a été supprimée et n’est plus prise en charge. Pour déclencher une recherche des mises à jour, effectuez l’une des opérations suivantes:

- Exécutez les commandes PowerShell suivantes:
    ````powershell
    $AutoUpdates = New-Object -ComObject "Microsoft.Update.AutoUpdate"`
    $AutoUpdates.DetectNow()` 
    ````

- Vous pouvez également utiliser le VBScript suivant:
    ````vb
    Set automaticUpdates = CreateObject("Microsoft.Update.AutoUpdate")
    automaticUpdates.DetectNow()
    ````

## Fonctionnalités déconseillées à compter de WindowsServer2016 
Les fonctionnalités suivantes, qui sont déconseillées à compter de cette version, vont être entièrement supprimées du produit. Elles restent disponibles dans cette version, bien qu’amputées parfois de certaines fonctions. Nous vous recommandons dès à présent de faire le nécessaire pour trouver d’autres méthodes à employer si vos applications, votre code ou des modes d’utilisation dépendent de ces fonctionnalités.  

### Outils de configuration  

-   **Scregedit.exe** est déconseillé. Si vous avez des scripts qui dépendent de Scregedit.exe, ajustez-les pour utiliser les méthodes Reg.exe ou Windows PowerShell.  

-   **Sconfig.exe** est déconseillé. Utilisez Windows PowerShell à la place.  

### API personnalisées NetCfg  
L’installation de PrintProvider, NetClient et RNIS à l’aide d’API personnalisées NetCfg est déconseillée.  

### Gestion à distance  
WinRM.vbs est déconseillé. Utilisez plutôt les fonctionnalités du fournisseur WinRM de Windows PowerShell.  

### SMB  
SMB2+ sur NetBT est déconseillé. Implémentez plutôt SMB sur TCP ou RDMA. 
