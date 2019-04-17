---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: "Guide de procédure pas à pas: gérer les risques avec contrôle d’accès conditionnel"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11d2d567f9264dca53a3426263a172649d7d7c11
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>Guide pas à pas: Gérer les risques avec contrôle d’accès conditionnel

>S’applique à: Windows Server2012R2


## <a name="about-this-guide"></a>À propos de ce Guide
Cette procédure pas à pas fournit des instructions pour la gestion des risques avec un des facteurs (données utilisateur) disponibles via le mécanisme de contrôle d’accès conditionnel dans Active Directory Federation Services (ADFS) dans Windows Server 2012 R2. Pour plus d’informations sur les mécanismes de contrôle et d’autorisation d’accès conditionnel dans AD FS dans Windows Server 2012 R2, voir [gérer les risques avec le contrôle d’accès conditionnel](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md).

Cette procédure comprend les sections suivantes:

-   [Étape 1: Configuration de l’environnement de laboratoire](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Étape 2: Vérifier le mécanisme de contrôle d’accès AD FS par défaut](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [Étape 3: Configurer la stratégie de contrôle d’accès conditionnel basée sur les données utilisateur](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [Étape 4: Vérifier le mécanisme de contrôle d’accès conditionnel](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>Étape 1: Configuration de l’environnement de laboratoire
Afin d’effectuer cette procédure pas à pas, vous avez besoin d’un environnement constitué des composants suivants:

-   Un domaine Active Directory avec des comptes de groupe en cours d’exécution sur Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 avec son schéma mis à niveau vers Windows Server 2012 R2 ou un domaine Active Directory s’exécutant sur Windows Server 2012 R2 et un utilisateur de test

-   Un serveur de fédération exécuté sous Windows Server 2012 R2

-   Un serveur web qui héberge votre exemple d’application

-   Un ordinateur client à partir de laquelle vous pouvez accéder à l’exemple d’application

> [!WARNING]
> Il est vivement recommandé (à la fois dans les environnements de production ou de test) que vous n’utilisez pas le même ordinateur comme serveur de fédération et votre serveur web.

Dans cet environnement, le serveur de fédération émet les revendications qui sont requises afin que les utilisateurs peuvent accéder à l’exemple d’application. Le serveur Web héberge un exemple d’application qui approuve les utilisateurs qui présentent les revendications qui les problèmes de serveur de fédération.

Pour obtenir des instructions sur la façon de configurer cet environnement, consultez [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Étape 2: Vérifier le mécanisme de contrôle d’accès AD FS par défaut
Dans cette étape, vous allez vérifier le mécanisme de contrôle d’accès AD FS, où l’utilisateur est redirigé vers la page de connexion AD FS, par défaut fournit des informations d’identification valides et est autorisé à accéder à l’application. Vous pouvez utiliser le **Robert Hatley** compte Active Directory et la **claimapp** exemple d’application que vous avez configuré dans [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>Pour vérifier la valeur par défaut le mécanisme de contrôle d’accès AD FS

1.  Sur votre ordinateur client, ouvrez une fenêtre de navigateur et accédez à votre exemple d’application: **https://webserv1.contoso.com/claimapp**.

    Cette action redirige automatiquement la demande vers le serveur de fédération et vous êtes invité à vous connecter avec un nom d’utilisateur et un mot de passe.

2.  Tapez les informations d’identification de le **Robert Hatley** compte Active Directory que vous avez créé dans [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Vous aura accès à l’application.

## <a name="BKMK_3"></a>Étape 3: Configurer la stratégie de contrôle d’accès conditionnel basée sur les données utilisateur
Dans cette étape, vous allez configurer une stratégie de contrôle d’accès basée sur les données d’appartenance au groupe utilisateur. En d’autres termes, vous allez configurer une **une règle d’autorisation d’émission** sur votre serveur de fédération pour une approbation de partie de confiance qui représente votre exemple d’application - **claimapp**. Selon la logique de cette règle, **Robert Hatley** utilisateur Active Directory reçoit des revendications qui sont nécessaires pour accéder à cette application car il appartient à un **Finance** groupe. Vous avez ajouté le **Robert Hatley** compte pour le **Finance** groupe dans [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Vous pouvez effectuer cette tâche à l’aide d’une Console de gestion AD FS ou via Windows PowerShell.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>Pour configurer la stratégie de contrôle d’accès conditionnel basée sur les données utilisateur via la Console de gestion AD FS

1.  Dans la Console de gestion AD FS, accédez à **relations d’approbation**, puis **approbations de partie de confiance**.

2.  Sélectionnez la partie de confiance qui représente votre exemple d’application (**claimapp**) et, dans le **Actions** volet ou en double-cliquant sur cette partie de confiance, sélectionnez **modifier les règles de revendication**.

3.  Dans le **modifier les règles de revendication pour claimapp** fenêtre, sélectionnez **les règles d’autorisation d’émission** onglet et cliquez sur **ajouter une règle**.

4.  Dans le **Assistant Ajout de d’émission d’autorisation de revendications règle**, dans le **page Sélectionner un modèle de règle**, sélectionnez **autoriser ou refuser les utilisateurs selon une revendication entrante** modèle de règle de revendication, puis cliquez sur **suivant**.

5.  Sur le **configurer la règle** page, effectuez toutes les opérations suivantes, puis sur **Terminer**:

    1.  Entrez un nom pour la règle de revendication, par exemple **TestRule**.

    2.  Sélectionnez **SID de groupe** comme **type de revendication entrante**.

    3.  Cliquez sur **Parcourir**, tapez **Finance** groupe de test pour le nom de votre annonce et résolvez-le pour le **valeur de revendication entrante** champ.

    4.  Sélectionnez le **refuser l’accès aux utilisateurs avec cette revendication entrante** option.

6.  Dans le **modifier les règles de revendication pour claimapp** fenêtre, assurez-vous de supprimer le **autoriser l’accès à tous les utilisateurs** règle qui a été créé par défaut lorsque vous avez créé cette approbation de partie de confiance.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>Pour configurer la stratégie de contrôle d’accès conditionnel basée sur les données utilisateur via Windows PowerShell

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante:


    `$rp = Get-AdfsRelyingPartyTrust -Name claimapp`


2.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante:


    `$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`

> [!NOTE]
> Veillez à remplacer < group_SID > par la valeur du SID de votre annonce **Finance** groupe.

## <a name="BKMK_4"></a>Étape 4: Vérifier le mécanisme de contrôle d’accès conditionnel
Dans cette étape, vous allez vérifier la stratégie de contrôle d’accès conditionnel que vous avez configuré à l’étape précédente. Vous pouvez utiliser la procédure suivante pour vérifier que **Robert Hatley** utilisateur Active Directory permettre accéder à votre exemple d’application car il appartient à la **Finance** groupe et des utilisateurs Active Directory qui n’appartiennent pas à la **Finance** groupe ne peut pas accéder à l’exemple d’application.

1.  Sur votre ordinateur client, ouvrez une fenêtre de navigateur et accédez à votre exemple d’application: **https://webserv1.contoso.com/claimapp**

    Cette action redirige automatiquement la demande vers le serveur de fédération et vous êtes invité à vous connecter avec un nom d’utilisateur et un mot de passe.

2.  Tapez les informations d’identification de le **Robert Hatley** compte Active Directory que vous avez créé dans [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Vous aura accès à l’application.

3.  Tapez les informations d’identification d’un autre utilisateur Active Directory qui n’appartient pas à la **Finance** groupe. (Pour plus d’informations sur la création de comptes d’utilisateur dans ActiveDirectory, voir [https://technet.microsoft.com/library/cc7833232.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

    À ce stade, en raison de la stratégie de contrôle d’accès que vous avez configuré à l’étape précédente, un message «accès refusé» s’affiche pour l’utilisateur Active Directory qui n’appartient pas à la **Finance** groupe. Le texte du message par défaut est **vous n’êtes pas autorisé à accéder à ce site. Cliquez ici pour vous déconnecter et vous reconnecter ou contactez votre administrateur pour les autorisations.** Toutefois, ce texte est entièrement personnalisable. Pour plus d’informations sur la façon de personnaliser l’expérience de connexion, voir [personnalisation des Pages AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx).

## <a name="see-also"></a>Voir aussi
[Gérer les risques avec le contrôle d’accès conditionnel](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



