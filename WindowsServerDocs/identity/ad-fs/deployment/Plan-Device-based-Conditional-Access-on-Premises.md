---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Planifier l’accès conditionnel local basé sur un périphérique
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a78334f64d9e51515757b01f2d788bf87f67a35
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501605"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Planifier l’accès conditionnel local basé sur un périphérique


Ce document décrit les stratégies d’accès conditionnel basés sur des appareils dans un scénario hybride où les répertoires locaux sont connectés à Azure AD à l’aide d’Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>Accès conditionnel AD FS et hybrides  

AD FS fournit le composant de site sur des stratégies d’accès conditionnel dans un scénario hybride.  Lorsque vous inscrivez des appareils avec Azure AD pour l’accès conditionnel aux ressources du cloud, l’appareil Azure AD Connect réécrit fonctionnalité rend des informations d’inscription de périphérique disponibles en local pour consommer et d’appliquer des stratégies AD FS.  De cette façon, vous avez une approche cohérente pour les stratégies de contrôle d’accès pour les deux en local et de ressources de cloud.  

![Accès conditionnel](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Types d’appareils inscrits  
Il existe trois types d’appareils inscrits, tous sont représentés en tant qu’objets d’appareil dans Azure AD et peuvent être utilisées pour l’accès conditionnel avec AD FS en local également.  

| |Ajouter un travail ou scolaire  |Joindre Azure AD  |Jonction de domaine 10 de Windows    
| --- | --- |--- | --- |
|Description    |  Les utilisateurs d’ajouter leur travail compte scolaire ou pour leur appareil BYOD interactivement.  **Remarque :** Ajouter le compte professionnel ou scolaire est le remplacement pour la jonction dans Windows 8/8.1       | Les utilisateurs joindre leurs appareils de travail de Windows 10 à Azure AD.|Les appareils Windows 10 joints à un domaine s’inscrivent automatiquement auprès d’Azure AD.|           
|Comment les utilisateurs se connecter à l’appareil     |  Aucun compte de connexion à Windows en tant que le compte professionnel ou scolaire.  Connexion à l’aide d’un compte Microsoft.       |   Connectez-vous à Windows en tant que le compte (Professionnel ou scolaire) qui inscrit l’appareil.      |     Connexion à l’aide de compte Active Directory.|      
|La gestion des appareils    |      Stratégies de gestion des appareils mobiles (avec l’inscription à Intune supplémentaire)   | Stratégies de gestion des appareils mobiles (avec l’inscription à Intune supplémentaire)        |   Stratégie de groupe, System Center Configuration Manager (SCCM) |
|Type d’approbation AD Azure|Espace de travail joint|Joint à Azure AD|Client appartenant à un domaine  |     
|Emplacement des paramètres de W10    | Paramètres > comptes > votre compte > Ajouter un compte professionnel ou scolaire        | Paramètres > système > sur > joindre Azure AD       |   Paramètres > système > sur > joindre un domaine |       
|Également disponible pour les appareils iOS et Android ?   |    Oui     |       Non  |   Non   |   

  

Pour plus d’informations sur les différentes façons d’inscrire des appareils, consultez également :  
* [À l’aide d’appareils Windows 10 dans votre espace de travail](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [Configuration des appareils Windows 10 pour le travail](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Joindre Windows 10 Mobile à Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Comment Windows 10 utilisateur et appareil de l’authentification est différent des versions précédentes  
Pour Windows 10 et AD FS 2016, il existe certains nouveaux aspects de l’inscription et d’authentification que vous devez connaître (surtout si vous êtes familiarisé avec l’inscription et « jonction » dans les versions précédentes).  

Tout d’abord, dans Windows 10 et AD FS dans Windows Server 2016, l’inscription et l’authentification n’est plus repose uniquement sur un X509 certificat utilisateur.  Il existe un protocole de nouveaux et plus robuste qui fournit une meilleure sécurité et une expérience utilisateur plus transparente.  Les principales différences sont que, pour la jonction de domaine Windows 10 et Azure AD Join, est un X509 certificat d’ordinateur et une information d’identification appelé un PRT.  Vous pouvez lire tous les il [ici](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) et [ici](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

En second lieu, Windows 10 et AD FS 2016 prennent en charge l’authentification des utilisateurs à l’aide de Microsoft Passport for Work, vous pouvez en savoir plus sur [ici](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) et [ici](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/).  

AD FS 2016 fournit l’appareil en toute transparence et utilisateur SSO avec les informations d’identification PRT et Passport.  Suivant les étapes de ce document, vous pouvez activer ces fonctionnalités et les afficher à fonctionner.  

### <a name="device-access-control-policies"></a>Stratégies de contrôle d’accès aux appareils  
Appareils peuvent être utilisés dans les règles de contrôle d’accès AD FS simples tel que :  

- Autoriser l’accès uniquement à partir d’un appareil enregistré   
- exiger l’authentification multifacteur lorsqu’un appareil n’est pas inscrit.  

Ces règles peuvent ensuite être combinées avec d’autres facteurs tels que l’emplacement d’accès réseau et l’authentification multifacteur, création de stratégies d’accès conditionnel riches, telles que :  


- Exigez l’authentification multifacteur pour les appareils non enregistrés accéder à partir en dehors du réseau d’entreprise, sauf pour les membres d’un groupe particulier ou de groupes  

Avec AD FS 2016, ces stratégies peuvent être configurées en particulier pour nécessitent également un niveau de confiance périphérique particulier : soit **authentifié**, **gérés**, ou **conformes**.  

Pour plus d’informations sur la configuration AD FS stratégies de contrôle d’accès, consultez [stratégies de contrôle d’accès dans AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Appareils authentifiés  
Appareils authentifiés sont les appareils inscrits ne sont pas inscrits dans MDM (Intune et 3e partie MDMs pour Windows 10, Intune uniquement pour iOS et Android).   

Appareils authentifiés auront le **isManaged** avec la valeur de revendication AD FS **FALSE**. (Tandis que les appareils non enregistrés ne disposera pas de cette revendication.)  Appareils authentifiés (et tous les appareils inscrits) auront l’isKnown AD FS revendication avec la valeur **TRUE**.  

#### <a name="managed-devices"></a>Appareils gérés :   

Périphériques gérés se trouvent les appareils inscrits qui sont inscrits avec des appareils mobiles.  

Les appareils gérés auront l’isManaged AD FS revendication avec la valeur **TRUE**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Appareils conformes (avec gestion des appareils mobiles ou des stratégies de groupe)  
Les appareils conformes sont les appareils inscrits ne sont pas uniquement inscrits avec la gestion des appareils mobiles, mais il est conforme aux stratégies de gestion des appareils mobiles. (Informations de conformité est associée à la gestion des appareils mobiles et sont écrite dans Azure AD).  

Les appareils conformes aura le **isCompliant** avec la valeur de revendication AD FS **TRUE**.    

Pour obtenir une liste complète de l’appareil d’AD FS 2016 et les revendications de l’accès conditionnel, consultez [référence](#reference).  


## <a name="reference"></a>Référence  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Liste complète des nouvelles des AD FS 2016 et les appareils des revendications  

* https://schemas.microsoft.com/ws/2014/01/identity/claims/anchorclaimtype  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/implicitupn  
* https://schemas.microsoft.com/2014/03/psso  
* https://schemas.microsoft.com/2015/09/prt  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name  
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
