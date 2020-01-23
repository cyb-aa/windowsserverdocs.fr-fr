---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configure Additional Authentication Methods for AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/26/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a7cfc80a1e137a4184afa0e8b33a488f1e35dcfa
ms.sourcegitcommit: 840d1d8851f68936db3934c80796fb8722d3c64a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519451"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configure Additional Authentication Methods for AD FS

Pour activer l'authentification multifacteur, vous devez sélectionner au moins une méthode d'authentification supplémentaire. Par défaut, dans Services ADFS (AD FS) dans Windows Server 2012 R2, vous pouvez sélectionner l’authentification par certificat (en d’autres termes, l’authentification basée sur une carte à puce) comme méthode d’authentification supplémentaire.

> [!NOTE]
> Si vous sélectionnez l’authentification par certificat, vérifiez que les certificats de carte à puce ont été configurés en toute sécurité et exigent un code confidentiel.

Saviez-vous que Microsoft Azure offre une fonctionnalité similaire dans le cloud ? En savoir plus sur les [solutions de gestion des identités Microsoft Azure](https://aka.ms/m2w274).<br /><br />Créer une solution d’identité hybride dans Microsoft Azure :<br /> - [en savoir plus sur Azure Multi-Factor Authentication.](https://aka.ms/ey6o9r)<br /> - [gérer les identités pour les environnements hybrides à forêt unique à l’aide de l’authentification Cloud.](https://aka.ms/g1jat8)<br /> - [gérer les risques avec des multi-Factor Authentication supplémentaires pour les applications sensibles.](https://aka.ms/kt1bbm)

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Méthodes d'authentification supplémentaires tierces et Microsoft
Vous pouvez également configurer et activer des méthodes d’authentification Microsoft et tierces dans AD FS dans Windows Server 2012 R2. Une fois installé et inscrit auprès de AD FS, vous pouvez appliquer l’authentification multifacteur dans le cadre de la stratégie d’authentification globale ou par partie de confiance.

Voici une liste alphabétique des fournisseurs tiers et Microsoft avec les offres d'authentification multifacteur actuellement disponibles pour AD FS dans Windows Server 2012 R2.

|Fournisseur|Offre|Lien pour en savoir plus|
|-|-|-| 
|aPersona|Multi-Factor Authentication adaptative aPersona pour l’authentification unique (SSO) Microsoft ADFS|[Adaptateur aPersona ASM ADFS](https://www.apersona.com/adfs)|
|Cyphercor Inc.|Multi-Factor Authentication LoginTC pour AD FS|[Connecteur AD FS LoginTC](https://www.logintc.com/docs/connectors/adfs.html)|
|Duo Security|Adaptateur d’authentification multifacteur duo pour AD FS|[Authentification duo pour AD FS](https://duo.com/docs/adfs)|
|Futurae|Suite d’authentification futurae pour AD FS|[Authentification forte futurae](https://futurae.com)|
|Gemalto|Services de sécurité & et d'identité Gemalto|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo Technologies|Service d'authentification d'entreprise inWebo|[inWebo Enterprise Authentication](http://www.inwebo.com)|
|Login People|Connecteur MFA API Login People pour AD FS 2012 R2 (bêta publique)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp.|Authentification multifacteur Microsoft Azure|[Guide pas à pas : gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles](https://technet.microsoft.com/library/dn280946.aspx) (voir étape 3)|
Mideye | Fournisseur d’authentification Mideye pour ADFS | [Mideye l’authentification à deux facteurs avec Microsoft Active Directory service FS (Federation Service)](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | Okta MFA pour Services ADFS | [Okta MFA pour Services ADFS (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|One Identity| AD FS 2FA Starling|[Adaptateur AD FS Starling 2FA](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|One Identity| AD FS Defender|[Adaptateur de AD FS Defender](https://www.oneidentity.com/products/defender/)|
|Ping Identity|Adaptateur MFA PingID pour AD FS|[Adaptateur MFA PingID pour AD FS](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, The Security Division of EMC|Agent d'authentification RSA SecurID pour Microsoft Active Directory Federation Services|[Agent d’authentification RSA SecurID pour Microsoft Services ADFS](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|Agent SAS (SafeNet Authentication Service) pour AD FS|[Service d’authentification SafeNet : Guide de configuration de l’agent de AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|Fournisseur de mot de passe à usage unique SecureMFA| [Fournisseurs d’authentification multifacteur ADFS](https://www.securemfa.com/)|
|Swisscom|Services de signature et service d'authentification Mobile ID|[Service d’authentification Mobile ID](http://swisscom.ch/mid)|
|Symantec|Symantec Validation and ID Protection Service (VIP)|[Service de sécurité d’ID et de validation Symantec (VIP)](http://www.symantec.com/vip-authentication-service)|
|Trusona|Essentiel (MFA avec mot de passe) et Executive (vérification essentielle + identité)| [Trusona Multi-Factor Authentication](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Méthode d'authentification personnalisée pour AD FS dans Windows Server 2012 R2
Nous proposons désormais des instructions pour créer votre propre méthode d'authentification personnalisée pour AD FS dans Windows Server 2012 R2. Pour plus d'informations, voir [Créer une méthode d'authentification personnalisée pour AD FS dans Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Articles associés
[Gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


