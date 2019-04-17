---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: "Guide de procédure pas à pas: gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 414f37e86f0072863e5fa2f107c39e5518e560ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Guide de procédure pas à pas: Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles

>S’applique à: Windows Server2012R2


## <a name="about-this-guide"></a>À propos de ce Guide
Cette procédure pas à pas fournit des instructions pour la configuration de l’authentification multifacteur (MFA) dans Active Directory Federation Services (ADFS) dans Windows Server 2012 R2 basées sur les données d’appartenance au groupe de l’utilisateur.

Pour plus d’informations sur les mécanismes d’authentification Multifacteur et l’authentification dans AD FS, voir [gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

Cette procédure comprend les sections suivantes:

-   [Étape 1: Configuration de l’environnement de laboratoire](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Étape 2: Vérifier le mécanisme d’authentification AD FS par défaut](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [Étape 3: Configurer l’authentification Multifacteur sur votre serveur de fédération](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [Étape 4: Vérifier le mécanisme d’authentification Multifacteur](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>Étape 1: Configuration de l’environnement de laboratoire
Afin d’effectuer cette procédure pas à pas, vous avez besoin d’un environnement constitué des composants suivants:

-   Un domaine Active Directory avec des comptes de groupe s’exécutant sur Windows Server 2012 R2 ou un domaine Active Directory exécuté sous Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 avec son schéma mis à niveau vers Windows Server 2012 R2 et un utilisateur de test

-   Un serveur de fédération exécuté sous Windows Server 2012 R2

-   Un serveur web qui héberge votre exemple d’application

-   Un ordinateur client à partir de laquelle vous pouvez accéder à l’exemple d’application

> [!WARNING]
> Il est vivement recommandé (à la fois dans un environnement de production et de test) que vous n’utilisez pas le même ordinateur comme serveur de fédération et votre serveur web.

Dans cet environnement, le serveur de fédération émet les revendications qui sont requises afin que les utilisateurs peuvent accéder à l’exemple d’application. Le serveur Web héberge un exemple d’application qui approuve les utilisateurs qui présentent les revendications qui les problèmes de serveur de fédération.

Pour obtenir des instructions sur la façon de configurer cet environnement, consultez [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Étape 2: Vérifier le mécanisme d’authentification AD FS par défaut
Dans cette étape, vous allez vérifier le mécanisme de contrôle d’accès AD FS par défaut (**l’authentification par formulaire** pour extranet et **l’authentification Windows** pour l’intranet), où l’utilisateur est redirigé vers la page de connexion AD FS, fournit des informations d’identification valides et a accès à l’application. Vous pouvez utiliser le **Robert Hatley** compte Active Directory et la **claimapp** exemple d’application que vous avez configuré dans [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

1.  Sur votre ordinateur client, ouvrez une fenêtre de navigateur et accédez à votre exemple d’application: **https://webserv1.contoso.com/claimapp**.

    Cette action redirige automatiquement la demande vers le serveur de fédération et vous êtes invité à vous connecter avec un nom d’utilisateur et un mot de passe.

2.  Tapez les informations d’identification de le **Robert Hatley** compte Active Directory que vous avez créé dans [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Vous aura accès à l’application.

## <a name="BKMK_3"></a>Étape 3: Configurer l’authentification Multifacteur sur votre serveur de fédération
Il existe deux parties à la configuration de l’authentification Multifacteur dans AD FS dans Windows Server 2012 R2:

-   [Sélectionnez une méthode d’authentification supplémentaires](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [Configurer la stratégie d’authentification Multifacteur](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>Sélectionnez une méthode d’authentification supplémentaires
Pour configurer l’authentification Multifacteur, vous devez sélectionner une méthode d’authentification supplémentaire. Dans cette procédure pas à pas pour la méthode d’authentification supplémentaire, vous pouvez choisir entre les options suivantes:

-   Sélectionnez [l’authentification par certificat](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7) méthode qui est disponible dans AD FS dans Windows Server 2012 R2 par défaut

-   Configurez et sélectionnez [Windows Azure multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>Authentification par certificat
Effectuez une des procédures suivantes pour sélectionner l’authentification par certificat comme méthode d’authentification supplémentaire:

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>Pour configurer l’authentification par certificat comme méthode d’authentification supplémentaire via la Console de gestion AD FS

1.  Sur votre serveur de fédération, dans la Console de gestion AD FS, accédez à la **stratégies d’authentification** nœud et, sous **multi-Factor Authentication** , cliquez sur le **modifier** lien en regard du **paramètres globaux** sous-section.

2.  Dans le **modifier la stratégie d’authentification globale** fenêtre, sélectionnez **l’authentification par certificat** comme une méthode d’authentification supplémentaire, puis cliquez sur **OK**.

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>Pour configurer l’authentification par certificat comme méthode d’authentification supplémentaire via Windows PowerShell

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante:

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > Pour vérifier que cette commande a été effectuée correctement, vous pouvez exécuter la `Get-AdfsGlobalAuthenticationPolicy` commande.

#### <a name="BKMK_8"></a>Windows Azure multi-Factor Authentication
Effectuez les procédures suivantes afin de télécharger et de configurer et de sélectionner **Windows Azure multi-Factor Authentication** comme authentification supplémentaire sur votre serveur de fédération:

1.  [Créer un fournisseur multi-Factor Authentication via le Windows portail Azure](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [Télécharger le serveur Windows Azure multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [Installer le serveur Windows Azure multi-Factor Authentication sur votre serveur de fédération](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [Configurer Windows Azure multi-Factor Authentication comme méthode d’authentification supplémentaires](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>Créer un fournisseur multi-Factor Authentication via le Windows portail Azure

1.  Ouvrez une session sur Windows Azure portail en tant qu’administrateur.

2.  Sur la gauche, sélectionnez Active Directory.

3.  Sur la page Active Directory, dans la partie supérieure, sélectionnez **fournisseurs d’authentification multifacteur**.  En bas, cliquez sur **New**.

4.  Sous **Services d’application -> Active Directory**, sélectionnez **fournisseur d’authentification multifacteur**, puis sélectionnez **création rapide**.

5.  Sous **Services d’application**, sélectionnez **fournisseurs d’authentification Active**, puis sélectionnez **création rapide**.

6.  Renseignez les champs et sélectionnez suivants **créer**.

    1.  **Nom** -le nom du fournisseur multi-Factor Authentication.

    2.  **Modèle d’utilisation** -modèle d’utilisation du fournisseur d’authentification multifacteur.

        -   **Par authentification** -modèle d’achat qui facture par authentification. Généralement utilisé pour les scénarios qui utilisent Windows Azure multi-Factor Authentication dans une application réservée aux consommateurs.

        -   **Par utilisateur activé** -modèle d’achat qui facture par utilisateur activé.  Généralement utilisé pour les scénarios liés aux employés Office 365.

        Pour plus d’informations sur les modèles d’utilisation, voir [tarification Windows Azure](http://www.windowsazure.com/pricing/details/active-directory/).

    3.  **Répertoire** -locataire Windows Azure Active Directory que le fournisseur d’authentification multifacteur est associé. Cette opération est facultative car le fournisseur ne doit pas être liés à Windows Azure Active Directory lors de la sécurisation des applications sur site.

7.  Une fois que vous cliquez sur Créer, le fournisseur d’authentification multifacteur sera créé et vous devriez voir un message indiquant: créé avec succès le fournisseur d’authentification multifacteur.  Cliquez sur **Ok**.

Ensuite, vous devez télécharger le serveur Windows Azure multi-Factor Authentication. Ce faire, vous pouvez lancer le portail Windows Azure multi-Factor Authentication via le portail Windows Azure.

##### <a name="BKMK_b"></a>Télécharger le serveur Windows Azure multi-Factor Authentication

1.  Ouvrez une session sur le portail Windows Azure en tant qu’administrateur et cliquez sur le fournisseur d’authentification multifacteur vous avez créé dans la procédure ci-dessus. Puis cliquez sur le **gérer** bouton.

    Cette opération lance le **Windows Azure multi-Factor Authentication** portal.

2.  Dans le **Windows Azure multi-Factor Authentication** portail, cliquez sur **télécharge**, puis cliquez sur **télécharger** pour télécharger une copie du serveur Windows Azure multi-Factor Authentication.

Une fois que vous avez téléchargé le fichier exécutable pour le serveur Windows Azure multi-Factor Authentication, vous devez l’installer sur votre serveur de fédération.

##### <a name="BKMK_c"></a>Installer le serveur Windows Azure multi-Factor Authentication sur votre serveur de fédération

1.  Téléchargez et double-cliquez sur le fichier exécutable pour le serveur Windows Azure multi-Factor Authentication.  L’installation commence alors.

2.  Sur l’écran du contrat de licence, lisez le contrat, sélectionnez **J’accepte** et cliquez sur **suivant**.

3.  Assurez-vous que le dossier de destination est correct, puis cliquez sur **suivant**.

4.  Une fois l’installation terminée, cliquez sur **Terminer**.

Vous êtes maintenant prêt à lancer le serveur Windows Azure multi-Factor Authentication que vous avez installé sur votre serveur de fédération et le configurer comme méthode d’authentification supplémentaire.

##### <a name="BKMK_d"></a>Configurer Windows Azure multi-Factor Authentication comme méthode d’authentification supplémentaires

1.  Lancer **Windows Azure multi-Factor Authentication** où vous installé sur votre serveur de fédération et sur la page d’accueil, vérifiez le **ignorer à l’aide de l’Assistant Configuration de l’authentification** case à cocher et cliquez sur **suivant**.

2.  Pour activer le serveur d’authentification multifacteur, revenez à la page dans le portail de gestion multi-Factor Authentication où vous avez téléchargé le serveur multi-Factor et cliquez sur le **générer les informations d’identification de l’Activation** bouton. Dans l’interface utilisateur du serveur multi-Factor Authentication, entrez les informations d’identification qui ont été générées et cliquez sur **Activate**.

3.  Ensuite, le **serveur multi-Factor Authentication** interface utilisateur vous invite à exécuter le **Assistant Configuration du multi-serveur**.  Sélectionnez **non**.

    > [!IMPORTANT]
    > Vous pouvez ignorer l’exécution du **Assistant Configuration du multi-serveur** donné l’environnement lab avec uniquement un serveur de fédération qui est utilisé pour effectuer cette procédure pas à pas. Toutefois, si votre environnement contient plusieurs serveurs de fédération, vous devez installer le serveur d’authentification multifacteur et terminer le **Assistant Configuration du multi-serveur** sur chaque serveur de fédération afin d’activer la réplication entre les serveurs multi-Factor en cours d’exécution sur vos serveurs de fédération.

4.  Dans le **serveur multi-Factor Authentication** interface utilisateur, sélectionnez le **utilisateurs** icône, cliquez sur **importer à partir d’Active Directory**, sélectionnez le **Robert Hatley** compte à configurer dans Windows Azure multi-Factor Authentication, puis cliquez sur **Import**.

5.  Dans le **utilisateurs** liste, sélectionnez le **Robert Hatley** de compte, cliquez sur **modifier**et dans le **modifier un utilisateur** fenêtre, fournir un numéro de téléphone portable de ce compte, assurez-vous que le **activé** case à cocher est activée, puis cliquez sur **appliquer**.

6.  Dans le **utilisateurs** liste, sélectionnez le **Robert Hatley** de compte, puis cliquez sur **Test**. Dans le **utilisateur de Test** fenêtre, fournir les informations d’identification pour le **Robert Hatley** compte. Lorsque le téléphone portable sonne, appuyez sur «#» pour terminer la vérification du compte.

7.  Dans le **serveur multi-Factor** interface utilisateur, sélectionnez le **AD FS** icône, vous assurer que **autoriser l’inscription utilisateur**, **permettent aux utilisateurs de sélectionner la méthode** (y compris **appel téléphonique** et **message texte**), **utiliser les questions de sécurité de secours** et **activer la journalisation** cases sont cochées, cliquez sur **adaptateur installation AD FS**et terminer le **multi-Factor Authentication AD FS carte** Assistant installation.

    > [!NOTE]
    > Le **multi-Factor Authentication AD FS carte** Assistant installation crée un groupe de sécurité appelé **PhoneFactor Admins** dans Active Directory, puis ajoute le compte de service AD FS de votre service de fédération à ce groupe.
    > 
    > Il est recommandé de vérifier sur votre contrôleur de domaine qui les **PhoneFactor Admins** groupe est créé en effet et que le compte de service les services AD FS est un membre de ce groupe.
    > 
    > Si nécessaire, ajoutez le compte de service AD FS pour le **PhoneFactor Admins** groupe manuellement sur votre contrôleur de domaine.

    Pour plus d’informations sur l’installation de l’adaptateur AD FS, cliquez sur le lien d’aide dans le coin supérieur droit du serveur multi-Factor Authentication.

8.  Pour inscrire l’adaptateur dans votre service de fédération, sur votre serveur de fédération, lancez la fenêtre de commande Windows PowerShell et exécutez la commande suivante: `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`. L’adaptateur est maintenant inscrit en tant que **WindowsAzureMultiFactorAuthentication**.  Vous devez redémarrer votre service AD FS pour l’inscription entre en vigueur.

9. Pour configurer Windows Azure multi-Factor Authentication comme méthode d’authentification supplémentaire, dans la Console de gestion AD FS, accédez à la **stratégies d’authentification** nœud et, sous **multi-Factor Authentication** , cliquez sur le **modifier** lien en regard du **paramètres globaux** sous-section. Dans le **modifier la stratégie d’authentification globale** fenêtre, sélectionnez **multi-Factor Authentication** comme une méthode d’authentification supplémentaire, puis cliquez sur **OK**.

    > [!NOTE]
    > Vous pouvez personnaliser le nom et la description de la méthode Windows Azure multi-Factor Authentication, ainsi que toute configurée méthode d’authentification tiers, tel qu’il apparaît dans votre interface utilisateur AD FS, en exécutant la **Set-AdfsAuthenticationProviderWebContent** applet de commande. Pour plus d’informations, voir [https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>Configurer la stratégie d’authentification Multifacteur
Pour activer l’authentification Multifacteur, vous devez configurer la stratégie d’authentification Multifacteur sur votre serveur de fédération. Pour cette procédure pas à pas, selon notre stratégie d’authentification Multifacteur, **Robert Hatley** compte est requis pour subir une authentification Multifacteur car il appartient à la **Finance** groupe que vous avez configuré dans [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Vous pouvez configurer la stratégie d’authentification Multifacteur via la Console de gestion AD FS ou à l’aide de Windows PowerShell.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>Pour configurer la stratégie d’authentification Multifacteur basée sur les données d’appartenance au groupe de l’utilisateur pour 'claimapp' via la Console de gestion AD FS

1.  Sur votre serveur de fédération, dans la Console de gestion AD FS, accédez à **stratégies d’authentification**\\**par confiance** nœud, puis sélectionnez la partie de confiance qui représente votre exemple d’application (**claimapp**).

2.  Dans le **Actions** page ou en cliquant **claimapp**, sélectionnez **modifier personnalisé multi-Factor Authentication**.

3.  Dans le **modifier de confiance pour claimapp** fenêtre, cliquez sur le **ajouter** situé en regard du **utilisateurs/groupes** liste. Tapez dans **Finance** pour le nom de votre groupe Active Directory que vous avez créé dans [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md), puis cliquez sur **vérifier les noms**, lorsque le nom est résolu, cliquez sur **OK**.

4.  Cliquez sur **OK** dans les **modifier de confiance pour claimapp** fenêtre.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>Pour configurer la stratégie d’authentification Multifacteur basée sur les données d’appartenance au groupe de l’utilisateur pour 'claimapp' via Windows PowerShell

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante:

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante:

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > Veillez à remplacer < group_SID > par la valeur du SID de groupe Active Directory **Finance**.

## <a name="BKMK_4"></a>Étape 4: Vérifier le mécanisme d’authentification Multifacteur
Dans cette étape, vous allez vérifier la fonctionnalité d’authentification Multifacteur que vous avez configuré à l’étape précédente. Vous pouvez utiliser la procédure suivante pour vérifier que **Robert Hatley** utilisateur Active Directory permettre accéder à votre exemple d’application et cette fois subir une authentification Multifacteur car il appartient à la **Finance** groupe.

1.  Sur votre ordinateur client, ouvrez une fenêtre de navigateur et accédez à votre exemple d’application: **https://webserv1.contoso.com/claimapp**.

    Cette action redirige automatiquement la demande vers le serveur de fédération et vous êtes invité à vous connecter avec un nom d’utilisateur et un mot de passe.

2.  Tapez les informations d’identification de le **Robert Hatley** compte Active Directory.

    À ce stade, en raison de la stratégie d’authentification Multifacteur que vous avez configuré, l’utilisateur sera invité à subir une authentification supplémentaire. Le texte du message par défaut est **pour des raisons de sécurité, nous avons besoin des informations supplémentaires pour vérifier votre compte.** Toutefois, ce texte est entièrement personnalisable. Pour plus d’informations sur la façon de personnaliser l’expérience de connexion, voir [personnalisation des Pages AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx).

    Si vous avez configuré l’authentification par certificat comme méthode d’authentification supplémentaire, le texte du message par défaut est **sélectionner un certificat que vous souhaitez utiliser pour l’authentification. Si vous annulez l’opération, fermez votre navigateur et réessayez.**

    Si vous avez configuré Windows Azure multi-Factor Authentication comme méthode d’authentification supplémentaire, le texte du message par défaut est **un appel sera placé sur votre téléphone pour finaliser votre authentification.** Pour plus d’informations sur la connexion avec Windows Azure multi-Factor Authentication et à l’aide de différentes options pour la méthode préférée de vérification, voir [vue d’ensemble de Windows Azure multi-Factor Authentication](https://technet.microsoft.com/library/dn249479.aspx).

## <a name="see-also"></a>Voir aussi
[Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



