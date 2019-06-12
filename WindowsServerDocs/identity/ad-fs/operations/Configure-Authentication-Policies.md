---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: Configurer des stratégies d’authentification
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2215f172128a533e0e0d4e10b72be53ad455a262
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444961"
---
# <a name="configure-authentication-policies"></a>Configurer des stratégies d’authentification

Dans AD FS, dans Windows Server 2012 R2, à la fois le contrôle d’accès et le mécanisme d’authentification ont été améliorés avec plusieurs facteurs qui comprennent les données utilisateur, appareil, emplacement et l’authentification. Ces améliorations vous permettent, via l’interface utilisateur ou via Windows PowerShell, pour gérer le risque d’octroi d’autorisations d’accès aux services AD FS\-sécurisé d’applications via plusieurs\-facteur de contrôle d’accès et multi\-l’authentification multifacteur qui reposent sur l’appartenance au identité ou du groupe d’utilisateurs, emplacement réseau, données de l’appareil qui sont l’espace de travail\-joint, et l’état de l’authentification lorsque plusieurs\-authentification multifacteur \(MFA\) a été effectuée.  

Pour plus d’informations sur l’authentification Multifacteur et multi\-factoriser le contrôle d’accès dans Active Directory Federation Services \(AD FS\) dans Windows Server 2012 R2, consultez les rubriques suivantes :  


-   [Rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente du facteur d’authentification entre les Applications d’entreprise](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [Gérer les risques avec le contrôle d’accès conditionnel](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>Configurer des stratégies d’authentification via le composant logiciel enfichable Gestion AD FS\-dans  
Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent sur l’ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   

Dans AD FS, dans Windows Server 2012 R2, vous pouvez spécifier une stratégie d’authentification à une étendue globale qui s’applique à toutes les applications et services sécurisés par AD FS. Vous pouvez également définir des stratégies d’authentification pour des applications et services qui s’appuient sur les approbations et qui sont sécurisés par AD FS. Spécification d’une stratégie d’authentification pour une application particulière par partie de confiance de confiance ne remplace pas la stratégie d’authentification globale. Si global ou par partie de confiance approbation de la stratégie d’authentification requiert l’authentification Multifacteur, celle-ci est déclenchée lorsque l’utilisateur tente de s’authentifier auprès de cette partie de confiance. La stratégie d’authentification globale est une action de secours pour les approbations de partie de confiance pour les applications et services qui n’ont pas d’une stratégie d’authentification configuré spécifique. 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>Pour configurer l’authentification principale globalement dans Windows Server 2012 R2 

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  

2.  Dans AD FS aligner\-dans, cliquez sur **stratégies d’authentification**.  

3.  Dans le **l’authentification principale** , cliquez sur **modifier** regard **paramètres globaux**. Vous pouvez également avec le bouton droit\-cliquez sur **stratégies d’authentification**, puis sélectionnez **modifier l’authentification principale globale**, ou sous le **Actions** volet, sélectionnez  **Modifier l’authentification principale globale**.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy1.png)

4.  Dans le **modifier la stratégie d’authentification globale** fenêtre, dans le **principal** onglet, vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification globale :  

    -   Méthodes d’authentification à utiliser pour l’authentification principale. Vous pouvez sélectionner les méthodes d’authentification disponibles sous le **Extranet** et **Intranet**.  

    -   L’authentification des appareils via le **activer l’authentification des appareils** case à cocher. Pour plus d'informations, voir [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>Pour configurer l’authentification principale par partie de confiance de confiance  

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  

2.  Dans AD FS aligner\-dans, cliquez sur **stratégies d’authentification**\\**par confiance**, puis cliquez sur la partie de confiance pour lequel vous souhaitez configurer l’authentification stratégies.  

3.  À droite\-cliquez sur la partie de confiance pour lequel vous souhaitez configurer des stratégies d’authentification, puis sélectionnez **modifier personnalisé l’authentification principale**, ou sous le **Actions** volet, Sélectionnez **modifier personnalisé l’authentification principale**.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  Dans le **modifier la stratégie d’authentification pour < partie de confiance\_tiers\_approbation\_nom >** fenêtre, sous le **principal** onglet, vous pouvez configurer le paramètre suivant dans le cadre de la **par Relying Party Trust** stratégie d’authentification :  

    -   Indique si les utilisateurs doivent fournir leurs informations d’identification chaque fois arobase\-dans via la **les utilisateurs doivent fournir leurs informations d’identification chaque fois arobase\-dans** case à cocher.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>Pour configurer l’authentification multifacteur dans le monde entier  

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  

2.  Dans AD FS aligner\-dans, cliquez sur **stratégies d’authentification**.  

3.  Dans le **Multi\-facteur d’authentification** , cliquez sur **modifier** regard **paramètres globaux**. Vous pouvez également avec le bouton droit\-cliquez sur **stratégies d’authentification**, puis sélectionnez **modifier les multiples Global\-facteur d’authentification**, ou sous le **Actions**volet, sélectionnez **modifier les multiples Global\-facteur d’authentification**.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  Dans le **modifier la stratégie d’authentification globale** fenêtre, sous le **Multi\-facteur** onglet, vous pouvez configurer les paramètres suivants dans le cadre de la multi global\-facteur stratégie d’authentification :  

    -   Paramètres ou des conditions pour l’authentification Multifacteur via les options disponibles sous le **utilisateurs\/groupes**, **appareils**, et **emplacements** sections.  

    -   Pour activer l’authentification Multifacteur pour un de ces paramètres, vous devez sélectionner au moins une méthode d’authentification supplémentaire. **Le certificat d’authentification** option disponible par défaut. Vous pouvez également configurer d’autres méthodes d’authentification supplémentaires personnalisées, par exemple, Windows Azure Active Authentication. Pour plus d’informations, consultez [Guide pas à pas : Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  

> [!WARNING]  
> Vous pouvez uniquement configurer des méthodes d’authentification supplémentaires dans le monde entier.  
![stratégies d’authentification](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>Pour configurer plusieurs\-authentification multifacteur par partie de confiance de confiance  

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  

2.  Dans AD FS aligner\-dans, cliquez sur **stratégies d’authentification**\\**par confiance**, puis cliquez sur la partie de confiance pour lequel vous souhaitez configurer l’authentification Multifacteur.  

3.  À droite\-cliquez sur la partie de confiance pour lequel vous souhaitez configurer l’authentification Multifacteur, puis sélectionnez **modifier les multiples personnalisé\-facteur d’authentification**, ou sous le **Actions** volet, Sélectionnez **modifier les multiples personnalisé\-facteur d’authentification**.  

4.  Dans le **modifier la stratégie d’authentification pour < partie de confiance\_tiers\_approbation\_nom >** fenêtre, sous le **Multi\-facteur** onglet, vous pouvez Configurez les paramètres suivants dans le cadre de la par\-stratégie d’authentification d’approbation par partie de confiance :  

    -   Paramètres ou des conditions pour l’authentification Multifacteur via les options disponibles sous le **utilisateurs\/groupes**, **appareils**, et **emplacements** sections.  

## <a name="configure-authentication-policies-via-windows-powershell"></a>Configurer des stratégies d’authentification via Windows PowerShell  
Windows PowerShell permet une plus grande flexibilité à l’aide de différents facteurs de contrôle d’accès et le mécanisme d’authentification qui sont disponibles dans AD FS dans Windows Server 2012 R2 pour configurer des stratégies d’autorisation et l’authentification des règles qui sont nécessaires pour implémenter l’accès conditionnel true pour AD FS \-des ressources sécurisées.  

L’appartenance à des administrateurs, ou équivalent, sur l’ordinateur local est le minimum requis pour réaliser ces procédures.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>Pour configurer la méthode d’authentification supplémentaire via Windows PowerShell  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `
~~~


> [!WARNING]  
> Pour vérifier que cette commande s’est correctement exécutée, vous pouvez exécuter la commande `Get-AdfsGlobalAuthenticationPolicy` .  

### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>Pour configurer l’authentification Multifacteur par\-de confiance qui est basée sur les données d’appartenance au groupe d’un utilisateur  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante :  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!WARNING]  
> Veillez à remplacer *< partie de confiance\_tiers\_confiance >* par le nom de votre partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  


~~~
$MfaClaimRule = “c:[Type == ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid’”, Value =~ ‘“^(?i) <group_SID>$’”] => issue(Type = ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’”, Value ‘“https://schemas.microsoft.com/claims/multipleauthn’”);” 

Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
~~~


> [!NOTE]  
> Veillez à remplacer < groupe\_SID > avec la valeur de l’identificateur de sécurité \(SID\) de votre Active Directory \(AD\) groupe.  

### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>Pour configurer l’authentification Multifacteur dans le monde entier en fonction des données d’appartenance au groupe utilisateurs  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
$MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid’", Value == ‘"group_SID’"]  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn’");”  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Veillez à remplacer *< groupe\_SID >* avec la valeur du SID de groupe Active Directory.  

### <a name="to-configure-mfa-globally-based-on-users-location"></a>Pour configurer l’authentification Multifacteur dans le monde entier basé sur l’emplacement de l’utilisateur  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
$MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork’", Value == ‘"true_or_false’"]  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn’");”  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~



> [!NOTE]  
> Veillez à remplacer *< true\_ou\_false >* avec soit `true` ou `false`. La valeur dépend de votre condition de règle spécifique qui est basée sur indique si la demande d’accès provient de l’extranet ou intranet.  

### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>Pour configurer l’authentification Multifacteur dans le monde entier en fonction des données de périphérique de l’utilisateur  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
$MfaClaimRule = "c:[Type == ‘" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser’", Value == ‘"true_or_false"']  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn’");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> Veillez à remplacer *< true\_ou\_false >* avec soit `true` ou `false`. La valeur dépend de votre condition de règle spécifique dépend de si l’appareil est workplace\-joint ou non.  

### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>Pour configurer l’authentification Multifacteur dans le monde entier si la demande d’accès est fourni à partir de l’extranet et d’un texte non\-workplace\-joint à un appareil  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
~~~


> [!NOTE]  
> Veillez à remplacer les deux instances de *< true\_ou\_false >* avec soit `true` ou `false`, qui varie selon vos conditions de règle spécifique. Les conditions de règle sont basées selon que l’appareil est workplace\-joint ou non et si la demande d’accès provient de l’extranet ou intranet.  

### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>Pour configurer l’authentification Multifacteur dans le monde entier si accès provient d’un utilisateur de l’extranet qui appartient à un certain groupe  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
~~~

> [!NOTE]  
> Veillez à remplacer *< groupe\_SID >* avec la valeur du SID de groupe et *< true\_ou\_false >* avec soit `true` ou `false`, qui dépend de votre condition de règle spécifique qui est basée sur indique si la demande d’accès provient de l’extranet ou intranet.  

### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>Pour accorder l’accès à une application basée sur les données utilisateur via Windows PowerShell  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Veillez à remplacer *< partie de confiance\_tiers\_confiance >* avec la valeur de votre partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  

   ```  

     $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
   Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
   ```  

> [!NOTE]  
> > Veillez à remplacer *< groupe\_SID >* avec la valeur du SID de groupe Active Directory.  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>Pour accorder l’accès à une application sécurisée par seulement si de AD FS identité de cet utilisateur a été validée avec l’authentification Multifacteur  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Veillez à remplacer *< partie de confiance\_tiers\_confiance >* avec la valeur de votre partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  

   ```  
   $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
   @RuleName = `"PermitAccessWithMFA`"  
   c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  

   ```  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>Pour accorder l’accès à une application sécurisée par seulement si de AD FS l’accès demande provient d’un espace de travail\-joint à un appareil qui est inscrit à l’utilisateur  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> Veillez à remplacer *< partie de confiance\_tiers\_confiance >* avec la valeur de votre partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
~~~



### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>Pour accorder l’accès à une application sécurisée par seulement si de AD FS l’accès demande provient d’un espace de travail\-joint à un appareil qui est inscrit à un utilisateur dont l’identité a été validée avec l’authentification Multifacteur  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> Veillez à remplacer *< partie de confiance\_tiers\_confiance >* avec la valeur de votre partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  

   ```  
   $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
   @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
   c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
   c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

   ```  

### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>Pour accorder l’accès extranet à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un utilisateur dont l’identité a été validée avec l’authentification Multifacteur  

1.  Sur votre serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!NOTE]  
> Veillez à remplacer *< partie de confiance\_tiers\_confiance >* avec la valeur de votre partie de confiance.  

2. Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"RequireMFAForExtranetAccess`"  
c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
~~~

## <a name="additional-references"></a>Références supplémentaires  

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
