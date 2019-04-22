---
title: Contrôles de l’authentification d’appareil dans AD FS
description: Ce document décrit comment activer l’authentification des appareils dans AD FS pour Windows Server 2016 et 2012 R2
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d66cfde20060229844c34abeea85dd83b802ddad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822820"
---
# <a name="device-authentication-controls-in-ad-fs"></a>Contrôles de l’authentification d’appareil dans AD FS
Le document suivant montre comment activer les contrôles d’authentification de périphériques dans Windows Server 2016 et 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Contrôles de l’authentification d’appareil dans AD FS 2012 R2
À l’origine dans AD FS 2012 R2, il y a une propriété d’authentification globale appelée `DeviceAuthenticationEnabled` que l’authentification de périphérique contrôlé.

Pour configurer le paramètre, le `Set-AdfsGlobalAuthenticationPolicy` applet de commande a été utilisée comme indiqué ci-dessous :


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Pour désactiver l’authentification des appareils, l’applet de commande a été utilisé pour définir la valeur sur $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Contrôles de l’authentification d’appareil dans AD FS 2016
Le seul type de l’authentification des appareils pris en charge dans 2012 R2 a été clientTLS.  Dans AD FS 2016, en plus de clientTLS sont deux nouveaux types de l’authentification des appareils pour l’authentification des appareils récents.  Ces équivalents sont :
- PKeyAuth
- PRT

Pour contrôler le comportement de nouveau, le `DeviceAuthenticationEnabled` propriété est utilisée en association avec une nouvelle propriété nommée `DeviceAuthenticationMethod`.  

La méthode d’authentification d’appareil détermine le type de l’authentification des appareils qui est effectuée : PRT, PKeyAuth, clientTLS ou une combinaison des deux.
Il possède les valeurs suivantes :
 - SignedToken: PRT uniquement
 - PKeyAuth : PRT + PKeyAuth
 - ClientTLS : PRT + clientTLS 
 - All : Tout le contenu ci-dessus

Comme vous pouvez le voir, PRT est partie de toutes les méthodes d’authentification de périphérique, ce qui en fait la méthode par défaut qui est toujours activée lorsque `DeviceAuthenticationEnabled` est défini sur `$true`.

Exemple : Pour configurer le (s), utilisez la cmdlet DeviceAuthenticationEnabled comme ci-dessus, ainsi que de la nouvelle propriété :

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```
>[!NOTE]
> L’activation de l’authentification des appareils (paramètre `DeviceAuthenticationEnabled` à `$true`) signifie que le `DeviceAuthenticationMethod` a implicitement la valeur `SignedToken`, ce qui équivaut à **PRT**.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
>[!NOTE]
>La méthode d’authentification d’appareil par défaut est `SignedToken`.  Les autres valeurs sont **PKeyAuth, *** ClientTLS,** et **tous les**.

La signification de la `DeviceAuthenticationMethod` valeurs ont été légèrement modifiés depuis la publication de services AD FS 2016.  Consultez le tableau ci-dessous pour la signification de chaque valeur, en fonction du niveau de mise à jour :


|Version de AD FS|Valeur de DeviceAuthenticationMethod|Moyen|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Tous|PRT + PkeyAuth + clientTLS|
|2016 RTM + mise à jour avec Windows Update|SignedToken (c'est-à-dire modifiée)|PRT (uniquement)|
||PkeyAuth (nouveau)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Tous|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>Voir aussi
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
