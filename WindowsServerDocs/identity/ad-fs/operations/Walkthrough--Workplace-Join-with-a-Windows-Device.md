---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: 'Procédure pas à pas : Workplace Join avec un appareil Windows'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9867e11aa659be9aff9912780e1186a796a7232e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357772"
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>Procédure pas à pas : joindre un espace de travail avec un appareil Windows

Cette rubrique montre comment utiliser la jonction d'espace de travail pour connecter votre appareil Windows à votre espace de travail et comment accéder à une application web à l'aide de l'authentification unique. Vous devez effectuer les étapes décrites dans la section [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2 avant de](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) pouvoir essayer cette procédure pas à pas.

## <a name="access-the-web-application-before-device-registration"></a>Accéder à l'application web avant d'inscrire l'appareil
Dans cette procédure pas à pas, vous accédez à une application web d'entreprise avant de joindre votre appareil à l'espace de travail. La page web affiche les revendications qui ont été incluses dans votre jeton de sécurité. Notez que la liste de revendications ne comprend aucune information sur votre appareil. Peut-être remarquerez-vous aussi que vous ne disposez pas de l'authentification unique.

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>Pour accéder à l'application web avant d'utiliser la jonction d'espace de travail sur votre appareil

1. Connectez-vous à Client1 avec votre compte Microsoft.

2. Ouvrez Internet Explorer et accédez à votre application de revendications génériques, **https://webserv1.contoso.com/claimapp** .

3. Connectez-vous à la page Web à l’aide d’un compte de domaine d’entreprise : <strong>roberth@contoso.com</strong>, mot de passe : <strong>P@ssword</strong>.

4. La page web répertorie toutes les revendications incluses dans votre jeton de sécurité. Ce dernier ne contient que des revendications utilisateur.

5. Fermez Internet Explorer.

6. Ouvrez Internet Explorer et accédez à la même application de revendications, **https://webserv1.contoso.com/claimapp** .

7. Vous êtes de nouveau invité à entrer vos informations d'identification. Comme vous n'êtes pas connecté à l'espace de travail depuis un appareil au moyen de la jonction d'espace de travail, vous ne disposez pas de l'authentification unique.

## <a name="join-your-device-with-workplace-join"></a>Rattacher votre appareil au moyen de la jonction d'espace de travail

> [!IMPORTANT]
> Pour que Workplace Join aboutisse, l’ordinateur client (CLIENT1) doit approuver le certificat SSL utilisé pour configurer Services ADFS (AD FS) dans [Step 2 : Configurez le serveur de Fédération avec Device Registration service (ADFS1) ](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Il doit également être en mesure de valider les informations de révocation pour le certificat. Si vous rencontrez des problèmes avec la jonction d’espace de travail, consultez le journal des événements sur Client1.
> 
> Pour voir le journal des événements, ouvrez l'Observateur d'événements, développez **Journaux des applications et des services**, **Microsoft**, **Windows**, puis cliquez sur **Jonction d'espace de travail**.

#### <a name="to-join-your-device-with-workplace-join"></a>Pour rattacher votre appareil au moyen de la jonction d'espace de travail

1. Connectez-vous à Client1 avec votre compte Microsoft.

2. Dans l'écran d'**accueil**, ouvrez la barre d'**icônes**, puis sélectionnez l'icône **Paramètres**. Sélectionnez **Modifier les paramètres du PC**.

3. Dans la page **Paramètres du PC**, sélectionnez **Réseau**, puis cliquez sur **Lieu de travail**.

4. Dans la zone **Entrez votre ID d’utilisateur pour accéder à l’espace de travail ou activer la gestion des périphériques** , tapez <strong>roberth@contoso.com</strong>, puis cliquez sur **joindre**.

5. Lorsque vous êtes invité à entrer vos informations d’identification, tapez <strong>roberth@contoso.com</strong>et le mot de passe : <strong>P@ssword</strong>. Cliquez sur **OK**.

6. Le message suivant doit maintenant s'afficher : « Cet appareil s'est joint au réseau de votre lieu de travail. »

### <a name="access-the-web-application-after-joining-the-workplace"></a>Accéder à l'application web après avoir joint l'espace de travail
Dans cette partie de la démonstration, vous accédez à une application web d'entreprise depuis votre appareil connecté au moyen de la jonction d'espace de travail. La page web affiche les revendications qui ont été incluses dans votre jeton de sécurité. Notez que la liste de revendications comprend les informations de l'appareil et les informations utilisateur. Peut-être remarquerez-vous aussi que vous disposez maintenant de l'authentification unique.

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>Pour accéder à l'application web après avoir joint l'espace de travail

1. Connectez-vous à **Client1** avec votre compte Microsoft.

2. Ouvrez Internet Explorer et accédez à votre application de revendications génériques, **https://webserv1.contoso.com/claimapp** .

3. Connectez-vous à la page Web à l’aide d’un compte de domaine d’entreprise : <strong>roberth@contoso.com</strong>, mot de passe : <strong>P@ssword</strong>.

4. La page web répertorie les revendications incluses dans votre jeton de sécurité. Ce dernier contient à la fois des revendications utilisateur et des revendications de périphérique.

5. Fermez Internet Explorer.

6. Ouvrez Internet Explorer et accédez à la même application de revendications, **https://webserv1.contoso.com/claimapp** .

7. Vous **n'êtes pas** de nouveau invité à entrer vos informations d'identification. Comme vous êtes connecté depuis un appareil au moyen de la jonction d'espace de travail, vous disposez de l'authentification unique.

## <a name="see-also"></a>Voir aussi
[Rejoindre l’espace de travail à partir de n’importe quel appareil pour l’authentification unique et l’authentification de second facteur transparente entre les applications](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)de l’entreprise 
[configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
 @ no__t-4Walkthrough : Joindre un espace de travail avec un appareil iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



