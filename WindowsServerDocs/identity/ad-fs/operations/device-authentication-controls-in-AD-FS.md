---
title: "Contrôles des appareils d’authentification dans ADFS"
description: "Ce document décrit comment activer l’authentification des appareils dans ADFS pour Windows Server2016 et 2012R2"
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d66cfde20060229844c34abeea85dd83b802ddad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="device-authentication-controls-in-ad-fs"></a>Contrôles des appareils d’authentification dans ADFS
Le document suivant montre comment activer les contrôles d’authentification de périphériques dans Windows Server2016 et 2012R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Contrôles des appareils d’authentification dans ADFS2012R2
À l’origine dans ADFS2012R2, il y a une propriété d’authentification globale appelée `DeviceAuthenticationEnabled`que l’authentification de périphérique contrôlé.

Pour configurer le paramètre, le `Set-AdfsGlobalAuthenticationPolicy`applet de commande a été utilisée comme indiqué ci-dessous:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Pour désactiver l’authentification des appareils, la même applet de commande a été utilisé pour définir la valeur $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Contrôles des appareils d’authentification dans ADFS2016
Le seul type de l’authentification des appareils pris en charge dans 2012R2 a été clientTLS.  Dans ADFS2016, en plus de clientTLS sont deux nouveaux types d’authentification d’appareil pour l’authentification des appareils modernes.  Il s’agit:
- PKeyAuth
- PRT

Pour contrôler le comportement de nouveau, le `DeviceAuthenticationEnabled`propriété est utilisée conjointement avec une nouvelle propriété nommée `DeviceAuthenticationMethod`.  

La méthode d’authentification appareil détermine le type d’authentification de périphérique sera effectuée: PRT, PKeyAuth, clientTLS, ou une combinaison.
Il présente les valeurs suivantes:
 - SignedToken: PRT uniquement
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS: PRT + clientTLS 
 - Toutes les: Tout le contenu ci-dessus

Comme vous pouvez le constater, PRT fait partie de toutes les méthodes d’authentification de périphériques, ce qui en fait la méthode par défaut qui est toujours activée lors `DeviceAuthenticationEnabled`est défini sur `$true`.

Exemple: Pour configurer les méthodes, utilisez la cmdlet DeviceAuthenticationEnabled comme ci-dessus, ainsi que de la nouvelle propriété:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```
>[!NOTE]
> Activer l’authentification des appareils (paramètre `DeviceAuthenticationEnabled`à `$true`) signifie que le `DeviceAuthenticationMethod`est implicitement défini sur `SignedToken`, ce qui équivaut à **PRT **.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
>[!NOTE]
>La méthode d’authentification du périphérique par défaut est `SignedToken`.  Les autres valeurs sont **PKeyAuth, *** ClientTLS,** et **tous les **.

La signification de la `DeviceAuthenticationMethod`valeurs ont été légèrement modifiés depuis ADFS2016 a été publiée.  Consultez le tableau ci-dessous pour la signification de chaque valeur, en fonction du niveau de mise à jour:


|Version FS de AD|Valeur DeviceAuthenticationMethod|Moyen|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Tous|PRT PkeyAuth + clientTLS|
|2016 RTM + à jour avec Windows Update|SignedToken (pas la même signification)|PRT (uniquement)|
||PkeyAuth (nouveau)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Tous|PRT PkeyAuth + clientTLS|

## <a name="see-also"></a>Voir aussi
[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md)
