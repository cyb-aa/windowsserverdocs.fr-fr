---
title: Configurer AD FS exclue des adresses IP
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 01ef992554a1e0961d8d795e9baa7730a1a1d682
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189890"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS et des adresses IP interdites


En juin 2018, AD FS sur Windows Server 2016 introduit **interdites** avec les services AD FS juin 2018 mises à jour.  Cette mise à jour vous permet de configurer un ensemble d’adresses IP dans le monde entier dans AD FS, afin que les demandes provenant de ces adresses IP, ou qui ont ces adresses IP le **x-forwarded-for** ou **x-ms-transférés-client-ip** en-têtes, sera bloqué par AD FS.

## <a name="adding-banned-ips"></a>Ajout d’adresses IP interdites
Pour ajouter des adresses IP interdite à la liste globale, utilisez l’applet de commande Powershell ci-dessous :

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formats autorisés

1.  IPv4
2.  IPv6
3.  Format CIDR avec IPv4 ou IPv6

Il existe une limite de 300 entrées pour les adresses IP interdites. Vous pouvez utiliser le format CIDR ou de plage pour refuser un grand bloc d’entrées avec une seule entrée.

## <a name="removing-banned-ips"></a>Suppression d’adresses IP interdites
Pour supprimer des adresses IP interdites à partir de la liste globale, utilisez l’applet de commande Powershell ci-dessous :

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>Lecture d’adresses IP interdites
Pour lire l’ensemble actuel des adresses IP interdites, utilisez l’applet de commande Powershell ci-dessous :

``` powershell
PS C:\ >Get-AdfsProperties 
```

Exemple de sortie :

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Références supplémentaires  
[Meilleures pratiques pour la sécurisation d’Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
