---
title: Contrôles d’authentification des appareils dans AD FS
description: Ce document décrit comment activer l’authentification des appareils dans AD FS pour Windows Server 2016 et 2012 R2
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 87c011b18ad4a1d464072c1ea90b09a44e831378
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407364"
---
# <a name="device-authentication-controls-in-ad-fs"></a>Contrôles d’authentification des appareils dans AD FS
Le document suivant montre comment activer les contrôles d’authentification des appareils dans Windows Server 2016 et 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Contrôles d’authentification des appareils dans AD FS 2012 R2
À l’origine dans AD FS 2012 R2, il existait une propriété d’authentification globale appelée `DeviceAuthenticationEnabled` qui contrôlait l’authentification des appareils.

Pour configurer le paramètre, l’applet de commande `Set-AdfsGlobalAuthenticationPolicy` a été utilisée comme indiqué ci-dessous :


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Pour désactiver l’authentification des appareils, la même applet de commande a été utilisée pour définir la valeur sur $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Contrôles d’authentification des appareils dans AD FS 2016
Le seul type d’authentification d’appareil pris en charge dans 2012 R2 était clientTLS.  Dans AD FS 2016, en plus de clientTLS, il existe deux nouveaux types d’authentification d’appareil pour l’authentification des appareils modernes.  Ces équivalents sont :
- PKeyAuth
- PRT

Pour contrôler le nouveau comportement, la propriété `DeviceAuthenticationEnabled` est utilisée en association avec une nouvelle propriété appelée `DeviceAuthenticationMethod`.  

La méthode d’authentification de l’appareil détermine le type d’authentification des appareils qui sera effectué : PRT, PKeyAuth, clientTLS ou une combinaison.
Il a les valeurs suivantes :
 - SignedToken: PRT uniquement
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS PRT + clientTLS
 - All : Tout le contenu ci-dessus

Comme vous pouvez le voir, PRT fait partie de toutes les méthodes d’authentification de l’appareil, ce qui en fait la méthode par défaut qui est toujours activée lorsque `DeviceAuthenticationEnabled` est défini sur `$true`.

Exemple : Pour configurer la ou les méthodes, utilisez l’applet de commande DeviceAuthenticationEnabled comme indiqué ci-dessus, ainsi que la nouvelle propriété :

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> Dans ADFS 2019, `DeviceAuthenticationMethod` peut être utilisé avec la commande `Set-AdfsRelyingPartyTrust`.

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> L’activation de l’authentification des appareils (paramètre `DeviceAuthenticationEnabled` à `$true`) signifie que le `DeviceAuthenticationMethod` est implicitement défini sur `SignedToken`, ce qui équivaut à **PRT**.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> La méthode d’authentification par défaut de l’appareil est `SignedToken`.  Les autres valeurs sont **PKeyAuth,** <strong>ClientTLS</strong> et **All**.

La signification des valeurs `DeviceAuthenticationMethod` a légèrement changé depuis la sortie de AD FS 2016.  Consultez le tableau ci-dessous pour connaître la signification de chaque valeur, en fonction du niveau de mise à jour :


|Version de AD FS|Valeur DeviceAuthenticationMethod|Cela|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Tous|PRT + PkeyAuth + clientTLS|
|2016 RTM + à jour avec Windows Update|SignedToken (signification modifiée)|PRT (uniquement)|
||PkeyAuth (nouveau)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Tous|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>Voir aussi
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
