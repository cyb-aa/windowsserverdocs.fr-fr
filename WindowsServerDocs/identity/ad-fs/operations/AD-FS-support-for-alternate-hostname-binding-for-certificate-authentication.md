---
ms.assetid: 4b71b212-7e5b-4fad-81ee-75b3d1f27869
title: "ADFS prend en charge pour la liaison de l’autre nom d’hôte pour l’authentification par certificat"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553ff059693c7b0c0e6f0364d82c1adbca661097
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication"></a>ADFS prend en charge pour la liaison de l’autre nom d’hôte pour l’authentification par certificat

>S’applique à: Windows Server2016

Sur de nombreux réseaux les stratégies de pare-feu locales ne permettent pas de trafic par le biais des ports non standard comme 49443. Cela est devenu un problème lorsque vous tentez d’effectuer l’authentification par certificat avec AD FS avant AD FS dans Windows Server 2016. Il s’agit, car vous pouvez avoir pas les liaisons différentes pour l’authentification des appareils et d’authentification des certificats utilisateur sur le même hôte. Le port par défaut 443 est lié à recevoir des certificats de l’appareil et ne peut pas être modifié pour prendre en charge plusieurs liaison dans le même canal. Les résultats ont été que l’authentification par carte à puce ne fonctionnait pas et que les utilisateurs ont été sans se préoccuper de ce qui est arrivé dans la mesure où il n’existe aucune indication ce qui est réellement arrivé.  
  
Cela peut être accompli avec AD FS dans Windows Server 2016.
  
Dans AD FS sur Windows Server 2016, cela a changé. Maintenant nous prend en charge deux modes, la première utilise le même ordinateur hôte (c'est-à-dire adfs.contoso.com) avec des ports différents (443, 49443). La seconde utilisé différents hôtes (adfs.contoso.com et certauth.adfs.contoso.com) avec le même port (443). Cela nécessite un certificat SSL pour prendre en charge «certauth. < nom de service AD FS >» comme nom de substitution du sujet. Cela est possible au moment de la création de la batterie de serveurs ou version ultérieure via PowerShell.  
  
## <a name="how-to-configure-alternate-host-name-binding-for-certificate-authentication"></a>Comment configurer la liaison de nom de l’autre hôte pour l’authentification par certificat  
Il existe deux façons que vous pouvez ajouter la liaison de nom d’hôte différent pour l’authentification par certificat. La première est lorsque vous configurez une nouvelle batterie AD FS avec AD FS pour Windows Server 2016, si le certificat contient un autre nom d’objet (SAN), puis il sera automatiquement configuré pour utiliser la seconde méthode mentionnée ci-dessus. Autrement dit, il va automatiquement configurer deux hôtes différents (sts.contoso.com et certauth.sts.contoso.com avec le même port. Si le certificat ne contient pas d’un réseau SAN, vous verrez un avertissement vous informant que certificat noms ne prend pas en charge certauth.*. Consultez les captures d’écran ci-dessous. Premier montre une installation où le certificat était un réseau SAN et le deuxième affiche un certificat qui n’a pas.  
  
![liaison d’un autre nom d’hôte](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_1.png)  
  
![liaison d’un autre nom d’hôte](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_2.png)  
  
De même, une fois que les services AD FS dans Windows Server 2016 a été déployé, vous pouvez utiliser l’applet de commande PowerShell: Set-AdfsAlternateTlsClientBinding.
  
```powershell
Set-AdfsAlternateTlsClientBinding -Member DC1.contoso.com -Thumbprint '<thumbprint of cert>'
```

Lorsque vous y êtes invité, cliquez sur Oui pour confirmer.  Et qui doit être il.

![liaison d’un autre nom d’hôte](media/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication/ADFS_CA_3.png)

## <a name="additional-references"></a>Références supplémentaires

* [La gestion des certificats SSL dans AD FS et WAP dans Windows Server 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
