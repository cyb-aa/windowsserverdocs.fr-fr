---
title: "Configurer les navigateurs pour utiliser l’authentification intégrée de Windows (WIA) avec ADFS"
description: "Ce document décrit comment configurer des navigateurs pour utiliser WIA avec ADFS"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f7d43931d1fe4958a539ff1b728e4cc154d06248
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurer les navigateurs pour utiliser l’authentification intégrée de Windows (WIA) avec ADFS

Par défaut, l’authentification intégrée de Windows (WIA) est activé dans ActiveDirectory Federation Services (ADFS) dans Windows Server2012R2 pour les demandes d’authentification qui se produisent dans le réseau interne de l’entreprise (intranet) pour une application qui utilise un navigateur pour son authentification.

ADFS2016 maintenant a un paramètre par défaut améliorés qui permet le navigateur Edge faire WIA pendant que Windows Phone pas également (de façon incorrecte) interception ainsi:

    =~Windows\s*NT.*Edge

Le moyen ci-dessus, que vous n’avez plus à configurer des chaînes de l’agent utilisateur pour prendre en charge des scénarios courants Edge, même s’ils sont assez souvent mis à jour.

Pour les autres navigateurs, configurer la propriété ADFS **WiaSupportedUserAgents** pour ajouter les valeurs requises selon les navigateurs que vous utilisez.  Vous pouvez utiliser les procédures ci-dessous.



### <a name="view-wiasupporteduseragent-settings"></a>Afficher les paramètres de WIASupportedUserAgent
Le **WIASupportedUserAgents** définit les agents d’utilisateur qui prennent en charge WIA. ADFS analyse la chaîne d’agent utilisateur lors de l’exécution des connexions dans un navigateur ou d’un contrôle de navigateur.

Vous pouvez afficher les paramètres actuels de PowerShell dans l’exemple suivant:

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Prise en charge WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Modifier les paramètres WIASupportedUserAgent
Par défaut, une nouvelle installation d’ADFS possède un jeu de correspondances de chaîne d’agent utilisateur créé. Toutefois, il peut s’agir obsolètes basé sur les modifications apportées aux appareils et navigateurs. En particulier, les appareils Windows possèdent des chaînes d’agent utilisateur similaire avec des variations mineures dans les jetons. L’exemple suivant de Windows PowerShell fournit les meilleurs conseils pour l’ensemble des périphériques qui sont actuellement sur le marché qui prennent en charge WIA transparente actuel:

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

La commande ci-dessus garantit que ADFS couvre uniquement les cas d’utilisation suivants pour WIA:

Agents utilisateurs|Cas d’utilisation|
-----|-----|
MicrosoftINTERNET EXPLORER 6.0|INTERNET EXPLORER 6.0|
MSIE 7.0; windows NT|Internet Explorer 7, Internet Explorer dans la zone intranet. Le fragment de «Windows NT» est envoyé par le système d’exploitation de bureau.|
MSIE 8.0|Internet Explorer 8.0 (aucun périphérique envoyer ces informations, et donc rendre plus spécifique)|
MicrosoftINTERNET EXPLORER 9.0|9.0 Internet Explorer (aucun périphérique envoyer, non nécessaires pour que la plus spécifique)|
MicrosoftINTERNET EXPLORER 10.0; windows NT 6|Version d’Internet Explorer 10.0 pour WindowsXP et versions plus récentes du système d’exploitation</br></br>Les appareils WindowsPhone8.0 (avec la préférence définie pour mobiles) sont exclues, car elles envoient</br></br>Agent utilisateur: Mozilla/5.0 (compatible; microsoft INTERNET EXPLORER 10.0; windowsPhone8.0; trident/6.0; iEMobile/10.0; aRM; tactile; nOKIA; lumia 920)|
WindowsNT6.3; trident/7.0</br></br>WindowsNT6.3; win64; x64; trident/7.0</br></br>WindowsNT6.3; wOW64; trident/7.0| Système d’exploitation Windows8.1Bureau, différentes plateformes|
WindowsNT6.2; trident/7.0</br></br>WindowsNT6.2; win64; x64; trident/7.0</br></br>WindowsNT6.2; wOW64; trident/7.0|Système d’exploitation Windows8 bureau, différentes plateformes|
WindowsNT6.1; trident/7.0</br></br>WindowsNT6.1; win64; x64; trident/7.0</br></br>WindowsNT6.1; wOW64; trident/7.0|Système d’exploitation Windows7 bureau, différents platoforms|
MSIPC| Client de contrôle et de Protection des informations de Microsoft|
Client de gestion des droits Windows|Client de gestion des droits Windows|
