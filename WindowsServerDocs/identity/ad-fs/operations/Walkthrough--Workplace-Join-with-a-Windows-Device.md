---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: "Procédure pas à pas: joindre un espace de travail avec un appareil Windows"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fd222eb47982591e051594e8a572443b65c0357f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>Procédure pas à pas: Joindre un espace de travail avec un appareil Windows

>S’applique à: Windows Server2016, Windows Server2012R2

Cette rubrique montre comment utiliser la jonction d’espace pour connecter votre appareil Windows avec votre espace de travail et comment accéder à une application web à l’aide de l’authentification unique. Vous devez effectuer les étapes décrites dans le [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) section avant de suivre cette procédure.

## <a name="access-the-web-application-before-device-registration"></a>Accéder à l’application web avant l’inscription de périphérique
Dans cette procédure pas à pas, vous accéder à une application web d’entreprise avant de joindre votre appareil à l’espace de travail. La page Web affiche les revendications qui ont été incluses dans votre jeton de sécurité. Notez que la liste de revendications n’inclut pas toutes les informations concernant votre appareil. Peut-être remarquerez-vous aussi que vous n’avez pas Single Sign-On.

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>Pour accéder à l’application web avant d’utiliser la jonction sur votre appareil

1.  Connectez-vous à Client1 avec votre compte Microsoft.

2.  Ouvrez Internet Explorer et accédez à votre application de revendications générique, **https://webserv1.contoso.com/claimapp**.

3.  Ouvrez une session sur la page Web à l’aide d’un compte de domaine d’entreprise: ** roberth@contoso.com **, mot de passe: ** P@ssword **.

4.  La page Web répertorie toutes les revendications dans votre jeton de sécurité. Uniquement les revendications d’utilisateur sont présentes dans votre jeton de sécurité.

5.  Fermez Internet Explorer.

6.  Ouvrez Internet Explorer et accédez à la même application de revendications, **https://webserv1.contoso.com/claimapp**.

7.  Notez que vous êtes invité à entrer de nouveau vos informations d’identification. Vous n’êtes pas connecté à l’espace de travail à partir d’un appareil avec la jonction et ne disposez donc pas Single Sign-On.

## <a name="join-your-device-with-workplace-join"></a>Joindre votre appareil avec l’espace de travail

> [!IMPORTANT]
> Pour la jonction d’espace de travail réussisse, l’ordinateur client (Client1) doit approuver le certificat SSL qui a été utilisé pour configurer les Services de fédération Active Directory (AD FS) dans [étape 2: configurer le serveur de fédération avec Device Registration Service (ADFS1)](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Il doit également être en mesure de valider les informations de révocation du certificat. Si vous rencontrez des problèmes avec la jonction d’espace, vous pouvez afficher le journal des événements sur Client1.
> 
> Pour afficher le journal des événements, ouvrez l’Observateur d’événements, développez **journaux des Applications et Services**, développez **Microsoft**, développez **Windows**, puis cliquez sur **jonction**.

#### <a name="to-join-your-device-with-workplace-join"></a>Pour joindre votre appareil avec la jonction d’espace

1.  Connectez-vous à Client1 avec votre compte Microsoft.

2.  Sur le **Démarrer** écran, ouvrez le **icônes** barre, puis sélectionnez le **paramètres** icône. Sélectionnez **modifier les paramètres du PC**.

3.  Sur le **les paramètres du PC** page, sélectionnez **réseau**, puis cliquez sur **espace de travail**.

4.  Dans le **Entrez votre nom d’utilisateur pour obtenir l’accès de l’espace de travail ou désactiver la gestion des périphériques** , tapez ** roberth@contoso.com **, puis cliquez sur **joindre**.

5.  Lorsque vous êtes invité pour des informations d’identification, tapez ** roberth@contoso.com **et le mot de passe: ** P@ssword **. Cliquez sur **OK**.

6.  Vous devez maintenant voir le message: «cet appareil est joint au réseau de votre espace de travail».

### <a name="access-the-web-application-after-joining-the-workplace"></a>Accéder à l’application web après avoir joint l’espace de travail
Dans cette partie de la démonstration, vous accédez à une application web d’entreprise à partir de votre appareil est connecté à l’espace de travail. La page Web affiche les revendications qui ont été incluses dans votre jeton de sécurité. Notez que la liste de revendications comprend des informations de périphérique et utilisateur. Peut-être remarquerez-vous aussi que vous disposez maintenant Single Sign-On.

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>Pour accéder à l’application web après avoir joint l’espace de travail

1.  Ouvrez une session sur **Client1** avec votre compte Microsoft.

2.  Ouvrez Internet Explorer et accédez à votre application de revendications générique, **https://webserv1.contoso.com/claimapp**.

3.  Ouvrez une session sur la page Web à l’aide d’un compte de domaine d’entreprise: ** roberth@contoso.com **, mot de passe: ** P@ssword **.

4.  La page Web répertorie les revendications dans votre jeton de sécurité. Votre jeton contient les revendications d’utilisateur et périphérique.

5.  Fermez Internet Explorer.

6.  Ouvrez Internet Explorer et accédez à la même application de revendications, **https://webserv1.contoso.com/claimapp**.

7.  Notez que vous êtes **pas** nouveau invité à entrer vos informations d’identification. Vous êtes connecté à partir d’un appareil avec la jonction et disposez par conséquent Single Sign-On.

## <a name="see-also"></a>Voir aussi
[Rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur d’authentification entre les Applications d’entreprise](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
[procédure pas à pas: espace de travail avec un appareil iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



