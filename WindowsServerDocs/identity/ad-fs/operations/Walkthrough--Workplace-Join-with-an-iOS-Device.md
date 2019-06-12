---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: Procédure pas à pas - jonction avec un appareil iOS
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 979802469737066612bc6242f942fd3acd077479
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444793"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Démonstration : joindre un espace de travail avec un appareil iOS


> [!IMPORTANT] 
> Cette méthode concerne uniquement les clients en local. Hybride ou cloud uniquement des clients doivent utiliser pas cette méthode pour inscrire leurs appareils iOS. Et cette méthode n’est pas compatible lorsque les clients locaux décider de déplacer vers le cloud. L’appareil doit être désinscrit et inscrit avec le cloud. 

Cette rubrique explique comment joindre un espace de travail avec un appareil iOS. Vous devez effectuer les étapes décrites dans le [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) section avant de suivre cette procédure. Vous pouvez utiliser l’appareil pour accéder à la même application web d’entreprise que vous avez accédé dans [procédure pas à pas : Espace de travail avec un appareil Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).


## <a name="join-an-ios-device-with-workplace-join"></a>Joindre un espace de travail avec un appareil iOS

> [!IMPORTANT]
> Lorsque DRS local est configuré, l’appareil iOS doit approuver le certificat de la couche SSL (Secure Socket) qui a été utilisé pour configurer Active Directory Federation Services (ADFS) dans [étape 2 : Configurer le serveur de fédération (ADFS1) avec Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4), pour la jonction aboutisse.
> 
> -   Si le certificat SSL AD FS a été émis depuis une autorité de certification de test, vous devez installer le certificat de l'autorité de certification sur votre appareil iOS.
> -   Si votre certificat d'autorité de certification est publié sur un site web, vous pouvez accéder à ce dernier depuis votre appareil iOS et installer le certificat.

Cette démonstration indique comment joindre l'appareil à l'espace de travail.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Pour joindre un appareil iOS à un espace de travail

1. -   **Lorsque le service d’inscription de périphérique Azure Active Directory est le service DRS configuré :** Ouvrez Apple Safari et accédez au point de terminaison du profil de l’air Azure Active Directory Device Registration service pour les appareils iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > où <`yourdomainname`> est le nom de domaine que vous avez configuré avec Azure Active Directory. Par exemple, si votre nom de domaine est contoso.com, l’URL serait : `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

   -   **Lorsque DRS local est le service DRS configuré**: Ouvrez Apple Safari et accédez au point de terminaison de Service DRS (Device Registration) air profil pour les appareils iOS, `https://adf1s.contoso.com/enrollmentserver/otaprofile`

   Il existe de nombreuses façons de communiquer cette URL à vos utilisateurs. Une méthode recommandée est de publier cette URL dans un message personnalisé de refus d’accès à une application dans AD FS. Celle-ci est décrite dans la section à venir : [Créer une stratégie d’accès à une application et un message de refus d’accès personnalisé](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2. Ouvrez une session sur la page Web à l’aide d’un compte de domaine de société : <strong>roberth@contoso.com</strong> et mot de passe : <strong>P@ssword</strong>.

3. Vous êtes invité à installer un profil. Dans l'écran **Installer un profil** , cliquez sur **Installer**.

4. Quand vous êtes invité à confirmer l'installation du profil, cliquez sur **Installer maintenant**.

5. Le cas échéant, entrez votre code confidentiel pour déverrouiller votre appareil.

6. L'installation du profil est terminée quand apparaît l'écran **Profil installé**. Cliquez sur **Terminé**.

   Revenez à Safari. Un message vous informe que vous pouvez fermer ou quitter Safari.

> [!TIP]
> Pour afficher ou supprimer le profil de jonction d'espace de travail, accédez à **Paramètres**, cliquez sur **Général**, puis cliquez sur **Profils** sur votre appareil iOS.

## <a name="see-also"></a>Voir aussi


- [Rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente du facteur d’authentification entre les Applications d’entreprise](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Démonstration : Joindre un espace de travail avec un appareil Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



