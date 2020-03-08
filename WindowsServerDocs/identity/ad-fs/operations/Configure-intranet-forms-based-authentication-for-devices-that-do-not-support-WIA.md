---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: Configuration de l’authentification basée sur les formulaires intranet pour les appareils qui ne prennent pas en charge WIA
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 09172b3fcfcedf0888205d099409647a6e077577
ms.sourcegitcommit: b5c12007b4c8fdad56076d4827790a79686596af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78856353"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>Configuration de l’authentification basée sur les formulaires intranet pour les appareils qui ne prennent pas en charge WIA


Par défaut, l’authentification intégrée Windows (WIA) est activée dans Services ADFS (AD FS) dans Windows Server 2012 R2 pour les demandes d’authentification qui se produisent au sein du réseau interne de l’organisation (intranet) pour toute application qui utilise un navigateur pour son authentification. Par exemple, il peut s’agir d’applications basées sur un navigateur qui utilisent des protocoles WS-Federation ou SAML et des applications riches qui utilisent le protocole OAuth. WIA offre aux utilisateurs finaux une connexion transparente aux applications sans avoir à entrer manuellement leurs informations d’identification. Toutefois, certains appareils et navigateurs ne peuvent pas prendre en charge WIA et, par conséquent, les demandes d’authentification de ces appareils échouent. En outre, l’expérience de certains navigateurs qui négocient avec NTLM n’est pas souhaitable. L’approche recommandée consiste à revenir à l’authentification basée sur les formulaires pour ces appareils et navigateurs.

AD FS dans Windows Server 2016 et Windows Server 2012 R2 offre aux administrateurs la possibilité de configurer la liste des agents utilisateur qui prennent en charge le secours pour l’authentification basée sur les formulaires. Le secours est rendu possible par deux configurations :


- Propriété **WIASupportedUserAgentStrings** de l’applet de `Set-ADFSProperties`
- Propriété **WindowsIntegratedFallbackEnabled** de l’applet de `Set-AdfsGlobalAuthenticationPolicy`

Le **WIASupportedUserAgentStrings** définit les agents utilisateur qui prennent en charge WIA. AD FS analyse la chaîne de l’agent utilisateur lors de l’exécution de connexions dans un navigateur ou un contrôle de navigateur. Si le composant de la chaîne de l’agent utilisateur ne correspond à aucun des composants des chaînes de l’agent utilisateur qui sont configurées dans la propriété **WIASupportedUserAgentStrings** , AD FS revient à fournir une authentification basée sur les formulaires, à condition que l’indicateur **WindowsIntegratedFallbackEnabled** ait la valeur true.

Par défaut, une nouvelle installation de AD FS a un ensemble de correspondances de chaîne d’agent utilisateur créées. Toutefois, ceux-ci peuvent être obsolètes en fonction des modifications apportées aux navigateurs et aux appareils. En particulier, les appareils Windows ont des chaînes d’agent utilisateur similaires avec des variations mineures dans les jetons. L’exemple Windows PowerShell suivant fournit les meilleures recommandations pour l’ensemble actuel d’appareils qui se trouvent sur le marché aujourd’hui et qui prennent en charge WIA en toute transparence :

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

La commande ci-dessus permet de s’assurer que AD FS couvre uniquement les cas d’utilisation suivants pour WIA :

Agents utilisateur|Cas d’utilisation|
-----|-----|
MSIE 6,0|IE 6,0|
MSIE 7,0 ; Windows NT|IE 7, Internet Explorer dans la zone intranet. Le fragment « Windows NT » est envoyé par le système d’exploitation de bureau.|
MSIE 8,0|IE 8,0 (aucun appareil ne l’envoie ; il doit donc être plus spécifique)|
MSIE 9,0|IE 9,0 (aucun appareil ne l’envoie, il n’est donc pas nécessaire de le rendre plus spécifique)|
MSIE 10,0 ; Windows NT 6|IE 10,0 pour Windows XP et versions plus récentes du système d’exploitation de bureau</br></br>Les appareils Windows Phone 8,0 (avec des préférences définies sur mobile) sont exclus car ils envoient</br></br>User-agent : Mozilla/5.0 (compatible ; MSIE 10,0 ; Windows Phone 8,0 ; Trident/6.0 ; IEMobile/10.0 ; CÂBLES Interface Ericsson Lumia 920)|
Windows NT 6,3 ; Trident/7.0</br></br>Windows NT 6,3 ; Win64 64x Trident/7.0</br></br>Windows NT 6,3 ; WOW64 Trident/7.0| Windows 8.1 système d’exploitation de bureau, différentes plateformes|
Windows NT 6,2 ; Trident/7.0</br></br>Windows NT 6,2 ; Win64 64x Trident/7.0</br></br>Windows NT 6,2 ; WOW64 Trident/7.0|Système d’exploitation Windows 8 Desktop, différentes plateformes|
Windows NT 6,1 ; Trident/7.0</br></br>Windows NT 6,1 ; Win64 64x Trident/7.0</br></br>Windows NT 6,1 ; WOW64 Trident/7.0|Système d’exploitation Windows 7 Desktop, différentes plateformes|
MSIPC| Client Microsoft Information Protection and Control|
Client Windows Rights Management|Client Windows Rights Management|

Pour activer le secours pour l’authentification par formulaire pour les agents utilisateur autres que ceux mentionnés dans la chaîne WIASupportedUserAgents, définissez l’indicateur WindowsIntegratedFallbackEnabled sur true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

Assurez-vous également que l’authentification basée sur les formulaires est activée pour l’intranet.

## <a name="configuring-wia-for-chrome"></a>Configuration de WIA pour Chrome
Vous pouvez ajouter chrome ou d’autres agents utilisateur à la configuration AD FS qui prend en charge WIA. Cela permet une ouverture de session transparente pour les applications sans avoir à entrer manuellement les informations d’identification lorsque vous accédez aux ressources protégées par AD FS. Suivez les étapes ci-dessous pour activer WIA sur chrome :

Dans AD FS configuration, ajoutez une chaîne d’agent utilisateur pour Chrome sur les plateformes Windows :

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Windows NT)")

Et de la même façon pour Chrome sur Apple macOS, ajoutez la chaîne d’agent utilisateur suivante à la configuration AD FS :

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Macintosh; Intel Mac OS X)")

Vérifiez que la chaîne de l’agent utilisateur pour Chrome est maintenant définie dans la AD FS propriétés :

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

(Vous aurez besoin d’une nouvelle capture d’écran ici) ![configurer l’authentification](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> Au fur et à mesure de la publication de nouveaux appareils et navigateurs, il est recommandé de rapprocher les capacités de ces agents utilisateur et de mettre à jour la configuration de AD FS en conséquence afin d’optimiser l’expérience d’authentification de l’utilisateur lors de l’utilisation du navigateur et des appareils. Plus spécifiquement, il est recommandé de réévaluer le paramètre **WIASupportedUserAgents** dans AD FS lors de l’ajout d’un nouveau type d’appareil ou de navigateur à votre matrice de prise en charge pour WIA.

