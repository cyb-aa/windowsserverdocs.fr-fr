---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: 'Guide pas à pas : gérer les risques avec des Multi-Factor Authentication supplémentaires pour les applications sensibles'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 08aadcf0322fcb937bdde17d18aa5d30e3da68ce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357788"
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Guide pas à pas : gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles




## <a name="about-this-guide"></a>À propos de ce guide
Cette procédure pas à pas fournit des instructions pour la configuration de l’authentification multifacteur (MFA) dans Services ADFS (AD FS) dans Windows Server 2012 R2 en fonction des données d’appartenance au groupe de l’utilisateur.

Pour plus d’informations sur l’authentification multifacteur et les mécanismes d’authentification dans AD FS, consultez [gérer les risques avec des multi-Factor Authentication supplémentaires pour les applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

Cette procédure pas à pas comporte les sections suivantes :

-   [Étape 1 : configuration de l’environnement Lab](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Étape 2 : vérifier le mécanisme d’authentification par défaut AD FS](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [Étape 3 : configurer l’authentification multifacteur sur votre serveur de Fédération](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [Étape 4 : vérifier le mécanisme MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>Étape 1 : configuration de l’environnement Lab
Afin de mener à bien cette procédure pas à pas, vous avez besoin d’un environnement constitué des composants suivants :

-   Un domaine Active Directory avec un compte d’utilisateur et de groupe de test, exécuté sur Windows Server 2012 R2 ou un domaine Active Directory s’exécutant sur Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012, avec son schéma mis à niveau vers Windows Server 2012 R2

-   Un serveur de Fédération s’exécutant sur Windows Server 2012 R2

-   un serveur Web qui héberge votre exemple d’application ;

-   un ordinateur client à partir duquel vous pouvez accéder à l’exemple d’application.

> [!WARNING]
> Il est vivement recommandé (à la fois dans les environnements de production et de test) de ne pas utiliser le même ordinateur comme serveur de fédération et serveur Web.

Dans cet environnement, le serveur de fédération émet les revendications requises afin que les utilisateurs puissent accéder à l’exemple d’application. Le serveur Web héberge un exemple d’application qui approuve les utilisateurs qui présentent les revendications émises par le serveur de fédération.

Pour obtenir des instructions sur la façon de configurer cet environnement, consultez [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Étape 2 : vérifier le mécanisme d’authentification par défaut AD FS
Au cours de cette étape, vous allez vérifier le mécanisme de contrôle d’accès AD FS par défaut (**Authentification par formulaires** pour l’extranet et **Authentification Windows** pour l’intranet), où l’utilisateur est redirigé vers la page de connexion AD FS, fournit des informations d’identification valides et est autorisé à accéder à l’application. Vous pouvez utiliser le compte **Robert Hatley** ad et l’exemple d’application **ClaimApp** que vous avez configurés dans Configuration de [l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

1.  Sur votre ordinateur client, ouvrez une fenêtre de navigateur et accédez à votre exemple d’application : **https://webserv1.contoso.com/claimapp** .

    Cette action redirige automatiquement la demande vers le serveur de fédération et vous êtes invité à vous connecter avec un nom d’utilisateur et un mot de passe.

2.  Tapez les informations d’identification du compte **Robert Hatley** ad que vous avez créé dans [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Vous êtes autorisé à accéder à l’application.

## <a name="BKMK_3"></a>Étape 3 : configurer l’authentification multifacteur sur votre serveur de Fédération
Il existe deux parties à la configuration de l’authentification multifacteur dans AD FS dans Windows Server 2012 R2 :

-   [Sélectionner une méthode d’authentification supplémentaire](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [Configurer la stratégie d’authentification multifacteur](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>Sélectionner une méthode d’authentification supplémentaire
Afin de configurer l’authentification multifacteur, vous devez sélectionner une méthode d’authentification supplémentaire. Dans cette procédure pas à pas, pour obtenir une méthode d’authentification supplémentaire, vous avez le choix entre les options suivantes :

-   Sélectionnez la méthode [d’authentification par certificat](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7) qui est disponible dans AD FS dans Windows Server 2012 R2 par défaut

-   Configurez et sélectionnez [Windows Azure Multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>Authentification par certificat
Effectuez l’une des procédures suivantes pour sélectionner l’authentification par certificat comme méthode d’authentification supplémentaire :

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>Pour configurer l’authentification par certificat comme méthode d’authentification supplémentaire via la console de gestion AD FS

1.  Sur votre serveur de fédération, dans la console de gestion AD FS, accédez au nœud **Stratégies d’authentification** et, sous la section **Authentification multifacteur** , cliquez sur le lien **Modifier** situé en regard de la sous-section **Paramètres globaux** .

2.  Dans la fenêtre **Modifier la stratégie d’authentification globale**, sélectionnez **Authentification par certificat** comme méthode d’authentification supplémentaire, puis cliquez sur **OK**.

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>Pour configurer l’authentification par certificat comme méthode d’authentification supplémentaire via Windows PowerShell

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante :

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > Pour vérifier que cette commande s’est correctement exécutée, vous pouvez exécuter la commande `Get-AdfsGlobalAuthenticationPolicy` .

#### <a name="BKMK_8"></a>Multi-Factor Authentication Windows Azure
Effectuez les procédures suivantes pour télécharger, configurer et sélectionner **Windows Azure Multi-Factor Authentication** comme authentification supplémentaire sur votre serveur de fédération :

1.  [Créer un fournisseur de Multi-Factor Authentication par le biais du portail Windows Azure](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [Télécharger le serveur Multi-Factor Authentication Windows Azure](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [Installer le serveur Multi-Factor Authentication Windows Azure sur votre serveur de Fédération](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [Configurer Windows Azure Multi-Factor Authentication comme méthode d’authentification supplémentaire](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>Créer un fournisseur de Multi-Factor Authentication par le biais du portail Windows Azure

1.  Ouvrez une session sur le portail Windows Azure en tant qu’administrateur.

2.  Sur la gauche, sélectionnez Active Directory.

3.  En haut de la page Active Directory, sélectionnez **Fournisseurs d’authentification multifacteur**.  Ensuite, au bas de la page, cliquez sur **Nouveau**.

4.  Sous **Services d’application->Active Directory**, sélectionnez **Fournisseur d’authentification multifacteur**, puis **Création rapide**.

5.  Sous **Services d’application**, sélectionnez **Fournisseurs d’authentification Active Directory**, puis **Création rapide**.

6.  Remplissez les champs suivants et sélectionnez **Créer**.

    1.  **Nom** : nom du fournisseur multi-Factor Authentication.

    2.  **Modèle d’utilisation** : modèle d’utilisation du fournisseur multi-Factor Authentication.

        -   **Par** authentification : modèle d’achat facturé par authentification. Modèle généralement utilisé pour les scénarios qui emploient Windows Azure Multi-Factor Authentication dans une application réservée aux consommateurs.

        -   **Par utilisateur activé** : modèle d’achat facturé par utilisateur activé.  Modèle généralement utilisé pour les scénarios liés aux employés, par exemple Office 365.

        Pour plus d’informations sur les modèles d’utilisation, voir [Tarification Windows Azure](http://www.windowsazure.com/pricing/details/active-directory/).

    3.  **Directory** : locataire Windows Azure Active Directory auquel le fournisseur de Multi-Factor Authentication est associé. Ce champ est facultatif, car le fournisseur ne doit pas nécessairement être lié à Windows Azure Active Directory lors de la sécurisation des applications sur site.

7.  Lorsque vous cliquez sur Créer, le fournisseur d’authentification multifacteur est créé et un message qui indique  « Le fournisseur d’authentification multifacteur a été créé correctement » doit s’afficher.  Cliquez sur **OK**.

Vous devez ensuite télécharger le serveur Windows Azure Multi-Factor Authentication. Pour ce faire, vous pouvez lancer le portail Windows Azure Multi-Factor Authentication via le portail Windows Azure.

##### <a name="BKMK_b"></a>Télécharger le serveur Multi-Factor Authentication Windows Azure

1.  Ouvrez une session sur le portail Windows Azure en tant qu’administrateur et cliquez sur le fournisseur d’authentification multifacteur que vous avez créé dans la procédure ci-dessus. Cliquez ensuite sur le bouton **Gérer** .

    Le portail **Windows Azure Multi-Factor Authentication** est lancé.

2.  Dans le portail **Windows Azure Multi-Factor Authentication** , cliquez sur **Téléchargements**, puis sur **Télécharger** pour télécharger une copie du serveur Windows Azure Multi-Factor Authentication.

Une fois que vous avez téléchargé l’exécutable pour le serveur Windows Azure Multi-Factor Authentication, vous devez l’installer sur votre serveur de fédération.

##### <a name="BKMK_c"></a>Installer le serveur Multi-Factor Authentication Windows Azure sur votre serveur de Fédération

1.  Téléchargez l’exécutable et double-cliquez dessus pour le serveur Windows Azure Multi-Factor Authentication.  L’installation commence alors.

2.  Sur l’écran du contrat de licence, lisez le contrat, sélectionnez **J’accepte** et cliquez sur **Suivant**.

3.  Vérifiez que le dossier de destination est correct, puis cliquez sur **Suivant**.

4.  Lorsque l’installation est terminée, cliquez sur **Terminer**.

Vous êtes maintenant prêt à lancer le serveur Windows Azure Multi-Factor Authentication que vous avez installé sur votre serveur de fédération et à le configurer comme méthode d’authentification supplémentaire.

##### <a name="BKMK_d"></a>Configurer Windows Azure Multi-Factor Authentication comme méthode d’authentification supplémentaire

1.  Lancez **Windows Azure Multi-Factor Authentication** à partir de l’emplacement où vous l’avez installé sur votre serveur de fédération et, sur la page d’accueil, activez la case à cocher **Ignorer l’Assistant Configuration de l’authentification** et cliquez sur **Suivant**.

2.  Pour activer le serveur Multi-Factor Authentication, revenez à la page du portail de gestion de l’authentification multifacteur où vous avez téléchargé le serveur Multi-Factor Authentication et cliquez sur le bouton **Générer des informations d’identification d’activation**. Dans l’interface utilisateur du serveur Multi-Factor Authentication, entrez les informations d’identification générées et cliquez sur **Activer**.

3.  Ensuite, l’interface utilisateur du **serveur Multi-Factor Authentication** vous invite à exécuter l’**Assistant Configuration de serveurs multiples**.  Sélectionnez **Non**.

    > [!IMPORTANT]
    > Vous pouvez ignorer l’exécution de l’**Assistant Configuration de serveurs multiples** en raison de l’environnement lab avec un seul serveur de fédération qui est utilisé pour effectuer cette procédure pas à pas. Toutefois, si votre environnement contient plusieurs serveurs de fédération, vous devez installer le serveur Multi-Factor Authentication et exécuter l’ **Assistant Configuration de serveurs multiples** sur chaque serveur de fédération afin d’activer la réplication entre les serveurs Multi-Factor Authentication qui sont exécutés sur les serveurs de fédération.

4.  Dans l’interface utilisateur du **serveur Multi-Factor Authentication** , sélectionnez l’icône **Utilisateurs** , cliquez sur **Importer à partir d’Active Directory**, sélectionnez le compte **Robert Hatley** à configurer dans Windows Azure Multi-Factor Authentication, puis cliquez sur **Importer**.

5.  Dans la liste **Utilisateurs**, sélectionnez le compte **Robert Hatley**, cliquez sur **Modifier** et, dans la fenêtre **Modifier un utilisateur**, indiquez un numéro de téléphone portable pour ce compte, vérifiez que la case à cocher **Activé** est activée, puis cliquez sur **Appliquer**.

6.  Dans la liste **Utilisateurs**, sélectionnez le compte **Robert Hatley**, puis cliquez sur **Tester**. Dans la fenêtre **Tester un utilisateur**, indiquez les informations d’identification du compte **Robert Hatley**. Lorsque le téléphone portable sonne, appuyez sur « # » pour terminer la vérification du compte.

7.  Dans l’interface utilisateur du **serveur Multi-Factor Authentication**, sélectionnez l’icône **AD FS**, vérifiez que les cases à cocher **Autoriser l’inscription utilisateur**, **Autoriser les utilisateurs à sélectionner une méthode** (notamment **Appel téléphonique** et **SMS**), **Utiliser les questions de sécurité de secours** et **Activer la journalisation** sont activées, cliquez sur **Installer l’adaptateur AD FS**, puis exécutez l’Assistant d’installation de l’**adaptateur AD FS Multi-Factor Authentication**.

    > [!NOTE]
    > L’Assistant d’installation de l’ **adaptateur AD FS Multi-Factor Authentication** crée un groupe de sécurité appelé **APhoneFactor Admins** dans Active Directory, puis ajoute le compte de service AD FS de votre service de fédération à ce groupe.
    > 
    > Il est conseillé de vérifier sur votre contrôleur de domaine que le groupe **PhoneFactor Admins** est bien créé et que le compte de service AD FS est un membre de ce groupe.
    > 
    > Si nécessaire, ajoutez le compte de service AD FS au groupe **PhoneFactor Admins** sur votre contrôleur de domaine manuellement.

    Pour plus d’informations sur l’installation de l’adaptateur AD FS, cliquez sur le lien de l’aide dans le coin supérieur droit du serveur Multi-Factor Authentication.

8.  Pour inscrire l’adaptateur dans votre service de fédération, sur votre serveur de fédération, lancez la fenêtre de commande Windows PowerShell, puis exécutez la commande suivante : `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`. L’adaptateur est maintenant inscrit en tant que **WindowsAzureMultiFactorAuthentication**.  Vous devez redémarrer votre service AD FS pour que l’inscription entre en vigueur.

9. Pour configurer Windows Azure Multi-Factor Authentication comme méthode d’authentification supplémentaire, dans la console de gestion AD FS, accédez au nœud **Stratégies d’authentification** et, sous la section **Authentification multifacteur** , cliquez sur le lien **Modifier** situé en regard de la sous-section **Paramètres globaux** . Dans la fenêtre **Modifier la stratégie d’authentification globale**, sélectionnez **Authentification multifacteur** comme méthode d’authentification supplémentaire, puis cliquez sur **OK**.

    > [!NOTE]
    > Vous pouvez personnaliser le nom et la description de la méthode Windows Azure Multi-Factor Authentication ainsi que toute méthode d’authentification tierce configurée telle qu’elle s’affiche dans votre interface utilisateur AD FS en exécutant l’applet de commande **Set-AdfsAuthenticationProviderWebContent** . Pour plus d’informations, consultez [https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>Configurer la stratégie d’authentification multifacteur
Afin d’activer l’authentification multifacteur, vous devez configurer la stratégie d’authentification multifacteur sur votre serveur de fédération. Pour cette procédure pas à pas, conformément à notre stratégie d’authentification multifacteur, le compte **Robert Hatley** doit subir l’authentification MFA, car il appartient au groupe **finance** que vous avez configuré dans [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Vous pouvez configurer la stratégie d’authentification multifacteur via la console de gestion AD FS ou à l’aide de Windows PowerShell.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>Pour configurer la stratégie d’authentification multifacteur basée sur les données d’appartenance au groupe de l’utilisateur pour « ClaimApp » via la console de gestion AD FS

1.  Sur votre serveur de fédération, dans la console de gestion AD FS, accédez au nœud **Stratégies d’authentification**\\**Par approbation de partie de confiance** , puis sélectionnez l’approbation de partie de confiance qui représente votre exemple d’application (**claimapp**).

2.  Dans la page **Actions** ou en cliquant avec le bouton droit sur **claimapp**, sélectionnez **Modifier l’authentification multifacteur personnalisée**.

3.  Dans la fenêtre **Modifier l’approbation de partie de confiance pour claimapp** , cliquez sur le bouton **Ajouter** en regard de la liste **Utilisateurs/Groupes** . Tapez **finance** pour le nom de votre groupe Active Directory que vous avez créé dans [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md), cliquez sur **vérifier les noms**et, lorsque le nom est résolu, cliquez sur **OK**.

4.  Cliquez sur **OK** dans la fenêtre **Modifier l’approbation de partie de confiance pour claimapp** .

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>Pour configurer la stratégie d’authentification multifacteur basée sur les données d’appartenance au groupe de l’utilisateur pour « ClaimApp » via Windows PowerShell

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante :

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante :

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > Veillez à remplacer <group_SID> par la valeur du SID de votre groupe Active Directory **Finance**.

## <a name="BKMK_4"></a>Étape 4 : vérifier le mécanisme MFA
Au cours de cette étape, vous allez vérifier la fonctionnalité d’authentification multifacteur que vous avez configurée à l’étape précédente. Vous pouvez utiliser la procédure suivante pour vérifier que l’utilisateur Active Directory **Robert Hatley** peut accéder à votre exemple d’application et qu’il doit cette fois subir une authentification multifacteur car il appartient au groupe **Finance**.

1.  Sur votre ordinateur client, ouvrez une fenêtre de navigateur et accédez à votre exemple d’application : **https://webserv1.contoso.com/claimapp** .

    Cette action redirige automatiquement la demande vers le serveur de fédération et vous êtes invité à vous connecter avec un nom d’utilisateur et un mot de passe.

2.  Tapez les informations d'identification du compte Active Directory **Robert Hatley**.

    À ce stade, en raison de la stratégie MFA que vous avez configurée, l’utilisateur devra s’authentifier de nouveau. Le texte du message par défaut est **Pour des raisons de sécurité, nous avons besoin d’informations supplémentaires pour vérifier votre compte.** Toutefois, ce texte est entièrement personnalisable. Pour plus d’informations sur la façon de personnaliser l’expérience de connexion, voir [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

    Si vous avez configuré l’authentification par certificat comme méthode d’authentification supplémentaire, le texte du message par défaut est **Sélectionnez un certificat à utiliser pour l’authentification. Si vous annulez l’opération, fermez votre navigateur et réessayez.**

    Si vous avez configuré Windows Azure Multi-Factor Authentication comme méthode d’authentification supplémentaire, le texte du message par défaut est **Vous recevrez un appel sur votre téléphone pour finaliser votre authentification.** Pour plus d’informations sur la connexion avec Windows Azure Multi-Factor Authentication et l’utilisation de différentes options pour la méthode préférée de vérification, voir [Vue d’ensemble de Windows Azure Multi-Factor Authentication](https://technet.microsoft.com/library/dn249479.aspx).

## <a name="see-also"></a>Voir aussi
[Gérer les risques avec des multi-Factor Authentication supplémentaires pour les applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



