---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: "Configurer des stratégies d’authentification"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7faffb7ccbb4b0ea3c65329d18f915d7dafcd46f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-authentication-policies"></a>Configurer des stratégies d’authentification

>S’applique à: Windows Server2012R2

Dans AD FS, dans Windows Server 2012 R2, à la fois le contrôle d’accès et le mécanisme d’authentification sont amélioré avec plusieurs facteurs qui incluent des données utilisateur, appareil, emplacement et d’authentification. Ces améliorations permettent de vous, par le biais de l’interface utilisateur ou par le biais de Windows PowerShell, permet de gérer les risques de l’octroi d’autorisations d’accès aux applications sécurisées par AD FS\ via le contrôle d’accès prennent-facteur et l’authentification prennent facteurs qui sont basées sur l’appartenance au groupe ou d’identité de l’utilisateur, l’emplacement réseau, données de l’appareil sont joint au workplace\, et l’état d’authentification lors de l’authentification prennent facteurs \(MFA\) effectuée.  
  
Pour plus d’informations sur l’authentification Multifacteur et les facteurs prennent le contrôle d’accès dans \(AD FS\) Active Directory Federation Services dans Windows Server 2012 R2, voir les rubriques suivantes:  


-   [Rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente à deux facteurs d’authentification entre les Applications d’entreprise](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Gérer les risques avec contrôle d’accès conditionnel](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurer des stratégies d’authentification via la gestion AD FS de composants  
L’appartenance au groupe **administrateurs**, ou équivalent, sur l’ordinateur local est le minimum requis pour réaliser ces procédures.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
Dans AD FS, dans Windows Server 2012 R2, vous pouvez spécifier une stratégie d’authentification à l’étendue globale qui s’applique à toutes les applications et services sécurisés par AD FS. Vous pouvez également définir des stratégies d’authentification pour des applications spécifiques et les services qui s’appuient sur les approbations et sont sécurisés par AD FS. Spécification d’une stratégie d’authentification pour une application particulière par partie de confiance approbation ne remplace pas la stratégie d’authentification globale. Si globale ou par partie de confiance approbation de stratégie d’authentification exige l’authentification Multifacteur, l’authentification Multifacteur est déclenchée lorsque l’utilisateur essaie de s’authentifier auprès de cette partie de confiance. La stratégie d’authentification globale est de secours pour les approbations de partie de confiance pour les applications et services qui n’ont pas d’une stratégie d’authentification configuré spécifique. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Pour configurer l’authentification principale globalement dans Windows Server 2012 R2 
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans AD FS logiciel enfichable, cliquez sur **stratégies d’authentification**.  
  
3.  Dans le **l’authentification principale** , cliquez sur **modifier** regard **paramètres globaux**. Vous pouvez également clic droit **stratégies d’authentification**, puis sélectionnez **modifier l’authentification principale globale**, ou, sous la **Actions** volet, sélectionnez **modifier l’authentification principale globale**.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy1.png)
  
4.  Dans le **modifier la stratégie d’authentification globale** fenêtre, dans le **principal** onglet, vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification globale:  
  
    -   Méthodes d’authentification à utiliser pour l’authentification principale. Vous pouvez sélectionner les méthodes d’authentification disponibles sous le **Extranet** et **Intranet**.  
  
    -   L’authentification des appareils via le **activer l’authentification des appareils** case à cocher. Pour plus d’informations, voir [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur d’authentification entre les Applications d’entreprise](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Pour configurer l’authentification principale par partie de confiance approbation  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans AD FS logiciel enfichable, cliquez sur **stratégies d’authentification**\\**par confiance**, puis cliquez sur la partie de confiance pour lequel vous souhaitez configurer des stratégies d’authentification.  
  
3.  Droit-cliquez sur l’approbation de la partie de confiance pour lequel vous souhaitez configurer des stratégies d’authentification, puis sélectionnez **modifier personnalisé l’authentification principale**, ou, sous la **Actions** volet, sélectionnez **modifier personnalisé l’authentification principale**.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  Dans le **modifier la stratégie d’authentification pour < relying\_party\_trust\_name >** fenêtre, sous le **principal** onglet, vous pouvez configurer le paramètre suivant dans le cadre de la **par confiance** stratégie d’authentification:  
  
    -   Indique si les utilisateurs sont requis pour fournir leurs informations d’identification chaque fois à la connexion via le **les utilisateurs sont requis pour fournir leurs informations d’identification chaque fois à la connexion** case à cocher.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Pour configurer l’authentification multifacteur global  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans AD FS logiciel enfichable, cliquez sur **stratégies d’authentification**.  
  
3.  Dans le **prennent-factor Authentication** , cliquez sur **modifier** regard **paramètres globaux**. Vous pouvez également clic droit **stratégies d’authentification**et sélectionnez **authentification prennent-modifier Global**, ou, sous la **Actions** volet, sélectionnez **authentification prennent-modifier Global**.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  Dans le **modifier la stratégie d’authentification globale** fenêtre, sous le **prennent facteurs** onglet, vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification globale prennent facteurs:  
  
    -   Les paramètres ou les conditions de l’authentification Multifacteur via les options disponibles sous le **utilisateurs\/groupes**, **périphériques**, et **emplacements** sections.  
  
    -   Pour activer l’authentification Multifacteur pour un de ces paramètres, vous devez sélectionner au moins une méthode d’authentification supplémentaire. **L’authentification des certificats** option disponible par défaut. Vous pouvez également configurer d’autres méthodes d’authentification supplémentaires personnalisé, par exemple, Windows Azure Active Authentication. Pour plus d’informations, voir [Guide pas à pas: gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
> [!WARNING]  
> Vous pouvez uniquement configurer des méthodes d’authentification supplémentaires globalement.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Pour configurer l’authentification de facteurs prennent par partie de confiance approbation  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Dans AD FS logiciel enfichable, cliquez sur **stratégies d’authentification**\\**par confiance**, puis cliquez sur la partie de confiance pour lequel vous souhaitez configurer l’authentification Multifacteur.  
  
3.  Droit-cliquez sur l’approbation de la partie de confiance pour lequel vous souhaitez configurer l’authentification Multifacteur, puis sélectionnez **personnalisé de modifier l’authentification prennent facteurs**, ou, sous la **Actions** volet, sélectionnez **personnalisé de modifier l’authentification prennent facteurs**.  
  
4.  Dans le **modifier la stratégie d’authentification pour < relying\_party\_trust\_name >** fenêtre, sous le **prennent facteurs** onglet, vous pouvez configurer les paramètres suivants dans le cadre de la confiance per\ approuver la stratégie d’authentification:  
  
    -   Les paramètres ou les conditions de l’authentification Multifacteur via les options disponibles sous le **utilisateurs\/groupes**, **périphériques**, et **emplacements** sections.  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurer des stratégies d’authentification via Windows PowerShell  
Windows PowerShell permet une plus grande flexibilité dans l’aide de différents facteurs de contrôle d’accès et le mécanisme d’authentification qui sont disponibles dans AD FS dans Windows Server 2012 R2 pour configurer les autorisations et les stratégies d’authentification des règles qui sont nécessaires pour implémenter l’accès conditionnel true pour vos ressources de \-secured AD FS.  
  
L’appartenance à des administrateurs, ou équivalent, sur l’ordinateur local est le minimum requis pour réaliser ces procédures.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Pour configurer la méthode d’authentification supplémentaire via Windows PowerShell  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  

    `Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `

  
> [!WARNING]  
> Pour vérifier que cette commande a été effectuée correctement, vous pouvez exécuter la `Get-AdfsGlobalAuthenticationPolicy` commande.  
  
### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Pour configurer l’authentification Multifacteur per\ de partie de confiance approbation basée sur les données d’appartenance au groupe d’un utilisateur  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante:  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
  
  
> [!WARNING]  
> Veillez à remplacer *< relying\_party\_trust >* avec le nom de votre approbation de partie de confiance.  
  
2.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  
  
 
    $MfaClaimRule =» c: [Type == '» https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'», valeur = ~ '» ^(?i) < group_SID >$ '»] = > problème (Type = '» https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'», valeur '» https://schemas.microsoft.com/claims/multipleauthn'»);» 
    
    Set-AdfsRelyingPartyTrust – TargetRelyingParty $rp – AdditionalAuthenticationRules $MfaClaimRule
  
  
> [!NOTE]  
> Veillez à remplacer < group\_SID > avec la valeur de l’identificateur de sécurité \(SID\) de votre groupe Active Directory \(AD\).  
  
### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Pour configurer l’authentification Multifacteur globalement basé sur les données d’appartenance au groupe des utilisateurs  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  

    $MfaClaimRule = "c: [Type == '» https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'», valeur == ' "sid_groupe '»]  
     = > problème (Type = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'», valeur = ' "https://schemas.microsoft.com/claims/multipleauthn'»);»  
      
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Veillez à remplacer *< group\_SID >* avec la valeur du SID de groupe Active Directory.  
  
### <a name="to-configure-mfa-globally-based-on-users-location"></a>Pour configurer l’authentification Multifacteur globalement basé sur l’emplacement de l’utilisateur  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  
 
    $MfaClaimRule = "c: [Type == ' "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'», valeur == '» true_or_false'»]  
     = > problème (Type = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'», valeur = ' "https://schemas.microsoft.com/claims/multipleauthn'»);»  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
  

  
> [!NOTE]  
> Veillez à remplacer *< true\_or\_false >* avec les deux `true` ou `false`. La valeur dépend de votre condition de règle spécifique qui est basée sur indique si la demande d’accès provient de l’extranet ou de l’intranet.  
  
### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Pour configurer l’authentification Multifacteur globalement basé sur les données de l’appareil de l’utilisateur  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  

    $MfaClaimRule = "c: [Type == ' "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'», valeur == «"true_or_false"»]  
     = > problème (Type = ' "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'», valeur = ' "https://schemas.microsoft.com/claims/multipleauthn'»);»  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> Veillez à remplacer *< true\_or\_false >* avec les deux `true` ou `false`. La valeur dépend de votre condition de règle spécifique qui est basée sur le périphérique soit workplace\ joint ou non.  
  
### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Pour configurer l’authentification Multifacteur globalement si la demande d’accès provient de l’extranet et à partir d’un appareil autre que celle workplace\ non joints  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  
 
    `Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
 
  
> [!NOTE]  
> Veillez à remplacer les deux instances de *< true\_or\_false >* avec les deux `true` ou `false`, selon vos conditions de règle spécifique. Les conditions de règle sont basées sur l’appareil soit workplace\ joint ou non et si la demande d’accès provient de l’extranet ou intranet.  
  
### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Pour configurer l’authentification Multifacteur globalement si accès provient d’un utilisateur qui appartient à un certain groupe de l’extranet  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  

    Set-AdfsAdditionalAuthenticationRule "c: [Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`», valeur == `"group_SID`»] et et c2: [Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`», valeur == `"true_or_false`»] = > problème (Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`», valeur ='» https://schemas.microsoft.com/claims/
      
> [!NOTE]  
> Veillez à remplacer *< group\_SID >* avec la valeur du SID de groupe et *< true\_or\_false >* avec les deux `true` ou `false`, qui dépend de votre condition de règle spécifique qui est basée sur indique si la demande d’accès provient de l’extranet ou intranet.  
  
### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Pour accorder l’accès à une application basée sur les données utilisateur via Windows PowerShell  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Veillez à remplacer *< relying\_party\_trust >* avec la valeur de votre approbation de partie de confiance.  
  
2.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  
  
    ```  
  
      $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
    ```  
  
> [!NOTE]  
> > Veillez à remplacer *< group\_SID >* avec la valeur du SID de groupe Active Directory.  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Pour accorder l’accès à une application sécurisée par uniquement si de AD FS identité de cet utilisateur a été validée avec l’authentification Multifacteur  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Veillez à remplacer *< relying\_party\_trust >* avec la valeur de votre approbation de partie de confiance.  
  
2.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  
  
    ```  
    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessWithMFA`"  
    c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  
  
    ```  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Pour accorder l’accès à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un appareil appartenant à un workplace\ qui est enregistré à l’utilisateur  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> Veillez à remplacer *< relying\_party\_trust >* avec la valeur de votre approbation de partie de confiance.  
  
2.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  
  

    $GroupAuthzRule = «@RuleTemplate = `"Authorization`»  
    @RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
    c: [Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`», valeur = ~ `"^(?i)true$`»] = > problème (Type = `"https://schemas.microsoft.com/authorization/claims/permit`», valeur = `"PermitUsersWithClaim`»);  
  

  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Pour accorder l’accès à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un appareil appartenant à un workplace\ qui est enregistré à un utilisateur dont l’identité a été validée avec l’authentification Multifacteur  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> Veillez à remplacer *< relying\_party\_trust >* avec la valeur de votre approbation de partie de confiance.  
  
2.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  
  
    ```  
    $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
    @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
  
    ```  
  
### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Pour accorder l’accès extranet à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un utilisateur dont l’identité a été validée avec l’authentification Multifacteur  
  
1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  

  
> [!NOTE]  
> Veillez à remplacer *< relying\_party\_trust >* avec la valeur de votre approbation de partie de confiance.  
  
2.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  
  

    $GroupAuthzRule = «@RuleTemplate = `"Authorization`»  
    @RuleName = `"RequireMFAForExtranetAccess`"  
    C1: [Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`», valeur = ~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`»] & &  
    C2: [Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`», valeur = ~ `"^(?i)false$`»] = > problème (Type = `"https://schemas.microsoft.com/authorization/claims/permit`», valeur = `"PermitUsersWithClaim`»);»  

## <a name="additional-references"></a>Références supplémentaires  

[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md)
