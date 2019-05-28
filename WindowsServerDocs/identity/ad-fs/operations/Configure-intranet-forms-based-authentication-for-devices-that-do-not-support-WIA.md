---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: Configuration de l’authentification basée sur les formulaires intranet pour les appareils qui ne prennent pas en charge WIA
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c79524a011336d676fa2e80936e1254a8d2dd6b2
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189689"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>Configuration de l’authentification basée sur les formulaires intranet pour les appareils qui ne prennent pas en charge WIA


Par défaut, l’authentification intégrée de Windows (WIA) est activé dans Active Directory Federation Services (ADFS) dans Windows Server 2012 R2 pour les demandes d’authentification qui se produisent au sein du réseau interne de l’entreprise (intranet) pour toute application qui utilise un navigateur pour son authentification. Par exemple, il peuvent être basée sur navigateur les applications qui utilisent WS-Federation ou SAML protocoles et des applications riches qui utilisent le protocole OAuth. WIA offre aux utilisateurs finaux d’ouverture de session transparente pour les applications sans avoir à entrer manuellement leurs informations d’identification. Toutefois, certains appareils et navigateurs ne peuvent pas prendre en charge WIA et ainsi échoueront les demandes d’authentification à partir de ces appareils. En outre, l’expérience sur certains navigateurs negotiate, NTLM n’est pas souhaitable. L’approche recommandée consiste à utiliser l’authentification basée sur les formulaires pour ces appareils et les navigateurs.

AD FS dans Windows Server 2016 et Windows Server 2012 R2 offre les administrateurs la possibilité de configurer la liste des agents utilisateurs qui prennent en charge de la procédure de secours pour l’authentification basée sur les formulaires. La procédure de secours est rendue possible par deux configurations :


- Le **WIASupportedUserAgentStrings** propriété de la `Set-ADFSProperties` applet de commande
- Le **WindowsIntegratedFallbackEnabled** propriété de la `Set-AdfsGlobalAuthenticationPolicy` applet de commande

Le **WIASupportedUserAgentStrings** définit les agents utilisateurs qui prennent en charge WIA. AD FS analyse la chaîne d’agent utilisateur lorsque vous effectuez des connexions dans un navigateur ou d’un contrôle de navigateur. Si le composant de la chaîne d’agent utilisateur ne correspond pas à tous les composants des chaînes d’agent utilisateur qui sont configurés dans **WIASupportedUserAgentStrings** propriété, AD FS se tourne vers en fournissant l’authentification par formulaire, condition que le **WindowsIntegratedFallbackEnabled** indicateur est défini sur True.

Par défaut, une nouvelle installation d’AD FS possède un ensemble de correspondances de chaîne d’agent utilisateur créé. Toutefois, il peuvent être obsolètes selon les modifications apportées aux appareils et navigateurs. En particulier, les appareils Windows ont des chaînes d’agent utilisateur similaires avec des variations mineures dans les jetons. L’exemple de Windows PowerShell suivant fournit les meilleurs conseils pour l’ensemble actuel des appareils qui se trouvent sur le marché aujourd'hui qui prennent en charge WIA transparente :

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

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

Pour permettre à l’authentification basée sur le formulaire pour les agents utilisateurs autres que celles mentionnées dans la chaîne WIASupportedUserAgents de secours, définissez l’indicateur de WindowsIntegratedFallbackEnabled sur true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

Assurez-vous également que l’authentification basée sur les formulaires est activée pour l’intranet.

## <a name="configuring-wia-for-chrome"></a>Configurer WIA pour Chrome
Vous pouvez ajouter de Chrome ou autres agents utilisateur à la configuration AD FS qui prend en charge WIA. Cela permet d’ouverture de session transparente aux applications sans avoir à entrer manuellement les informations d’identification lorsque vous accédez à des ressources protégées par AD FS. Suivez les étapes ci-dessous pour activer WIA sur Chrome :

Ajouter une chaîne d’agent utilisateur pour Chrome dans la configuration AD FS

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
Vérifiez que la chaîne d’agent utilisateur pour Chrome est maintenant définie dans les propriétés AD FS

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![configurer l’authentification](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> Comme de nouveaux navigateurs et appareils sont publiées, il est recommandé de rapprocher les fonctionnalités de ces agents utilisateurs et de mettre à jour la configuration AD FS pour optimiser l’expérience d’authentification de l’utilisateur lorsque utilisant ledit navigateur et les appareils. Plus précisément, il est recommandé que vous réévaluer la **WIASupportedUserAgents** définition dans AD FS lorsque vous ajoutez un nouveau type d’appareil ou navigateur à votre matrice de prise en charge pour WIA.


