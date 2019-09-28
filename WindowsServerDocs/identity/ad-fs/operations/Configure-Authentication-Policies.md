---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: Configurer des stratégies d’authentification
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ef38b0280a5753b0995e85d0809de6b632fa3afc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358118"
---
# <a name="configure-authentication-policies"></a>Configurer des stratégies d’authentification

Dans AD FS, dans Windows Server 2012 R2, le contrôle d’accès et le mécanisme d’authentification sont améliorés avec plusieurs facteurs, notamment les données d’utilisateur, de périphérique, d’emplacement et d’authentification. Ces améliorations vous permettent, par le biais de l’interface utilisateur ou de Windows PowerShell, de gérer le risque d’accorder des autorisations d'\-accès à AD FS applications\-sécurisées par le biais du contrôle d’accès multifacteur et de plusieurs\-authentification par facteur basée sur l’identité de l’utilisateur ou l’appartenance au groupe, l’emplacement réseau,\-les données de l’appareil qui sont jointes à \(l’espace de travail et l'\-état d’authentification lors de l’authentification multifacteur MFA\) a été effectué.  

Pour plus d’informations sur l’authentification\-MFA et le contrôle d' \(accès\) multifacteur dans services ADFS AD FS dans Windows Server 2012 R2, consultez les rubriques suivantes :  


-   [Joindre un espace de travail à partir de n’importe quel appareil en utilisant l’authentification unique et l’authentification de second facteur transparente](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Gérer les risques avec le contrôle d’accès conditionnel](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurer des stratégies d’authentification via le composant\-logiciel enfichable de gestion AD FS  
Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent sur l’ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   

Dans AD FS, dans Windows Server 2012 R2, vous pouvez spécifier une stratégie d’authentification au niveau d’une étendue globale applicable à l’ensemble des applications et services sécurisés par AD FS. Vous pouvez également définir des stratégies d’authentification pour des applications et des services spécifiques qui reposent sur des approbations de tiers et qui sont sécurisés par AD FS. La spécification d’une stratégie d’authentification pour une application particulière par approbation de partie de confiance ne remplace pas la stratégie d’authentification globale. Si la stratégie d’authentification globale ou par approbation de partie de confiance requiert l’authentification MFA, l’authentification MFA est déclenchée lorsque l’utilisateur tente de s’authentifier auprès de cette approbation de partie de confiance. La stratégie d’authentification globale est une solution de secours pour les approbations de partie de confiance pour les applications et les services qui n’ont pas de stratégie d’authentification configurée spécifique. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Pour configurer globalement l’authentification principale dans Windows Server 2012 R2 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans AD FS composant\-logiciel enfichable, cliquez sur **stratégies d’authentification**.  

3.  Dans la section **authentification principale** , cliquez sur **modifier** en regard de **paramètres globaux**. Vous pouvez également cliquer\-avec le bouton droit sur **stratégies d’authentification**, sélectionner **modifier l’authentification principale globale**, ou, dans le volet **actions** , sélectionner **modifier l’authentification principale globale**.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy1.png)

4.  Dans la fenêtre **modifier la stratégie d’authentification globale** , sous l’onglet **principal** , vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification globale :  

    -   Méthodes d’authentification à utiliser pour l’authentification principale. Vous pouvez sélectionner les méthodes d’authentification disponibles sous **extranet** et **Intranet**.  

    -   Authentification de l’appareil via la case à cocher **activer l’authentification** de l’appareil. Pour plus d’informations, consultez [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Pour configurer l’authentification principale par approbation de partie de confiance  

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans AD FS composant\-logiciel enfichable, cliquez sur\\ **stratégies d’authentification** **par approbation de partie de confiance**, puis cliquez sur l’approbation de la partie de confiance pour laquelle vous souhaitez configurer des stratégies d’authentification.  

3.  Cliquez avec\-le bouton droit sur l’approbation de la partie de confiance pour laquelle vous souhaitez configurer des stratégies d’authentification, puis sélectionnez **modifier l’authentification principale personnalisée**. ou bien, dans le volet **actions** , sélectionnez Modifier le serveur **principal personnalisé. Authentification**.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  Dans la fenêtre **modifier la stratégie d’authentification pour\_<\_nom\_de l’approbation de partie de confiance >** , sous l’onglet **principal** , vous pouvez configurer le paramètre suivant dans le cadre de l' **approbation par partie de confiance** . stratégie d’authentification :  

    -   Si les utilisateurs doivent fournir leurs informations d’identification chaque fois qu’ils\-se connectent par le biais des utilisateurs, ils doivent **fournir leurs informations d'\-identification chaque fois** que la case se connecter.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Pour configurer l’authentification multifacteur globalement  

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans AD FS composant\-logiciel enfichable, cliquez sur **stratégies d’authentification**.  

3.  Dans la **\-section authentification multifacteur** , cliquez sur **modifier** en regard de **paramètres globaux**. Vous pouvez également cliquer\-avec le bouton droit sur **stratégies d’authentification**, **\-sélectionner modifier l’authentification multifacteur globale**, ou, **\-dans le volet Actions, sélectionner modifier l’authentification multifacteur globale.** .  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  Dans la fenêtre **modifier la stratégie d’authentification globale** , sous l’onglet **\-plusieurs facteurs** , vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification\-multifacteur globale :  

    -   Paramètres ou conditions pour l’authentification MFA via les options disponibles dans les sections **groupes d'\/utilisateurs**, **appareils**et **emplacements** .  

    -   Pour activer MFA pour l’un de ces paramètres, vous devez sélectionner au moins une méthode d’authentification supplémentaire. L' **authentification par certificat** est l’option par défaut disponible. Vous pouvez également configurer d’autres méthodes d’authentification supplémentaires personnalisées, par exemple, Windows Azure active Authentication. Pour plus d’informations, [consultez Guide pas à pas : Gérer les risques avec des Multi-Factor Authentication supplémentaires pour](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)les applications sensibles.  

> [!WARNING]  
> Vous pouvez uniquement configurer des méthodes d’authentification supplémentaires de manière globale.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Pour configurer\-l’authentification multifacteur par approbation de partie de confiance  

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans AD FS composant\-logiciel enfichable, cliquez sur\\ **stratégies d’authentification** **par approbation de partie de confiance**, puis cliquez sur l’approbation de la partie de confiance pour laquelle vous souhaitez configurer l’authentification multifacteur.  

3.  Cliquez avec\-le bouton droit sur l’approbation de la partie de confiance pour laquelle vous souhaitez configurer l' **authentification\-multifacteur, puis sélectionnez Modifier l’authentification multifacteur personnalisée**ou, dans le volet **actions** , sélectionnez **modifier le multi\- -groupe personnalisé Authentification par facteur**.  

4.  Dans la fenêtre **modifier la stratégie d’authentification pour\_< nom\_de l’approbation de la partie\_de confiance >** , sous l' **\-onglet multifacteur** , vous pouvez configurer les paramètres suivants dans le\-cadredustratégie d’authentification par approbation de partie de confiance :  

    -   Paramètres ou conditions pour l’authentification MFA via les options disponibles dans les sections **groupes d'\/utilisateurs**, **appareils**et **emplacements** .  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurer des stratégies d’authentification via Windows PowerShell  
Windows PowerShell offre une plus grande flexibilité dans l’utilisation de différents facteurs de contrôle d’accès et le mécanisme d’authentification disponible dans AD FS dans Windows Server 2012 R2 pour configurer les stratégies d’authentification et les règles d’autorisation nécessaires pour Implémentez un véritable accès conditionnel pour \-vos ressources AD FS sécurisées.  

L’appartenance au groupe administrateurs, ou équivalent, sur l’ordinateur local est la condition minimale requise pour effectuer ces procédures.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les\/ \( [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) http :\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Pour configurer une méthode d’authentification supplémentaire via Windows PowerShell  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `
~~~


> [!WARNING]  
> Pour vérifier que cette commande s’est correctement exécutée, vous pouvez exécuter la commande `Get-AdfsGlobalAuthenticationPolicy` .  

### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Pour configurer l’authentification\-MFA par approbation de partie de confiance basée sur les données d’appartenance aux groupes d’un utilisateur  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante :  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!WARNING]  
> Veillez à remplacer *< approbation\_de\_partie de confiance >* par le nom de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  


~~~
$MfaClaimRule = “c:[Type == ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'”, Value =~ ‘“^(?i) <group_SID>$'”] => issue(Type = ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'”, Value ‘“https://schemas.microsoft.com/claims/multipleauthn'”);” 

Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
~~~


> [!NOTE]  
> Veillez à remplacer <\_SID de groupe > par la valeur du \(sid\) de l’identificateur de sécurité \(de\) votre groupe ad Active Directory.  

### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Pour configurer l’authentification MFA à l’échelle mondiale en fonction des données d’appartenance au groupe des utilisateurs  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
$MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", Value == ‘"group_SID'"]  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");”  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Veillez à remplacer *<\_SID de groupe >* par la valeur du SID de votre groupe Active Directory.  

### <a name="to-configure-mfa-globally-based-on-users-location"></a>Pour configurer l’authentification MFA à l’échelle mondiale en fonction de l’emplacement de l’utilisateur  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
$MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == ‘"true_or_false'"]  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");”  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~



> [!NOTE]  
> Veillez à remplacer *<\_true\_ou false >* par `false` `true` ou. La valeur dépend de votre condition de règle spécifique qui est basée sur le fait que la demande d’accès provient de l’extranet ou de l’intranet.  

### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Pour configurer l’authentification MFA à l’échelle mondiale en fonction des données de l’appareil de l’utilisateur  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
$MfaClaimRule = "c:[Type == ‘" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == ‘"true_or_false"']  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Veillez à remplacer *<\_true\_ou false >* par `false` `true` ou. La valeur dépend de votre condition de règle spécifique qui est basée sur le fait que l'\-appareil est joint à un espace de travail ou non.  

### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Pour configurer l’authentification multifacteur globalement si la demande d’accès provient de l’extranet et\-d'\-un appareil non joint à un espace de travail  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
~~~


> [!NOTE]  
> Veillez à remplacer les deux instances de *<\_true\_ou false >* par `true` ou `false`, ce qui dépend de vos conditions de règle spécifiques. Les conditions de règle sont basées sur si l’appareil est\-joint à un espace de travail ou non et si la demande d’accès provient de l’extranet ou de l’intranet.  

### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Pour configurer l’authentification MFA de manière globale si l’accès provient d’un utilisateur extranet appartenant à un certain groupe  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
~~~

> [!NOTE]  
> Veillez à *remplacer <\_SID de groupe >* par la valeur du SID du groupe et à *< les > true\_ou\_false* avec `true` ou `false`, ce qui dépend de votre condition de règle spécifique. est basé sur le fait que la demande d’accès provient de l’extranet ou de l’intranet.  

### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Pour accorder l’accès à une application en fonction des données utilisateur via Windows PowerShell  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Veillez à remplacer *< approbation\_de\_partie de confiance >* par la valeur de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  

   ```  

     $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
   Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
   ```  

> [!NOTE]  
> > Veillez à remplacer *<\_SID de groupe >* par la valeur du SID de votre groupe Active Directory.  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Pour accorder l’accès à une application sécurisée par AD FS uniquement si l’identité de cet utilisateur a été validée avec MFA  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Veillez à remplacer *< approbation\_de\_partie de confiance >* par la valeur de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  

   ```  
   $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
   @RuleName = `"PermitAccessWithMFA`"  
   c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim'");"  

   ```  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Pour accorder l’accès à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un\-appareil rattaché à un espace de travail qui est inscrit auprès de l’utilisateur  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Veillez à remplacer *< approbation\_de\_partie de confiance >* par la valeur de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
~~~



### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Pour accorder l’accès à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un\-appareil rattaché à un espace de travail qui est inscrit auprès d’un utilisateur dont l’identité a été validée avec MFA  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Veillez à remplacer *< approbation\_de\_partie de confiance >* par la valeur de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  

   ```  
   $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
   @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
   c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
   c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

   ```  

### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Pour accorder l’accès extranet à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un utilisateur dont l’identité a été validée avec MFA  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!NOTE]  
> Veillez à remplacer *< approbation\_de\_partie de confiance >* par la valeur de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"RequireMFAForExtranetAccess`"  
c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
~~~

## <a name="additional-references"></a>Références supplémentaires  

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
