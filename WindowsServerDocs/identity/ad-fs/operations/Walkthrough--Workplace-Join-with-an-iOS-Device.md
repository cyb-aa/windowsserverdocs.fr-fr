---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 'Procédure pas à pas: joindre un espace de travail avec un appareil iOS'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a8643bab913dfec07c6bbea0c068e1240f16c5b
ms.sourcegitcommit: a2260c96b0e49519d180c7382b921ce8ddb053fe
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Procédure pas à pas: Jonction avec un appareil iOS

>S’applique à: Windows Server2012R2

Cette rubrique explique la jonction d’espace sur un périphérique iOS. Vous devez effectuer les étapes décrites dans le [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) section avant de suivre cette procédure. Vous pouvez utiliser l’appareil pour accéder à la même application web d’entreprise que dans le cadre [procédure pas à pas: espace de travail avec un appareil Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).

## <a name="join-an-ios-device-with-workplace-join"></a>Joindre un appareil iOS avec la jonction

> [!IMPORTANT]
> Lorsque le service DRS sur site est configuré, le périphérique iOS doit approuver le certificat de Secure Socket Layer (SSL) qui a été utilisé pour configurer les Services de fédération Active Directory (AD FS) dans [étape 2: configurer le serveur de fédération (ADFS1) avec Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4), pour la jonction d’espace de travail réussisse.
> 
> -   Si le certificat SSL AD FS a été émis par une autorité de certification (CA) de test, vous devez installer le certificat d’autorité de certification sur votre appareil iOS.
> -   Si votre certificat d’autorité de certification est publié sur un site Web, vous pouvez naviguer vers le site Web à partir de votre appareil iOS et installer le certificat.

Dans cette démonstration, vous joignez l’appareil à l’espace de travail.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Pour joindre un appareil iOS à un espace de travail

1.  -   **Lorsque le service d’inscription de l’appareil Azure Active Directory est le service DRS configuré:** ouvrez Apple Safari et accédez au point de terminaison du profil air Azure Active Directory Device Registration service pour les périphériques iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > où <`yourdomainname`> est le nom de domaine que vous avez configuré avec Azure Active Directory. Par exemple, si votre nom de domaine est contoso.com, l’URL serait: `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

    -   **Lorsque DRS local est le service DRS configuré**: ouvrez Apple Safari et accédez au point de terminaison Service DRS (Device Registration Service) air profil pour les périphériques iOS,`https://adf1s.contoso.com/enrollmentserver/otaprofile`

    Il existe de nombreuses façons de communiquer cette URL pour vos utilisateurs. Une méthode recommandée est de publier cette URL dans un accès de l’application personnalisée refusé message dans AD FS. Celle-ci est décrite dans la section à venir: [créer une stratégie d’accès application et un message de refus d’accès personnalisé](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2.  Ouvrez une session sur la page Web à l’aide d’un compte de domaine d’entreprise: ** roberth@contoso.com ** et mot de passe: ** P@ssword **.

3.  Vous êtes invité à installer un profil. Sur le **installer un profil** , cliquez sur **installer**.

4.  Lorsque vous êtes invité à confirmer l’installation du profil, cliquez sur **installer maintenant**.

5.  Si votre appareil nécessite un code confidentiel déverrouiller l’appareil, vous êtes invité à entrer votre code confidentiel.

6.  L’installation du profil est terminée lorsque vous verrez le **profil installé** écran. Cliquez sur **fait**.

    Revenez à Safari. Un message vous informe que vous pouvez fermer ou quitter Safari.

> [!TIP]
> Pour afficher ou supprimer le profil de l’espace de travail, accédez à **paramètres**, cliquez sur **général**, puis cliquez sur **profils** sur votre appareil iOS.

## <a name="see-also"></a>Voir aussi


- [Rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente à deux facteurs d’authentification entre les Applications d’entreprise](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configurer l’environnement lab pour ADFS dans Windows Server2012R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Procédure pas à pas: Joindre un espace de travail avec un appareil Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



