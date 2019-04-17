---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: "Configuration de l’authentification basée sur les formulaires intranet pour les périphériques qui ne prennent pas en charge WIA"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c78dc702cbff8eab487c6c077dfe57b0f0663342
ms.sourcegitcommit: 5012c078b410f59261edf0cc917393467243a5c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/23/2018
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>Configuration de l’authentification basée sur les formulaires intranet pour les périphériques qui ne prennent pas en charge WIA

>S’applique à: Windows Server2016, Windows Server2012R2

Par défaut, l’authentification intégrée de Windows (WIA) est activé dans ActiveDirectory Federation Services (ADFS) dans Windows Server2012R2 pour les demandes d’authentification qui se produisent dans le réseau interne de l’entreprise (intranet) pour une application qui utilise un navigateur pour son authentification. Par exemple, il peut s’applications de navigateur qui utilisent WS-Federation ou SAML protocoles et applications qui utilisent le protocole OAuth. WIA fournit aux utilisateurs finaux d’ouverture de session transparente pour les applications sans avoir à entrer manuellement leurs informations d’identification. Toutefois, certains appareils et navigateurs ne sont pas capables de prendre en charge WIA et par conséquent d’échouer les demandes d’authentification à partir de ces appareils. En outre, l’expérience sur certains navigateurs négocier NTLM n’est pas souhaitable. L’approche recommandée est de revenir à l’authentification par formulaire pour ces appareils et les navigateurs.

ADFS dans Windows Server2016 et Windows Server2012R2 offre les administrateurs la possibilité de configurer la liste d’agents utilisateurs qui prennent en charge de secours pour l’authentification par formulaire. Le secours est rendu possible par les deux configurations:


- Le **WIASupportedUserAgentStrings** propriété de la `Set-ADFSProperties`applet de commande
- Le **WindowsIntegratedFallbackEnabled** propriété de la `Set-AdfsGlobalAuthenticationPolicy`commmandlet

Le **WIASupportedUserAgentStrings** définit les agents d’utilisateur qui prennent en charge WIA. ADFS analyse la chaîne d’agent utilisateur lors de l’exécution des connexions dans un navigateur ou d’un contrôle de navigateur. Si le composant de la chaîne d’agent utilisateur ne correspond pas à un des composants des chaînes d’agent utilisateur qui sont configurés dans **WIASupportedUserAgentStrings** propriété, ADFS reviennent fournir l’authentification basée sur les formulaires, à condition que la **WindowsIntegratedFallbackEnabled** indicateur est défini sur True.

Par défaut, une nouvelle installation d’ADFS possède un jeu de correspondances de chaîne d’agent utilisateur créé. Toutefois, il peut s’agir obsolètes basé sur les modifications apportées aux appareils et navigateurs. En particulier, les appareils Windows possèdent des chaînes d’agent utilisateur similaire avec des variations mineures dans les jetons. L’exemple suivant de Windows PowerShell fournit les meilleurs conseils pour l’ensemble des périphériques qui sont actuellement sur le marché qui prennent en charge WIA transparente actuel:

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

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
WindowsNT6.1; trident/7.0</br></br>WindowsNT6.1; win64; x64; trident/7.0</br></br>WindowsNT6.1; wOW64; trident/7.0|Système d’exploitation Windows7 bureau, différentes plateformes|
MSIPC| Client de contrôle et de Protection des informations de Microsoft|
Client de gestion des droits Windows|Client de gestion des droits Windows|

Afin de permettre le retour à l’authentification par formulaire en fonction des agents utilisateur autre que ceux mentionnés dans la chaîne WIASupportedUserAgents, définir l’indicateur WindowsIntegratedFallbackEnabled sur true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

Assurez-vous également que l’authentification basée sur les formulaires est activée pour l’intranet.

## <a name="configuring-wia-for-chrome"></a>Configuration WIA pour Chrome
Vous pouvez ajouter Chrome ou autres agents utilisateurs à la configuration ADFS qui prend en charge WIA. Cela permet d’ouverture de session transparente pour les applications sans avoir à entrer manuellement les informations d’identification lorsque vous accéder aux ressources protégées par ADFS. Suivez les étapes ci-dessous pour activer WIA sur Chrome:

Ajouter une chaîne d’agent utilisateur pour le Chrome de configuration ADFS

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
Vérifiez que la chaîne d’agent utilisateur pour Chrome est maintenant définie dans les propriétés d’ADFS

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![Configurer l’authentification](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> Comme de nouveaux navigateurs et appareils sont publiées, il est recommandé de rapprocher les fonctionnalités de ces agents utilisateurs et de mettre à jour de la configuration ADFS en conséquence afin d’optimiser l’expérience d’authentification de l’utilisateur lorsque utilisant ledit navigateur et les périphériques. Plus précisément, il est recommandé que vous réévaluez les **WIASupportedUserAgents** définition dans ADFS lorsque vous ajoutez un nouveau type de périphérique ou le navigateur à votre matrice de prise en charge pour WIA.


