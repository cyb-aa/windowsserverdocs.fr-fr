---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 'Procédure pas à pas : Workplace Join avec un appareil iOS'
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5214165c2843461a2da8b574ad28f92b0e0bc64d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407498"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Démonstration : joindre un espace de travail avec un appareil iOS


> [!IMPORTANT] 
> Cette méthode s’applique uniquement aux clients entièrement local. Les clients hybrides ou Cloud uniquement ne doivent pas utiliser cette méthode pour inscrire leurs appareils iOS. Cette méthode n’est pas compatible lorsque les clients local décident de passer au Cloud. L’inscription de l’appareil doit être annulée et inscrite auprès du Cloud. 

Cette rubrique explique comment joindre un espace de travail avec un appareil iOS. Vous devez effectuer les étapes décrites dans la section [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2 avant de](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) pouvoir essayer cette procédure pas à pas. Vous pouvez utiliser l’appareil pour accéder à la même application Web d’entreprise que vous avez consultée dans la [procédure pas à pas : Workplace Join avec un appareil Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).


## <a name="join-an-ios-device-with-workplace-join"></a>Joindre un espace de travail avec un appareil iOS

> [!IMPORTANT]
> Lorsqu’un service DRS sur site est configuré, l’appareil iOS doit approuver le certificat SSL (Secure Socket Layer) utilisé pour configurer les services AD FS (Active Directory Federation Services) à l’ [Step 2: Configure the federation server (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4), pour que la jonction d’espace de travail réussisse.
> 
> -   Si le certificat SSL AD FS a été émis depuis une autorité de certification de test, vous devez installer le certificat de l'autorité de certification sur votre appareil iOS.
> -   Si votre certificat d'autorité de certification est publié sur un site web, vous pouvez accéder à ce dernier depuis votre appareil iOS et installer le certificat.

Cette démonstration indique comment joindre l'appareil à l'espace de travail.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Pour joindre un appareil iOS à un espace de travail

1. -   **Quand Azure Active Directory Device Registration service est le service DRS configuré :** Ouvrez Apple Safari et accédez à Azure Active Directory Device Registration point de terminaison de profil sur air pour les appareils iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > où <`yourdomainname`> est le nom de domaine que vous avez configuré avec Azure Active Directory. Par exemple, si votre nom de domaine est contoso.com, l’URL serait : `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

   -   **Lorsque Drs local est la configuration Drs**: Open Apple Safari et accédez au point de terminaison de profil par le biais du service d’inscription d’appareils (DRS) pour les appareils iOS, `https://adf1s.contoso.com/enrollmentserver/otaprofile`

   Il existe de nombreuses façons de communiquer cette URL à vos utilisateurs. Une méthode recommandée est de publier cette URL dans un message personnalisé de refus d’accès à une application dans AD FS. Celle-ci est décrite dans la section à venir : [Créer une stratégie d’accès à une application et un message de refus d’accès personnalisé](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2. Connectez-vous à la page Web à l’aide d’un compte de domaine d’entreprise : <strong>roberth@contoso.com</strong> et mot de passe : <strong>P@ssword</strong>.

3. Vous êtes invité à installer un profil. Dans l'écran **Installer un profil** , cliquez sur **Installer**.

4. Quand vous êtes invité à confirmer l'installation du profil, cliquez sur **Installer maintenant**.

5. Le cas échéant, entrez votre code confidentiel pour déverrouiller votre appareil.

6. L'installation du profil est terminée quand apparaît l'écran **Profil installé**. Cliquez sur **Terminé**.

   Revenez à Safari. Un message vous informe que vous pouvez fermer ou quitter Safari.

> [!TIP]
> Pour afficher ou supprimer le profil de jonction d'espace de travail, accédez à **Paramètres**, cliquez sur **Général**, puis cliquez sur **Profils** sur votre appareil iOS.

## <a name="see-also"></a>Voir aussi


- [Joindre un espace de travail à partir de n’importe quel appareil en utilisant l’authentification unique et l’authentification de second facteur transparente](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Procédure pas à pas : Workplace Join avec un appareil Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



