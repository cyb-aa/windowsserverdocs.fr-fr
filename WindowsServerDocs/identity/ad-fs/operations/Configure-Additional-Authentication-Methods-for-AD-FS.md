---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: "Configurer des méthodes d’authentification supplémentaires pour ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 65baa6f94e1efa1fa337eab477b18f947cf0bb2d
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/21/2018
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configurer des méthodes d’authentification supplémentaires pour ADFS

>S’applique à: Windows Server2016, Windows Server2012R2

Pour activer l’authentification multifacteur (MFA), vous devez sélectionner au moins une méthode d’authentification supplémentaire. Par défaut, dans ActiveDirectory Federation Services (ADFS) dans Windows Server2012R2, vous pouvez sélectionner l’authentification par certificat (en d’autres termes, l’authentification basée sur une carte à puce) comme méthode d’authentification supplémentaire.

> [!NOTE]
> Si vous sélectionnez l’authentification par certificat, assurez-vous que les certificats de carte à puce ont été configurés en toute sécurité et exigent un code confidentiel.

Saviez-vous que Microsoft Azure offre une fonctionnalité similaire dans le cloud? En savoir plus sur [solutions d’identité MicrosoftAzure ](http://aka.ms/m2w274).<br /><br />Créez une solution d’identité hybride dans MicrosoftAzure:<br /> - [En savoir plus sur Azure multi-Factor Authentication.](http://aka.ms/ey6o9r)<br /> - [Gérer les identités pour les environnements hybrides à forêt unique à l’aide de l’authentification cloud.](http://aka.ms/g1jat8)<br /> - [Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles.](http://aka.ms/kt1bbm)|

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Méthodes d’authentification supplémentaires tierces et Microsoft
Vous pouvez également configurer et activer des méthodes d’authentification tiers dans ADFS dans Windows Server2012R2 et Microsoft. Une fois installée et inscrite avec ADFS, vous pouvez appliquer l’authentification Multifacteur dans le cadre de la stratégie d’authentification globale ou par partie de confiance.

Voici une liste alphabétique de Microsoft et des fournisseurs tiers avec des offres d’authentification Multifacteur actuellement disponibles pour ADFS dans Windows Server2012R2.

||||
|-|-|-|
|**Fournisseur**|**Offre**|**Lien pour en savoir plus**|
|Gemalto|Services de sécurité et et d’identité Gemalto|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|Technologies d’inWebo|inWebo service d’authentification de l’entreprise|[inWebo authentification en entreprise](http://www.inwebo.com)|
|Personnes de connexion|Connecteur de personnes MFA API Login pour ADFS2012R2 (bêta publique)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|MicrosoftCorp.|MicrosoftAzure MFA|[Guide pas à pas: Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](https://technet.microsoft.com/library/dn280946.aspx) (voir l’étape3)|
|RSA, la Division sécurité d’EMC|Agent d’authentification RSA SecurID pour MicrosoftActiveDirectory Federation Services|[Agent d’authentification RSA SecurID pour MicrosoftActiveDirectory Federation Services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|Agent de Service (SAS) SafeNet d’authentification pour ADFS|[SafeNet Authentication Service: Guide de Configuration de l’Agent ADFS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|Swisscom|Services de Signature et Service d’authentification mobile ID|[Service d’authentification mobile ID](http://swisscom.ch/mid)|
|Symantec|Service de Protection de ID (VIP) et les Symantec Validation|[Service de Protection de ID (VIP) et les Symantec Validation](http://www.symantec.com/vip-authentication-service)|

## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Méthode d’authentification personnalisée pour ADFS dans Windows Server2012R2
Nous proposons désormais des instructions pour créer votre propre méthode d’authentification personnalisée pour ADFS dans Windows Server2012R2. Pour plus d’informations, voir [créer une méthode d’authentification personnalisée pour ADFS dans Windows Server2012R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Voir aussi
[Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


