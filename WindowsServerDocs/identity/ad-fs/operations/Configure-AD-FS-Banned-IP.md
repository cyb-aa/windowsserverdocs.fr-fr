---
title: Configurer AD FS adresse IP interdite adresses
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2b518f92f80d06e4bd0854fde94013a412aae515
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407716"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS et adresses IP interdites


En juin 2018, AD FS sur Windows Server 2016 a introduit des **adresses IP interdites** avec la mise à jour AD FS de juin 2018.  Cette mise à jour vous permet de configurer globalement un ensemble d’adresses IP dans AD FS, de sorte que les demandes provenant de ces adresses IP, ou qui ont ces adresses IP dans les en-têtes **x/** **MS-forwarded-client-IP** , sont bloquées par AD FS.

## <a name="adding-banned-ips"></a>Ajout d’adresses IP interdites
Pour ajouter des adresses IP interdites à la liste globale, utilisez l’applet de commande PowerShell ci-dessous :

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formats autorisés

1.  IPv4/IPv6
2.  IPv6
3.  Format CIDR avec IPv4 ou V6

Il existe une limite de 300 entrées pour les adresses IP interdites. Vous pouvez utiliser CIDR ou le format de plage pour refuser un grand bloc d’entrées avec une seule entrée.

## <a name="removing-banned-ips"></a>Suppression des adresses IP interdites
Pour supprimer des adresses IP interdites de la liste globale, utilisez l’applet de commande PowerShell ci-dessous :

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>Lire les adresses IP interdites
Pour lire l’ensemble actuel d’adresses IP interdites, utilisez l’applet de commande PowerShell ci-dessous :

``` powershell
PS C:\ >Get-AdfsProperties 
```

Exemple de sortie :

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Références supplémentaires  
[Meilleures pratiques pour la sécurisation des Services ADFS](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
