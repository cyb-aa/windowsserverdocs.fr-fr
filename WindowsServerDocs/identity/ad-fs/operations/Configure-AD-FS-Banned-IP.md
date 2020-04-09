---
title: Configurer AD FS adresse IP interdite adresses
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1a1e8a9e668caa0c766f6fe3012d5ae6ecaddb50
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859922"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS et adresses IP interdites


En juin 2018, AD FS sur Windows Server 2016 a introduit des **adresses IP interdites** avec la mise à jour AD FS de juin 2018.  Cette mise à jour vous permet de configurer globalement un ensemble d’adresses IP dans AD FS, de sorte que les demandes provenant de ces adresses IP, ou qui ont ces adresses IP dans les en-têtes **x/** **MS-forwarded-client-IP** , sont bloquées par AD FS.

## <a name="adding-banned-ips"></a>Ajout d’adresses IP interdites
Pour ajouter des adresses IP interdites à la liste globale, utilisez l’applet de commande PowerShell ci-dessous :

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formats autorisés

1.  IPv4
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
