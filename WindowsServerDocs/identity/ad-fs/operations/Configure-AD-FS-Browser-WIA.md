---
title: Configurer des navigateurs pour utiliser l’authentification intégrée de Windows (WIA) avec AD FS
description: Ce document décrit la configuration des navigateurs pour utiliser WIA avec AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f71680bb721635bd37197dca9d3ae4726099525f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845480"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurer des navigateurs pour utiliser l’authentification intégrée de Windows (WIA) avec AD FS

Par défaut, l’authentification intégrée de Windows (WIA) est activé dans Active Directory Federation Services (ADFS) dans Windows Server 2012 R2 pour les demandes d’authentification qui se produisent au sein du réseau interne de l’entreprise (intranet) pour toute application qui utilise un navigateur pour son authentification.

AD FS 2016 maintenant a un paramètre par défaut amélioré qui permet au navigateur Edge faire WIA tandis que Windows Phone pas également (de façon incorrecte) l’interception ainsi :

    =~Windows\s*NT.*Edge

Les moyens ci-dessus, que vous n’avez plus à configurer les chaînes d’agent utilisateur individuels pour prendre en charge des scénarios courants de périphérie, même si elles sont souvent mises à jour.

D’autres navigateurs, configurer la propriété AD FS **WiaSupportedUserAgents** pour ajouter les valeurs requises selon les navigateurs que vous utilisez.  Vous pouvez utiliser les procédures ci-dessous.



### <a name="view-wiasupporteduseragent-settings"></a>Afficher les paramètres WIASupportedUserAgent
Le **WIASupportedUserAgents** définit les agents utilisateurs qui prennent en charge WIA. AD FS analyse la chaîne d’agent utilisateur lorsque vous effectuez des connexions dans un navigateur ou d’un contrôle de navigateur.

Vous pouvez afficher les paramètres actuels à l’aide de l’exemple PowerShell suivant :

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Prise en charge WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Modifier les paramètres WIASupportedUserAgent
Par défaut, une nouvelle installation d’AD FS possède un ensemble de correspondances de chaîne d’agent utilisateur créé. Toutefois, il peuvent être obsolètes selon les modifications apportées aux appareils et navigateurs. En particulier, les appareils Windows ont des chaînes d’agent utilisateur similaires avec des variations mineures dans les jetons. L’exemple de Windows PowerShell suivant fournit les meilleurs conseils pour l’ensemble actuel des appareils qui se trouvent sur le marché aujourd'hui qui prennent en charge WIA transparente :

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

La commande ci-dessus garantit que AD FS couvre uniquement les cas d’usage suivant pour WIA :

Agents utilisateurs|Cas d’utilisation|
-----|-----|
MSIE 6.0|IE 6.0|
MSIE 7.0; Windows NT|IE 7, IE dans la zone intranet. Le fragment « Windows NT » est envoyé par le système d’exploitation de bureau.|
MSIE 8.0|Internet Explorer 8.0 (aucun appareil envoyer ce, et donc rendre plus spécifique)|
MSIE 9.0|9.0 IE (aucun appareil envoyer, est donc inutile d’en faire plus spécifique)|
MSIE 10.0; Windows NT 6|10.0 IE pour Windows XP et les versions plus récentes du système d’exploitation</br></br>Les appareils Windows Phone 8.0 (de préférence définie sur mobile) sont exclus car ils envoient</br></br>Agent utilisateur : Mozilla/5.0 (compatible ; MICROSOFT INTERNET EXPLORER 10.0 ; Windows Phone 8.0 ; Trident/6.0 ; IEMobile/10.0 ; ARM ; Pression tactile ; NOKIA ; Lumia 920)|
Windows NT 6.3 ; Trident/7.0</br></br>Windows NT 6.3 ; Win64 ; x64 ; Trident/7.0</br></br>Windows NT 6.3 ; WOW64. Trident/7.0| Windows 8.1 des systèmes d’exploitation, différentes plateformes|
Windows NT 6.2 ; Trident/7.0</br></br>Windows NT 6.2 ; Win64 ; x64 ; Trident/7.0</br></br>Windows NT 6.2 ; WOW64. Trident/7.0|Système d’exploitation bureau Windows 8, différentes plateformes|
Windows NT 6.1 ; Trident/7.0</br></br>Windows NT 6.1 ; Win64 ; x64 ; Trident/7.0</br></br>Windows NT 6.1 ; WOW64. Trident/7.0|Système d’exploitation bureau Windows 7, différentes plateformes|
MSIPC| Client de contrôle et de Protection des informations de Microsoft|
Client Windows Rights Management|Client Windows Rights Management|
