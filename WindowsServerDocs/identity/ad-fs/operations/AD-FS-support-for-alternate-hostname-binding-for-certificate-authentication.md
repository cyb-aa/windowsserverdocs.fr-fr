---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: Prise en charge ADFS de la liaison de l’autre nom d’hôte pour l’authentification par certificat
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e3764977e29413ea1e361fa78cadd040adabcf04
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858092"
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>Prise en charge ADFS de la liaison de l’autre nom d’hôte pour l’authentification par certificat

Sur de nombreux réseaux, les stratégies de pare-feu local peuvent ne pas autoriser le trafic via des ports non standard tels que 49443. Ce problème est apparu lors de la tentative d’authentification par certificat avec AD FS avant AD FS dans Windows Server 2016. Cela est dû au fait que vous ne pouviez pas avoir des liaisons différentes pour l’authentification des appareils et l’authentification des certificats utilisateur sur le même hôte. Le port par défaut 443 est lié à la réception des certificats d’appareil et ne peut pas être modifié pour prendre en charge plusieurs liaisons dans le même canal. Les résultats étaient que l’authentification par carte à puce ne fonctionnait pas et que les utilisateurs ne pouvaient pas savoir ce qui s’est passé, car il n’existe aucune indication sur ce qui s’est réellement produit.  
  
Avec AD FS dans Windows Server 2016, cela peut être accompli.
  
Dans AD FS sur Windows Server 2016, cela a changé. Maintenant que nous prenons en charge deux modes, le premier utilise le même hôte (c.-à-d. adfs.contoso.com) avec des ports différents (443, 49443). Le deuxième utilise des hôtes différents (adfs.contoso.com et certauth.adfs.contoso.com) avec le même port (443). Cela nécessite un certificat SSL pour prendre en charge « certauth. < ADFS-Service-Name > » comme autre nom de sujet. Cette opération peut être effectuée au moment de la création de la batterie de serveurs ou ultérieurement via PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Comment configurer une autre liaison de nom d’hôte pour l’authentification par certificat  
Il existe deux façons d’ajouter l’autre liaison de nom d’hôte pour l’authentification par certificat. La première est lors de la configuration d’une nouvelle batterie de serveurs AD FS avec AD FS pour Windows Server 2016, si le certificat contient un autre nom de l’objet (SAN), alors il sera automatiquement configuré pour utiliser la deuxième méthode mentionnée ci-dessus. Autrement dit, il configure automatiquement deux hôtes différents (sts.contoso.com et certauth.sts.contoso.com avec le même port. Si le certificat ne contient pas de réseau SAN, vous verrez un avertissement vous informant que les autres noms de l’objet du certificat ne prennent pas en charge certauth. *. Consultez les captures d’écran ci-dessous. La première affiche une installation où le certificat a un réseau SAN et le second un certificat qui n’a pas été utilisé.  
  
![autre liaison de nom d’hôte](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![autre liaison de nom d’hôte](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
De même, une fois que AD FS dans Windows Server 2016 a été déployé, vous pouvez utiliser l’applet de commande PowerShell : Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Lorsque vous y êtes invité, cliquez sur Oui pour confirmer.  Et c’est le cas.

![autre liaison de nom d’hôte](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Références supplémentaires

* [Gestion des certificats SSL dans AD FS et WAP dans Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
