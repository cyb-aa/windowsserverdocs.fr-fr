---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: Prise en charge ADFS de la liaison de l’autre nom d’hôte pour l’authentification par certificat
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e3d1e5d86afbef2fdabd211047f513d31a40300
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190323"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>Prise en charge ADFS de la liaison de l’autre nom d’hôte pour l’authentification par certificat

Sur de nombreux réseaux, les stratégies de pare-feu local ne permettent pas le trafic via des ports non standards comme 49443. Ceci est devenu un problème lorsque vous tentez d’effectuer l’authentification par certificat avec AD FS avant d’AD FS dans Windows Server 2016. Il s’agit, car vous ne pouvez pas avoir des liaisons différentes pour l’authentification des appareils et d’authentification des certificats utilisateur sur le même hôte. Le port par défaut 443 est lié à recevoir des certificats de l’appareil et ne peut pas être modifié pour prendre en charge la liaison de plusieurs dans le même canal. Les résultats ont été que l’authentification de carte à puce ne fonctionne pas et les utilisateurs ont été pas conscients de ce qui est arrivé dans la mesure où il n’existe aucune indication de ce qui s’est vraiment passé.  
  
Cela peut être accompli avec AD FS dans Windows Server 2016.
  
Dans AD FS sur Windows Server 2016, cela a changé. Maintenant nous prenons en charge deux modes, le premier utilise le même hôte (par exemple, adfs.contoso.com) avec des ports différents (443, 49443). Le second utilisé des hôtes différents (adfs.contoso.com et certauth.adfs.contoso.com) avec le même port (443). Cela nécessite un certificat SSL pour prendre en charge « certauth. < adfs-service-name > » en tant qu’un autre nom de sujet. Cela est possible au moment de la création de la batterie de serveurs ou plus tard via PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Comment configurer la liaison de nom d’hôte de remplacement pour l’authentification par certificat  
Il existe deux façons que vous pouvez ajouter la liaison de nom d’hôte de remplacement pour l’authentification par certificat. La première est lorsque vous configurez une nouvelle batterie de serveurs AD FS avec AD FS pour Windows Server 2016, si le certificat contient un autre nom d’objet (SAN), puis elle sera automatiquement configuré pour utiliser la deuxième méthode mentionnée ci-dessus. Autrement dit, il configurera automatiquement deux hôtes différents (sts.contoso.com et certauth.sts.contoso.com avec le même port. Si le certificat ne contient pas d’un réseau SAN, vous verrez un avertissement vous indiquant qu’autres noms d’objet certificat ne prend pas en charge certauth.*. Consultez les captures d’écran ci-dessous. La première affiche une installation où le certificat a un réseau SAN et le deuxième affiche un certificat qui n’ont pas.  
  
![liaison de l’autre nom d’hôte](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![liaison de l’autre nom d’hôte](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
De même, après le déploiement d’AD FS dans Windows Server 2016, vous pouvez utiliser l’applet de commande PowerShell : Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Lorsque vous y êtes invité, cliquez sur Oui pour confirmer.  Et cela devrait l’être.

![liaison de l’autre nom d’hôte](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Références supplémentaires

* [La gestion des certificats SSL dans AD FS et WAP dans Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
