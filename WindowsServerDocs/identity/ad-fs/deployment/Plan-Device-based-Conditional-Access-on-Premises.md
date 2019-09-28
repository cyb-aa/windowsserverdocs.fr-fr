---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Planifier l’accès conditionnel local basé sur un périphérique
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 79dfc7fbf9e2dcc753829cc53d914f374010f925
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408334"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Planifier l’accès conditionnel local basé sur un périphérique


Ce document décrit les stratégies d’accès conditionnel basées sur les appareils dans un scénario hybride dans lequel les annuaires locaux sont connectés à Azure AD à l’aide de Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS et accès conditionnel hybride  

AD FS fournit le composant local des stratégies d’accès conditionnel dans un scénario hybride.  Lorsque vous inscrivez des appareils avec Azure AD pour l’accès conditionnel aux ressources du Cloud, la fonctionnalité de réécriture de l’appareil Azure AD Connect rend les informations d’inscription de l’appareil disponibles localement pour les stratégies de AD FS à consommer et à appliquer.  De cette façon, vous disposez d’une approche cohérente pour accéder aux stratégies de contrôle des ressources locales et du Cloud.  

![Accès conditionnel](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Types d’appareils inscrits  
Il existe trois types d’appareils inscrits, qui sont tous représentés en tant qu’objets de périphérique dans Azure AD et peuvent être utilisés pour l’accès conditionnel avec AD FS également sur site.  

| |Ajouter un compte professionnel ou scolaire  |Joindre Azure AD  |Jonction de domaine Windows 10    
| --- | --- |--- | --- |
|Description    |  Les utilisateurs ajoutent leur compte professionnel ou scolaire à leur appareil BYOD de manière interactive.  **Remarque :** Ajouter un compte professionnel ou scolaire est le remplacement de Workplace Join dans Windows 8/8.1       | Les utilisateurs joignent leur appareil de travail Windows 10 à Azure AD.|Les appareils Windows 10 joints à un domaine s’inscrivent automatiquement auprès de Azure AD.|           
|Comment les utilisateurs se connectent à l’appareil     |  Aucune connexion à Windows en tant que compte professionnel ou scolaire.  Connectez-vous à l’aide d’un compte Microsoft.       |   Connectez-vous à Windows en tant que compte (professionnel ou scolaire) qui a inscrit l’appareil.      |     Connectez-vous à l’aide du compte Active Directory.|      
|Gestion des appareils    |      Stratégies MDM (avec inscription Intune supplémentaire)   | Stratégies MDM (avec inscription Intune supplémentaire)        |   Stratégie de groupe, System Center Configuration Manager (SCCM) |
|Type d’approbation Azure AD|Espace de travail joint|Azure AD joint|Client appartenant à un domaine  |     
|Emplacement des paramètres W10    | Paramètres > comptes > votre compte > Ajouter un compte professionnel ou scolaire        | Paramètres > système > sur > joindre Azure AD       |   Paramètres > système > sur > joindre un domaine |       
|Également disponible pour les appareils iOS et Android ?   |    Oui     |       Non  |   Non   |   

  

Pour plus d’informations sur les différentes façons d’inscrire des appareils, consultez également :  
* [Utilisation d’appareils Windows 10 dans votre espace de travail](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [Configuration des appareils Windows 10 pour le travail](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Joindre Windows 10 mobile à Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>La façon dont l’authentification des utilisateurs et des appareils Windows 10 est différente de celle des versions précédentes  
Pour Windows 10 et AD FS 2016, vous devez connaître les nouveaux aspects de l’inscription et de l’authentification des appareils (surtout si vous êtes familiarisé avec l’inscription des appareils et la « jonction d’espace de travail » dans les versions précédentes).  

Tout d’abord, dans Windows 10 et AD FS dans Windows Server 2016, l’inscription et l’authentification de l’appareil ne sont plus basées uniquement sur un certificat d’utilisateur x509.  Il existe un nouveau protocole plus robuste qui offre une meilleure sécurité et une expérience utilisateur plus transparente.  Les principales différences résident dans le fait que, pour la jonction de domaine Windows 10 et la jonction de Azure AD, il existe un certificat d’ordinateur x509 et une nouvelle information d’identification appelée PRT.  Vous pouvez le lire [ici](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) et [ici](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

Deuxièmement, Windows 10 et AD FS 2016 prennent en charge l’authentification utilisateur à l’aide de Microsoft Passport for Work, que vous pouvez consulter [ici](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) et [ici](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/).  

AD FS 2016 fournit une authentification unique des appareils et des utilisateurs en fonction des informations d’identification PRT et Passport.  À l’aide des étapes décrites dans ce document, vous pouvez activer ces fonctionnalités et les voir fonctionner.  

### <a name="device-access-control-policies"></a>Stratégies de Access Control des appareils  
Les appareils peuvent être utilisés dans des règles de contrôle d’accès AD FS simples telles que :  

- autoriser l’accès uniquement à partir d’un appareil inscrit   
- exiger l’authentification multifacteur lorsqu’un appareil n’est pas inscrit  

Ces règles peuvent ensuite être combinées avec d’autres facteurs tels que l’emplacement d’accès réseau et l’authentification multifacteur, créant ainsi des stratégies d’accès conditionnel riches, telles que :  


- exiger l’authentification multifacteur pour les appareils non inscrits qui accèdent à l’extérieur du réseau d’entreprise, à l’exception des membres d’un groupe ou de groupes particuliers  

Avec AD FS 2016, ces stratégies peuvent être configurées spécifiquement pour exiger un niveau de confiance d’appareil particulier : **authentifié**, **géré**ou **conforme**.  

Pour plus d’informations sur la configuration des stratégies de contrôle d’accès AD FS, consultez [stratégies de contrôle d’accès dans AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Appareils authentifiés  
Les appareils authentifiés sont des appareils inscrits qui ne sont pas inscrits dans MDM (Intune et MDMs tiers pour Windows 10, Intune uniquement pour iOS et Android).   

Les appareils authentifiés auront la revendication **isManaged** AD FS avec la valeur **false**. (Alors que les appareils qui ne sont pas inscrits sur le tout n’auront pas cette revendication.)  Les appareils authentifiés (et tous les appareils inscrits) comporteront la revendication isKnown AD FS avec la valeur **true**.  

#### <a name="managed-devices"></a>Appareils gérés :   

Les appareils gérés sont des appareils inscrits inscrits auprès de MDM.  

Les appareils gérés comporteront la revendication isManaged AD FS avec la valeur **true**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Appareils conformes (avec MDM ou stratégies de groupe)  
Les appareils conformes sont des appareils inscrits non seulement inscrits avec MDM, mais conformes aux stratégies MDM. (Les informations de conformité proviennent de MDM et sont écrites dans Azure AD.)  

Les appareils conformes comporteront la revendication **isCompliant** AD FS avec la valeur **true**.    

Pour obtenir la liste complète des revendications de l’appareil AD FS 2016 et de l’accès conditionnel, consultez [référence](#reference).  


## <a name="reference"></a>Référence  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Liste complète des nouvelles AD FS 2016 et des revendications de périphérique  

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
