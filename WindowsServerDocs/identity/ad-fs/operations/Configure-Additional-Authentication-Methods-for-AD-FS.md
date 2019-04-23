---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configurer des méthodes d'authentification supplémentaires pour AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/04/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73de8908677b3f74651b10c29ef2abe62e484694
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868410"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configurer des méthodes d'authentification supplémentaires pour AD FS

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Pour activer l'authentification multifacteur, vous devez sélectionner au moins une méthode d'authentification supplémentaire. Par défaut, dans Active Directory Federation Services (ADFS) dans Windows Server 2012 R2, vous pouvez sélectionner l’authentification par certificat (en d’autres termes, authentification basée sur une carte à puce) comme méthode d’authentification supplémentaires.

> [!NOTE]
> Si vous sélectionnez l’authentification par certificat, vérifiez que les certificats de carte à puce ont été configurés en toute sécurité et exigent un code confidentiel.

Saviez-vous que Microsoft Azure offre une fonctionnalité similaire dans le cloud ? En savoir plus sur les [solutions de gestion des identités Microsoft Azure](http://aka.ms/m2w274).<br /><br />Créez une solution d’identité hybride dans Microsoft Azure :<br /> - [En savoir plus sur Azure multi-Factor Authentication.](http://aka.ms/ey6o9r)<br /> - [Gérer les identités pour les environnements hybrides à forêt unique à l’aide de l’authentification cloud.](http://aka.ms/g1jat8)<br /> - [Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles.](http://aka.ms/kt1bbm)

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Méthodes d'authentification supplémentaires tierces et Microsoft
Vous pouvez également configurer et activer Microsoft et les méthodes d’authentification tiers dans AD FS dans Windows Server 2012 R2. Une fois installé et inscrit avec AD FS, vous pouvez appliquer l’authentification Multifacteur dans le cadre de la stratégie d’authentification globale ou par partie de confiance.

Voici une liste alphabétique des fournisseurs tiers et Microsoft avec les offres d'authentification multifacteur actuellement disponibles pour AD FS dans Windows Server 2012 R2.

|Fournisseur|Offre|Lien pour en savoir plus|
|-|-|-| 
|aPersona|aPersona ADAPTATIF multi-Factor Authentication pour Microsoft SSO ADFS|[aPersona adaptateur ADFS de ASM](https://www.apersona.com/adfs)|
|Duo sécurité|Adaptateur MFA Duo pour AD FS|[Authentification Duo pour AD FS](https://duo.com/docs/adfs)|
|Gemalto|Services de sécurité & et d'identité Gemalto|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo Technologies|Service d'authentification d'entreprise inWebo|[inWebo l’authentification d’entreprise](http://www.inwebo.com)|
|Login People|Connecteur MFA API Login People pour AD FS 2012 R2 (bêta publique)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp.|Authentification multifacteur Microsoft Azure|[Guide pas à pas : gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles](https://technet.microsoft.com/library/dn280946.aspx) (voir l'étape 3)|
Mideye | Fournisseur d’authentification mideye pour ADFS | [Authentification à deux facteurs mideye avec Microsoft Active Directory Federation Services](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | MFA Okta pour Active Directory Federation Services | [MFA Okta pour Active Directory Federation Services (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|Une seule identité| Starling 2FA AD FS|[Starling 2FA adaptateur AD FS](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|Une seule identité| Defender AD FS|[Adaptateur Defender AD FS](https://www.oneidentity.com/products/defender/)|
|Ping Identity|Adaptateur PingID MFA pour AD FS|[Adaptateur PingID MFA pour AD FS](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, The Security Division of EMC|Agent d'authentification RSA SecurID pour Microsoft Active Directory Federation Services|[Agent d’authentification RSA SecurID pour Microsoft Active Directory Federation Services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|Agent SAS (SafeNet Authentication Service) pour AD FS|[SafeNet Authentication Service: AD FS Agent Configuration Guide](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|Fournisseur de SecureMFA OTP| [Fournisseurs d’authentification AD FS Multi-Factor](https://www.securemfa.com/)|
|Swisscom|Services de signature et service d'authentification Mobile ID|[Service d’authentification mobile ID](http://swisscom.ch/mid)|
|Symantec|Symantec Validation and ID Protection Service (VIP)|[Validation de Symantec et de Service de Protection d’ID (VIP)](http://www.symantec.com/vip-authentication-service)|
|Trusona|Essentielles (sans mot de passe MFA) et Executive (essentielles + vérification d’identité)| [Trusona multi-factor Authentication](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Méthode d'authentification personnalisée pour AD FS dans Windows Server 2012 R2
Nous proposons désormais des instructions pour créer votre propre méthode d'authentification personnalisée pour AD FS dans Windows Server 2012 R2. Pour plus d'informations, voir [Créer une méthode d'authentification personnalisée pour AD FS dans Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Voir aussi
[Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


