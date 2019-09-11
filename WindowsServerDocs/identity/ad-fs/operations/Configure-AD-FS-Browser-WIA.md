---
title: Configurer des navigateurs pour utiliser l’authentification intégrée Windows (WIA) avec AD FS
description: Ce document décrit comment configurer des navigateurs pour utiliser WIA avec AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c1413e4fa9d86b3c2204b9ed7337389437b93952
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865879"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurer des navigateurs pour utiliser l’authentification intégrée Windows (WIA) avec AD FS

Par défaut, l’authentification intégrée Windows (WIA) est activée dans Services ADFS (AD FS) dans Windows Server 2012 R2 pour les demandes d’authentification qui se produisent au sein du réseau interne de l’organisation (intranet) pour toute application qui utilise un navigateur pour son authentification.

AD FS 2016 a désormais un paramètre par défaut amélioré qui permet à l’Explorateur Edge de faire WIA (WIA) tout en maintenant (de manière incorrecte) l’interception des Windows Phone :

    =~Windows\s*NT.*Edge

La méthode ci-dessus signifie que vous n’avez plus besoin de configurer des chaînes individuelles de l’agent utilisateur pour prendre en charge les scénarios courants, même s’ils sont mis à jour très souvent.

Pour les autres navigateurs, configurez la propriété AD FS **WiaSupportedUserAgents** pour ajouter les valeurs requises en fonction des navigateurs que vous utilisez.  Vous pouvez utiliser les procédures ci-dessous.



### <a name="view-wiasupporteduseragent-settings"></a>Afficher les paramètres WIASupportedUserAgent
Le **WIASupportedUserAgents** définit les agents utilisateur qui prennent en charge WIA. AD FS analyse la chaîne de l’agent utilisateur lors de l’exécution de connexions dans un navigateur ou un contrôle de navigateur.

Vous pouvez afficher les paramètres actuels à l’aide de l’exemple PowerShell suivant :

```powershell
    Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Prise en charge WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Modifier les paramètres WIASupportedUserAgent
Par défaut, une nouvelle installation de AD FS a un ensemble de correspondances de chaîne d’agent utilisateur créées. Toutefois, ceux-ci peuvent être obsolètes en fonction des modifications apportées aux navigateurs et aux appareils. En particulier, les appareils Windows ont des chaînes d’agent utilisateur similaires avec des variations mineures dans les jetons. L’exemple Windows PowerShell suivant fournit les meilleures recommandations pour l’ensemble actuel d’appareils qui se trouvent sur le marché aujourd’hui et qui prennent en charge WIA en toute transparence :

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

La commande ci-dessus permet de s’assurer que AD FS couvre uniquement les cas d’utilisation suivants pour WIA :

Agents utilisateur|Cas d’usage|
-----|-----|
MSIE 6,0|IE 6,0|
MSIE 7,0 ; Windows NT|IE 7, Internet Explorer dans la zone intranet. Le fragment « Windows NT » est envoyé par le système d’exploitation de bureau.|
MSIE 8,0|IE 8,0 (aucun appareil ne l’envoie ; il doit donc être plus spécifique)|
MSIE 9,0|IE 9,0 (aucun appareil ne l’envoie, il n’est donc pas nécessaire de le rendre plus spécifique)|
MSIE 10,0 ; Windows NT 6|IE 10,0 pour Windows XP et versions plus récentes du système d’exploitation de bureau</br></br>Les appareils Windows Phone 8,0 (avec des préférences définies sur mobile) sont exclus car ils envoient</br></br>Agent utilisateur : Mozilla/5.0 (compatible ; MSIE 10,0 ; Windows Phone 8,0 ; Trident/6.0 ; IEMobile/10.0 ; CÂBLES Interface Ericsson Lumia 920)|
Windows NT 6,3 ; Trident/7.0</br></br>Windows NT 6,3 ; Win64 64x Trident/7.0</br></br>Windows NT 6,3 ; WOW64 Trident/7.0| Windows 8.1 système d’exploitation de bureau, différentes plateformes|
Windows NT 6,2 ; Trident/7.0</br></br>Windows NT 6,2 ; Win64 64x Trident/7.0</br></br>Windows NT 6,2 ; WOW64 Trident/7.0|Système d’exploitation Windows 8 Desktop, différentes plateformes|
Windows NT 6,1 ; Trident/7.0</br></br>Windows NT 6,1 ; Win64 64x Trident/7.0</br></br>Windows NT 6,1 ; WOW64 Trident/7.0|Système d’exploitation Windows 7 Desktop, différentes plateformes|
MSIPC| Client Microsoft Information Protection and Control|
Client Windows Rights Management|Client Windows Rights Management|
