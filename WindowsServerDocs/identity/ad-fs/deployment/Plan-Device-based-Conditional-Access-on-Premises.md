---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: "Planifier l’accès conditionnel basé sur l’appareil local"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6303bba92ce4f4f99ae8e5095060b06885e427d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Planifier l’accès conditionnel basé sur l’appareil local

>S’applique à: Windows Server2016

Ce document décrit les stratégies d’accès conditionnel basés sur les appareils dans un scénario hybride où les répertoires locaux sont connectés à Azure AD à l’aide d’Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>Accès conditionnel AD FS et hybride  

AD FS fournit le composant de site sur des stratégies d’accès conditionnel dans un scénario hybride.  Lorsque vous inscrivez des périphériques avec Azure AD pour l’accès conditionnel aux ressources cloud, l’appareil Azure AD Connect réécrire fonctionnalité rend informations d’inscription de périphérique disponibles localement pour les stratégies d’AD FS d’utiliser et de mettre en œuvre.  De cette façon, vous avez une approche cohérente pour les stratégies de contrôle d’accès pour les deux sites et de ressources de cloud.  

![Accès conditionnel](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Types d’appareils inscrits  
Il existe trois types d’appareils inscrits, qui sont représentés en tant qu’objets appareil dans Azure AD et peut être utilisé pour l’accès conditionnel avec AD FS sur local, ainsi.  

| |Ajouter un travail ou scolaire compte  |Jonction à Azure AD  |Jointure Domian Windows 10    
| --- | --- |--- | --- |
|Description    |  Les utilisateurs d’ajouter leur travail ou scolaire compte à leur appareil BYOD de manière interactive.  **Remarque:** Ajouter compte professionnel ou scolaire est le remplacement pour la jonction d’espace de travail dans Windows 8/8.1       | Les utilisateurs joindre son appareil de travail Windows 10 à Azure AD.|Enregistrent automatiquement les appareils joints à un domaine de Windows 10 avec Azure AD.|           
|Comment les utilisateurs se connecter à l’appareil     |  Aucune connexion à Windows en tant que le compte professionnel ou scolaire.  Connectez-vous à l’aide d’un compte Microsoft.       |   Connectez-vous à Windows en tant que le compte (Professionnel ou scolaire) qui inscrit l’appareil.      |     Connectez-vous à l’aide du compte Active Directory.|      
|Mode de gestion des appareils    |      Stratégies GPM (avec inscription d’Intune supplémentaire)   | Stratégies GPM (avec inscription d’Intune supplémentaire)        |   Stratégie de groupe, System Center Configuration Manager (SCCM) |
|Type d’approbation AD Azure|Joint à un espace de travail|Joints à Azure AD|Joint au domaine  |     
|Emplacement des paramètres W10    | Paramètres > comptes > votre compte > ajouter un compte professionnel ou scolaire        | Paramètres > système > à propos > joindre Azure AD       |   Paramètres > système > à propos > joindre un domaine |       
|Vous pouvez également les périphériques iOS et Android?   |    Oui     |       N°  |   N°   |   

  

Pour plus d’informations sur les différentes façons d’inscrire des appareils, voir aussi:  
* [À l’aide d’appareils Windows 10 dans votre espace de travail](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [Configuration des appareils Windows 10 pour le travail](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Joindre Windows 10 Mobile à Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Comment Windows 10 utilisateur et périphérique ouverture de session est différent des versions précédentes  
Pour Windows10 et ADFS2016, il existe certains nouveaux aspects d’inscription de l’appareil et l’authentification, vous devez savoir sur (en particulier si vous êtes familiarisé avec l’inscription de l’appareil et «jonction» dans les versions précédentes).  

Tout d’abord, dans Windows 10 et AD FS dans Windows Server 2016, inscription de l’appareil et l’authentification n’est plus repose uniquement sur un X509 certificat utilisateur.  Il est un protocole qui fournit une meilleure sécurité et une expérience utilisateur plus transparente et la plus robuste.  Les principales différences que, pour la jonction de domaine Windows 10 et Azure AD Join, il existe une X509 certificat d’ordinateur et de nouvelles informations d’identification appellent un PRT.  Vous pouvez consulter toutes les [ici](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) et [ici](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

En second lieu, Windows 10 et les services AD FS 2016 prennent en charge l’authentification des utilisateurs à l’aide de Microsoft Passport for Work, qui vous pouvez en savoir plus sur [ici](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) et [ici](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/).  

AD FS 2016 fournit l’appareil en toute transparence et l’utilisateur l’authentification unique basée sur les informations d’identification PRT et Passport.  Suivant les étapes de ce document, vous pouvez activer ces fonctionnalités et voir leur travail.  

### <a name="device-access-control-policies"></a>Stratégies de contrôle d’accès aux périphériques  
Les périphériques peuvent être utilisés dans les règles de contrôle d’accès AD FS simples tels que:  

- Autoriser l’accès uniquement à partir d’un appareil enregistré   
- requièrent multi-Factor authentication lorsqu’un périphérique n’est pas enregistré.  

Ces règles peuvent être combinés avec d’autres facteurs tels que l’emplacement d’accès réseau et multi-Factor authentication, création de stratégies d’accès conditionnel enrichi tels que:  


- requièrent multi-Factor authentication pour les appareils non inscrits accès à partir d’en dehors du réseau d’entreprise, à l’exception des membres d’un groupe particulier ou de groupes  

Avec AD FS 2016, ces stratégies peuvent être configurées spécifiquement pour exiger l’ainsi un niveau de confiance appareil particulier: soit **authentifié**, **gérés**, ou **conforme**.  

Pour plus d’informations sur la configuration AD FS stratégies de contrôle d’accès, consultez [stratégies de contrôle d’accès dans AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Périphériques authentifiés  
Périphériques authentifiés sont enregistrés qui ne sont pas inscrits dans GPM (Intune et 3ème partie gestion des appareils mobiles pour Windows 10, Intune uniquement pour iOS et Android).   

Périphériques authentifiés aura le **isManaged** AD FS revendication avec la valeur **FALSE**. (Alors que les périphériques qui ne sont pas enregistrés tout ne disposera pas cette revendication.) Périphériques authentifiés (et enregistrés tous les périphériques) aura l’isKnown AD FS revendication avec la valeur **TRUE**.  

#### <a name="managed-devices"></a>Périphériques gérés:   

Les périphériques gérés sont les appareils inscrits qui sont inscrits avec GPM.  

Les périphériques gérés aura l’isManaged AD FS revendication avec la valeur **TRUE**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Appareils compatibles (avec GPM ou des stratégies de groupe)  
Périphériques conformes sont enregistrés qui ne sont pas uniquement inscrits avec GPM, mais il est compatible avec les stratégies GPM. (Informations de conformité est associée à la GPM et sont écrit dans Azure AD).  

Périphériques compatibles aura le **isCompliant** AD FS revendication avec la valeur **TRUE**.    

Pour obtenir une liste complète des revendications d’accès conditionnel appareil ADFS2016, voir [référence](#reference).  


## <a name="reference"></a>Référence  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Liste complète des nouvelles revendications FS 2016 et le dispositif d’Active Directory  

* https://schemas.microsoft.com/ws/2014/01/identity/claims/anchorclaimtype  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/implicitupn  
* https://schemas.microsoft.com/2014/03/psso  
* https://schemas.microsoft.com/2015/09/prt  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/UPN  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Name  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/displayname  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/identifier  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ostype  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/osversion  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser  
* https://schemas.microsoft.com/2014/02/devicecontext/claims/isknown  
* https://schemas.microsoft.com/2014/02/deviceusagetime  
* https://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant  
* https://schemas.microsoft.com/2014/09/devicecontext/claims/trusttype  
* https://schemas.microsoft.com/claims/authnmethodsreferences  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path  
* https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/client-request-id  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/relyingpartytrustid  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-ip  
* https://schemas.microsoft.com/2014/09/requestcontext/claims/userip  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod  
