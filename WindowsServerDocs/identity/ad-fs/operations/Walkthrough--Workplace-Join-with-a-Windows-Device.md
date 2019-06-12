---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: Procédure pas à pas - jonction avec un appareil Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 83dab73230737b8303660b32683a6d05322ac754
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444771"
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>Démonstration : joindre un espace de travail avec un appareil Windows

Cette rubrique montre comment utiliser la jonction d'espace de travail pour connecter votre appareil Windows à votre espace de travail et comment accéder à une application web à l'aide de l'authentification unique. Vous devez effectuer les étapes décrites dans le [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) section avant de suivre cette procédure.

## <a name="access-the-web-application-before-device-registration"></a>Accéder à l'application web avant d'inscrire l'appareil
Dans cette procédure pas à pas, vous accédez à une application web d'entreprise avant de joindre votre appareil à l'espace de travail. La page web affiche les revendications qui ont été incluses dans votre jeton de sécurité. Notez que la liste de revendications ne comprend aucune information sur votre appareil. Peut-être remarquerez-vous aussi que vous ne disposez pas de l'authentification unique.

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>Pour accéder à l'application web avant d'utiliser la jonction d'espace de travail sur votre appareil

1. Connectez-vous à Client1 avec votre compte Microsoft.

2. Ouvrez Internet Explorer et accédez à votre application de revendications générique, **https://webserv1.contoso.com/claimapp** .

3. Ouvrez une session sur la page Web à l’aide d’un compte de domaine de société : <strong>roberth@contoso.com</strong>, mot de passe : <strong>P@ssword</strong>.

4. La page web répertorie toutes les revendications incluses dans votre jeton de sécurité. Ce dernier ne contient que des revendications utilisateur.

5. Fermez Internet Explorer.

6. Ouvrez Internet Explorer et accédez à la même application de revendications, **https://webserv1.contoso.com/claimapp** .

7. Vous êtes de nouveau invité à entrer vos informations d'identification. Comme vous n'êtes pas connecté à l'espace de travail depuis un appareil au moyen de la jonction d'espace de travail, vous ne disposez pas de l'authentification unique.

## <a name="join-your-device-with-workplace-join"></a>Rattacher votre appareil au moyen de la jonction d'espace de travail

> [!IMPORTANT]
> Pour la jonction aboutisse, l’ordinateur client (Client1) doit approuver le certificat SSL qui a été utilisé pour configurer Active Directory Federation Services (ADFS) dans [étape 2 : Configurer le serveur de fédération avec Device Registration Service (ADFS1)](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Il doit également être en mesure de valider les informations de révocation pour le certificat. Si vous rencontrez des problèmes avec la jonction d’espace de travail, consultez le journal des événements sur Client1.
> 
> Pour voir le journal des événements, ouvrez l'Observateur d'événements, développez **Journaux des applications et des services**, **Microsoft**, **Windows**, puis cliquez sur **Jonction d'espace de travail**.

#### <a name="to-join-your-device-with-workplace-join"></a>Pour rattacher votre appareil au moyen de la jonction d'espace de travail

1. Connectez-vous à Client1 avec votre compte Microsoft.

2. Dans l'écran d'**accueil**, ouvrez la barre d'**icônes**, puis sélectionnez l'icône **Paramètres**. Sélectionnez **Modifier les paramètres du PC**.

3. Dans la page **Paramètres du PC**, sélectionnez **Réseau**, puis cliquez sur **Lieu de travail**.

4. Dans le **Entrez votre ID d’utilisateur pour obtenir l’accès de l’espace de travail ou désactiver la gestion des appareils** , tapez <strong>roberth@contoso.com</strong>, puis cliquez sur **joindre**.

5. Lorsque vous y êtes invité pour les informations d’identification, tapez <strong>roberth@contoso.com</strong>et le mot de passe : <strong>P@ssword</strong>. Cliquez sur **OK**.

6. Le message suivant doit maintenant s'afficher : « Cet appareil s'est joint au réseau de votre lieu de travail. »

### <a name="access-the-web-application-after-joining-the-workplace"></a>Accéder à l'application web après avoir joint l'espace de travail
Dans cette partie de la démonstration, vous accédez à une application web d'entreprise depuis votre appareil connecté au moyen de la jonction d'espace de travail. La page web affiche les revendications qui ont été incluses dans votre jeton de sécurité. Notez que la liste de revendications comprend les informations de l'appareil et les informations utilisateur. Peut-être remarquerez-vous aussi que vous disposez maintenant de l'authentification unique.

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>Pour accéder à l'application web après avoir joint l'espace de travail

1. Connectez-vous à **Client1** avec votre compte Microsoft.

2. Ouvrez Internet Explorer et accédez à votre application de revendications générique, **https://webserv1.contoso.com/claimapp** .

3. Ouvrez une session sur la page Web à l’aide d’un compte de domaine de société : <strong>roberth@contoso.com</strong>, mot de passe : <strong>P@ssword</strong>.

4. La page web répertorie les revendications incluses dans votre jeton de sécurité. Ce dernier contient à la fois des revendications utilisateur et des revendications de périphérique.

5. Fermez Internet Explorer.

6. Ouvrez Internet Explorer et accédez à la même application de revendications, **https://webserv1.contoso.com/claimapp** .

7. Vous **n'êtes pas** de nouveau invité à entrer vos informations d'identification. Comme vous êtes connecté depuis un appareil au moyen de la jonction d'espace de travail, vous disposez de l'authentification unique.

## <a name="see-also"></a>Voir aussi
[Rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur Authentication Across Company Applications](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) 
 [ Procédure pas à pas : Joindre un espace de travail avec un appareil iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



