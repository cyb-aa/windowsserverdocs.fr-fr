---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: 'Guide pas à pas : gérer les risques avec des Access Control conditionnelles'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: aefcd597a580de526a758c6d026c6c91d02d10c8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407468"
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>Guide pas à pas : Gérer les risques avec le contrôle d’accès conditionnel




## <a name="about-this-guide"></a>À propos de ce guide
Cette procédure pas à pas fournit des instructions pour la gestion des risques avec l’un des facteurs (données utilisateur) disponibles via le mécanisme de contrôle d’accès conditionnel de Services ADFS (AD FS) dans Windows Server 2012 R2. Pour plus d’informations sur le contrôle d’accès conditionnel et les mécanismes d’autorisation dans AD FS dans Windows Server 2012 R2, consultez [gérer les risques avec des Access Control conditionnelles](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md).

Cette procédure pas à pas comporte les sections suivantes :

-   [Étape 1 : configuration de l’environnement Lab](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Étape 2 : vérifier le mécanisme de contrôle d’accès AD FS par défaut](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [Étape 3 : configurer la stratégie de contrôle d’accès conditionnel en fonction des données utilisateur](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [Étape 4 : vérifier le mécanisme de contrôle d’accès conditionnel](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>Étape 1 : configuration de l’environnement Lab
Afin de mener à bien cette procédure pas à pas, vous avez besoin d’un environnement constitué des composants suivants :

-   Un domaine Active Directory avec un compte d’utilisateur et de groupe de test, exécuté sur Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 avec son schéma mis à niveau vers Windows Server 2012 R2 ou un domaine Active Directory s’exécutant sur Windows Server 2012 R2

-   Un serveur de Fédération s’exécutant sur Windows Server 2012 R2

-   un serveur Web qui héberge votre exemple d’application ;

-   un ordinateur client à partir duquel vous pouvez accéder à l’exemple d’application.

> [!WARNING]
> Il est vivement recommandé (à la fois dans les environnements de production ou de test) de ne pas utiliser le même ordinateur comme serveur de fédération et serveur Web.

Dans cet environnement, le serveur de fédération émet les revendications requises afin que les utilisateurs puissent accéder à l’exemple d’application. Le serveur Web héberge un exemple d’application qui approuve les utilisateurs qui présentent les revendications émises par le serveur de fédération.

Pour obtenir des instructions sur la façon de configurer cet environnement, consultez [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Étape 2 : vérifier le mécanisme de contrôle d’accès AD FS par défaut
Au cours de cette étape, vous allez vérifier le mécanisme de contrôle d’accès AD FS par défaut, où l’utilisateur est redirigé vers la page de connexion AD FS, fournit des informations d’identification valides et est autorisé à accéder à l’application. Vous pouvez utiliser le compte **Robert Hatley** ad et l’exemple d’application **ClaimApp** que vous avez configurés dans Configuration de [l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>Pour vérifier le mécanisme de contrôle d’accès AD FS par défaut

1.  Sur votre ordinateur client, ouvrez une fenêtre de navigateur et accédez à votre exemple d’application : **https://webserv1.contoso.com/claimapp** .

    Cette action redirige automatiquement la demande vers le serveur de fédération et vous êtes invité à vous connecter avec un nom d’utilisateur et un mot de passe.

2.  Tapez les informations d’identification du compte **Robert Hatley** ad que vous avez créé dans [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Vous êtes autorisé à accéder à l’application.

## <a name="BKMK_3"></a>Étape 3 : configurer la stratégie de contrôle d’accès conditionnel en fonction des données utilisateur
Au cours de cette étape, vous allez configurer une stratégie de contrôle d’accès en fonction des données d’appartenance au groupe de l’utilisateur. Autrement dit, vous allez configurer une **règle d'autorisation d'émission** sur votre serveur de fédération pour une approbation de partie de confiance qui représente votre exemple d'application : **claimapp**. En raison de la logique de cette règle, l’utilisateur **Robert Hatley** ad recevra les revendications requises pour accéder à cette application, car il appartient à un groupe **finance** . Vous avez ajouté le compte **Robert Hatley** au groupe **finance** dans [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Vous pouvez effectuer cette tâche à l’aide de la console de gestion AD FS ou via Windows PowerShell.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>Pour configurer la stratégie de contrôle d’accès conditionnel en fonction des données utilisateur via la console de gestion AD FS

1.  Dans la console de gestion AD FS, accédez à **Relations d’approbation**, puis **Approbations de partie de confiance**.

2.  Sélectionnez l’approbation de partie de confiance qui représente votre exemple d’application (**claimapp**) et, dans le volet **Actions** ou en cliquant avec le bouton droit sur cette approbation de partie de confiance, sélectionnez **Modifier les règles de revendication**.

3.  Dans la fenêtre **Modifier les règles de revendication pour claimapp**, sélectionnez l’onglet **Règles d’autorisation d’émission** et cliquez sur **Ajouter une règle**.

4.  Dans l’**Assistant Ajout de règle de revendication d’autorisation d’émission**, dans la page **Sélectionner le modèle de règle**, sélectionnez le modèle de règle de revendication **Autoriser ou refuser l’accès des utilisateurs en fonction d’une revendication entrante** et cliquez sur **Suivant**.

5.  Dans la page **Configurer la règle**, effectuez toutes les opérations suivantes, puis cliquez sur **Terminer** :

    1.  Entrez un nom pour la règle de revendication, par exemple **TestRule**.

    2.  Sélectionnez **SID de groupe** comme **Type de revendication entrante**.

    3.  Cliquez sur **Parcourir**, tapez **Finance** pour le nom du groupe de test Active Directory et résolvez-le pour le champ **Valeur de revendication entrante**.

    4.  Sélectionnez l’option **Refuser l’accès aux utilisateurs avec cette revendication entrante**.

6.  Dans la fenêtre **Modifier les règles de revendication pour claimapp**, assurez-vous de supprimer la règle **Autoriser l’accès à tous les utilisateurs** qui a été créée par défaut lorsque vous avez créé cette approbation de partie de confiance.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>Pour configurer la stratégie de contrôle d’accès conditionnel en fonction des données utilisateur via Windows PowerShell

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante :


~~~
`$rp = Get-AdfsRelyingPartyTrust -Name claimapp`
~~~


2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante :


~~~
`$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`
~~~

> [!NOTE]
> Veillez à remplacer <group_SID> par la valeur du SID de votre groupe **Finance** Active Directory.

## <a name="BKMK_4"></a>Étape 4 : vérifier le mécanisme de contrôle d’accès conditionnel
Au cours de cette étape, vous allez vérifier la stratégie de contrôle d’accès conditionnel que vous avez configurée à l’étape précédente. Vous pouvez utiliser la procédure suivante pour vérifier que l’utilisateur Active Directory **Robert Hatley** peut accéder à votre exemple d’application car il appartient au groupe **Finance** et les utilisateurs Active Directory qui n’appartiennent pas au groupe **Finance** ne peuvent pas accéder à l’exemple d’application.

1.  Sur votre ordinateur client, ouvrez une fenêtre de navigateur et accédez à votre exemple d’application : **https://webserv1.contoso.com/claimapp**

    Cette action redirige automatiquement la demande vers le serveur de fédération et vous êtes invité à vous connecter avec un nom d’utilisateur et un mot de passe.

2.  Tapez les informations d’identification du compte **Robert Hatley** ad que vous avez créé dans [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Vous êtes autorisé à accéder à l’application.

3.  Tapez les informations d’identification d’un autre utilisateur Active Directory qui n’appartient PAS au groupe **Finance** . (Pour plus d’informations sur la création de comptes d’utilisateur dans Active Directory, consultez [https://technet.microsoft.com/library/cc7833232.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

    À ce stade, en raison de la stratégie de contrôle d’accès que vous avez configurée à l’étape précédente, un message « accès refusé » s’affiche pour cet utilisateur Active Directory qui n’appartient pas au groupe **finance** . Le texte du message par défaut est **vous n’êtes pas autorisé à accéder à ce site. Cliquez ici pour vous déconnecter et vous reconnecter ou contactez votre administrateur pour obtenir des autorisations.** Toutefois, ce texte est entièrement personnalisable. Pour plus d’informations sur la façon de personnaliser l’expérience de connexion, voir [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

## <a name="see-also"></a>Voir aussi
[Gérer les risques avec des Access Control conditionnelles](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



