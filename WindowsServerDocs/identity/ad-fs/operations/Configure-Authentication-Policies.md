---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: Configurer des stratégies d’authentification
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 46eb61db92207a73320f87790a4063076a3cac4f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817282"
---
# <a name="configure-authentication-policies"></a>Configurer des stratégies d’authentification

Dans AD FS, dans Windows Server 2012 R2, le contrôle d’accès et le mécanisme d’authentification sont améliorés avec plusieurs facteurs, notamment les données d’utilisateur, de périphérique, d’emplacement et d’authentification. Ces améliorations vous permettent, par le biais de l’interface utilisateur ou de Windows PowerShell, de gérer le risque d’accorder des autorisations d’accès à AD FS\-des applications sécurisées via le contrôle d’accès multifacteur\-et l’authentification multi\-, en fonction de l’identité de l’utilisateur ou de l’appartenance à un groupe, de l’emplacement réseau, des données de l’appareil\-jointe et de l'\-\(\)  

Pour plus d’informations sur l’authentification multifacteur et le contrôle d’accès à facteur multi\-dans Services ADFS \(AD FS\) dans Windows Server 2012 R2, consultez les rubriques suivantes :  


-   [Joindre un espace de travail à partir de n’importe quel appareil en utilisant l’authentification unique et l’authentification de second facteur transparente](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Gérer les risques avec le contrôle d’accès conditionnel](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurer des stratégies d’authentification via le\-du composant logiciel enfichable de gestion de AD FS  
Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent sur l’ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   

Dans AD FS, dans Windows Server 2012 R2, vous pouvez spécifier une stratégie d’authentification au niveau d’une étendue globale applicable à l’ensemble des applications et services sécurisés par AD FS. Vous pouvez également définir des stratégies d’authentification pour des applications et des services spécifiques qui reposent sur des approbations de tiers et qui sont sécurisés par AD FS. La spécification d’une stratégie d’authentification pour une application particulière par approbation de partie de confiance ne remplace pas la stratégie d’authentification globale. Si la stratégie d’authentification globale ou par approbation de partie de confiance requiert l’authentification MFA, l’authentification MFA est déclenchée lorsque l’utilisateur tente de s’authentifier auprès de cette approbation de partie de confiance. La stratégie d’authentification globale est une solution de secours pour les approbations de partie de confiance pour les applications et les services qui n’ont pas de stratégie d’authentification configurée spécifique. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Pour configurer globalement l’authentification principale dans Windows Server 2012 R2 

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans AD FS\-de composant logiciel enfichable, cliquez sur **stratégies d’authentification**.  

3.  Dans la section **authentification principale** , cliquez sur **modifier** en regard de **paramètres globaux**. Vous pouvez également cliquer avec le bouton droit\-sur **stratégies d’authentification**, sélectionner **modifier l’authentification principale globale**, ou, dans le volet **actions** , sélectionner **modifier l’authentification principale globale**.  
stratégies d’authentification ![](media/Configure-Authentication-Policies/authpolicy1.png)

4.  Dans la fenêtre **modifier la stratégie d’authentification globale** , sous l’onglet **principal** , vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification globale :  

    -   Méthodes d’authentification à utiliser pour l’authentification principale. Vous pouvez sélectionner les méthodes d’authentification disponibles sous **extranet** et **Intranet**.  

    -   Authentification de l’appareil via la case à cocher **activer l’authentification** de l’appareil. Pour plus d'informations, voir [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
stratégies d’authentification ![](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Pour configurer l’authentification principale par approbation de partie de confiance  

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans AD FS\-de composant logiciel enfichable dans, cliquez sur **stratégies d’authentification**\\**par approbation de partie de confiance**, puis cliquez sur l’approbation de la partie de confiance pour laquelle vous souhaitez configurer des stratégies d’authentification.  

3.  Cliquez avec le bouton\-droit sur l’approbation de la partie de confiance pour laquelle vous souhaitez configurer des stratégies d’authentification, puis sélectionnez **modifier l’authentification principale personnalisée**. ou bien, dans le volet **actions** , sélectionnez **modifier l’authentification principale personnalisée**.  
stratégies d’authentification ![](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  Dans la fenêtre **modifier la stratégie d’authentification pour < partie de\_de confiance\_\_> nom de l’approbation de** la partie de confiance, sous l’onglet **principal** , vous pouvez configurer le paramètre suivant dans le cadre de la stratégie d’authentification **par approbation de partie de confiance** :  

    -   Si les utilisateurs doivent fournir leurs informations d’identification à chaque fois qu’ils se connectent\-dans via, les **utilisateurs doivent fournir leurs informations d’identification à chaque fois qu'** ils se connectent\-dans la case à cocher.  
stratégies d’authentification ![](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Pour configurer l’authentification multifacteur globalement  

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans AD FS\-de composant logiciel enfichable, cliquez sur **stratégies d’authentification**.  

3.  Dans la section **authentification multi\-facteur** , cliquez sur **modifier** en regard de **paramètres globaux**. Vous pouvez également cliquer avec le bouton droit\-sur **stratégies d’authentification**, puis sélectionner **modifier l’authentification multifacteur globale multi\-** ou, dans le volet **actions** , sélectionner **modifier l’authentification multifacteur globale multi-\-** .  
stratégies d’authentification ![](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  Dans la fenêtre **modifier la stratégie d’authentification globale** , sous l’onglet **facteur multi\-** , vous pouvez configurer les paramètres suivants dans le cadre de la stratégie globale d’authentification multi-\-:  

    -   Paramètres ou conditions pour MFA via les options disponibles dans les sections **utilisateurs\/groupes**, **appareils**et **emplacements** .  

    -   Pour activer MFA pour l’un de ces paramètres, vous devez sélectionner au moins une méthode d’authentification supplémentaire. L' **authentification par certificat** est l’option par défaut disponible. Vous pouvez également configurer d’autres méthodes d’authentification supplémentaires personnalisées, par exemple, Windows Azure active Authentication. Pour plus d’informations, consultez [Guide de procédure pas à pas : gérer les risques avec des multi-Factor Authentication supplémentaires pour les applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  

> [!WARNING]  
> Vous pouvez uniquement configurer des méthodes d’authentification supplémentaires de manière globale.  
stratégies d’authentification ![](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Pour configurer multi\-Factor Authentication par approbation de partie de confiance  

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  

2.  Dans AD FS\-de composant logiciel enfichable dans, cliquez sur **stratégies d’authentification**\\**par approbation de partie de confiance**, puis cliquez sur l’approbation de la partie de confiance pour laquelle vous souhaitez configurer l’authentification multifacteur.  

3.  Cliquez avec le bouton\-droit sur l’approbation de la partie de confiance pour laquelle vous souhaitez configurer l' **authentification multifacteur, puis sélectionnez Modifier l’authentification multi-\-personnalisée**ou, dans le volet **actions** , sélectionnez **modifier l’authentification multifacteur\-personnalisée**.  

4.  Dans la fenêtre **modifier la stratégie d’authentification pour < partie de\_de confiance\_de l’approbation de\_nom de l' >** , sous l’onglet **facteur multi\-** , vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification d’approbation de partie de confiance par\-:  

    -   Paramètres ou conditions pour MFA via les options disponibles dans les sections **utilisateurs\/groupes**, **appareils**et **emplacements** .  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurer des stratégies d’authentification via Windows PowerShell  
Windows PowerShell offre une plus grande flexibilité dans l’utilisation de différents facteurs de contrôle d’accès et le mécanisme d’authentification disponible dans AD FS dans Windows Server 2012 R2 pour configurer les stratégies d’authentification et les règles d’autorisation nécessaires pour implémenter un accès conditionnel réel pour votre AD FS \-des ressources sécurisées.  

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe Administrateurs ou à un groupe équivalent sur l’ordinateur local.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Pour configurer une méthode d’authentification supplémentaire via Windows PowerShell  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `
~~~


> [!WARNING]  
> Pour vérifier que cette commande s’est correctement exécutée, vous pouvez exécuter la commande `Get-AdfsGlobalAuthenticationPolicy` .  

### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Pour configurer l’authentification MFA par\-approbation de partie de confiance basée sur les données d’appartenance aux groupes d’un utilisateur  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante :  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!WARNING]  
> Veillez à remplacer *<\_de confiance\_de confiance >* par le nom de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  


~~~
$MfaClaimRule = "c:[Type == '"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", Value =~ '"^(?i) <group_SID>$'"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value '"https://schemas.microsoft.com/claims/multipleauthn'");" 

Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
~~~


> [!NOTE]  
> Veillez à remplacer < groupe\_SID > par la valeur de l’identificateur de sécurité \(SID\) de votre groupe Active Directory \(AD\).  

### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Pour configurer l’authentification MFA à l’échelle mondiale en fonction des données d’appartenance au groupe des utilisateurs  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
$MfaClaimRule = "c:[Type == '" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", Value == '"group_SID'"]  
 => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = '"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Veillez à remplacer *< groupe\_sid >* par la valeur du SID de votre groupe Active Directory.  

### <a name="to-configure-mfa-globally-based-on-users-location"></a>Pour configurer l’authentification MFA à l’échelle mondiale en fonction de l’emplacement de l’utilisateur  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
$MfaClaimRule = "c:[Type == '" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '"true_or_false'"]  
 => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = '"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~



> [!NOTE]  
> Veillez à remplacer *< true\_ou\_false >* par `true` ou `false`. La valeur dépend de votre condition de règle spécifique qui est basée sur le fait que la demande d’accès provient de l’extranet ou de l’intranet.  

### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Pour configurer l’authentification MFA à l’échelle mondiale en fonction des données de l’appareil de l’utilisateur  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
$MfaClaimRule = "c:[Type == '" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false"']  
 => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = '"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Veillez à remplacer *< true\_ou\_false >* par `true` ou `false`. La valeur dépend de votre condition de règle spécifique qui est basée sur le fait que l’appareil est un espace de travail\-joint ou non.  

### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Pour configurer l’authentification multifacteur de manière globale si la demande d’accès provient de l’extranet et à partir d’un appareil non\-Workplace\-joint  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
~~~


> [!NOTE]  
> Veillez à remplacer les deux instances de *< true\_ou\_false >* par `true` ou `false`, ce qui dépend de vos conditions de règle spécifiques. Les conditions de règle sont basées sur le fait que l’appareil est un espace de travail\-joint ou non et si la demande d’accès provient de l’extranet ou de l’intranet.  

### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Pour configurer l’authentification MFA de manière globale si l’accès provient d’un utilisateur extranet appartenant à un certain groupe  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
~~~

> [!NOTE]  
> Veillez à remplacer *< groupe\_sid >* par la valeur du SID du groupe et *< true\_ou\_false >* par `true` ou `false`, ce qui dépend de votre condition de règle spécifique selon que la demande d’accès provient de l’extranet ou de l’intranet.  

### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Pour accorder l’accès à une application en fonction des données utilisateur via Windows PowerShell  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Veillez à remplacer *<\_de confiance\_de confiance >* par la valeur de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  

   ```  

     $GroupAuthzRule = "@RuleTemplate = `"Authorization`" @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
   Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
   ```  

> [!NOTE]  
> > Veillez à remplacer *< groupe\_sid >* par la valeur du SID de votre groupe Active Directory.  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Pour accorder l’accès à une application sécurisée par AD FS uniquement si l’identité de cet utilisateur a été validée avec MFA  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Veillez à remplacer *<\_de confiance\_de confiance >* par la valeur de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  

   ```  
   $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
   @RuleName = `"PermitAccessWithMFA`"  
   c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = '"PermitUsersWithClaim'");"  

   ```  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Pour accorder l’accès à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un espace de travail\-appareil joint qui est inscrit auprès de l’utilisateur  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Veillez à remplacer *<\_de confiance\_de confiance >* par la valeur de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
~~~



### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Pour accorder l’accès à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un espace de travail\-appareil joint qui est inscrit auprès d’un utilisateur dont l’identité a été validée avec MFA  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Veillez à remplacer *<\_de confiance\_de confiance >* par la valeur de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  

   ```  
   $GroupAuthzRule = '@RuleTemplate = "Authorization"  
   @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"  
   c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
   c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

   ```  

### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Pour accorder l’accès extranet à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un utilisateur dont l’identité a été validée avec MFA  

1.  Sur votre serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!NOTE]  
> Veillez à remplacer *<\_de confiance\_de confiance >* par la valeur de votre approbation de partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"RequireMFAForExtranetAccess`"  
c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
~~~

## <a name="additional-references"></a>Références supplémentaires  

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
