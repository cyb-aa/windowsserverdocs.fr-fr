---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: Gérer les risques avec le contrôle d’accès conditionnel
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2ad7d1467abd6d69077b515b8c69a65f7e70f19
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865590"
---
# <a name="manage-risk-with-conditional-access-control"></a>Gérer les risques avec le contrôle d’accès conditionnel

>S'applique à : Windows Server 2012 R2


-   [Contrôle de clé concepts-accès conditionnel dans AD FS](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Gestion des risques avec contrôle d’accès conditionnel](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="BKMK_1"></a>Concepts clés : contrôle d’accès conditionnel dans AD FS
La fonction globale des services AD FS consiste à émettre un jeton d’accès qui contient un ensemble de revendications. La décision concernant les revendications AD FS accepte et lui envoie ensuite est régie par les règles de revendication.

Contrôle d’accès dans AD FS est implémenté avec les règles de revendication d’autorisation d’émission qui sont utilisés pour émettre une autorisation ou refuser des revendications qui déterminent si un utilisateur ou un groupe d’utilisateurs pourront accéder aux ressources de sécurisée par ADFS AD ou non. Les règles d’autorisation ne peuvent être définies que sur des approbations de partie de confiance.

|Option de règle|Logique de règle|
|---------------|--------------|
|Autoriser tous les utilisateurs|Si le type de revendication entrante est égal à *tout type de revendication* et que la valeur est égale à *toute valeur*, la revendication émise avec la valeur est égale à *Autoriser*|
|Autoriser l’accès aux utilisateurs avec cette revendication entrante|Si le type de revendication entrante est égal à *type de revendication spécifié* et que la valeur est égale à *valeur de revendication spécifiée*, la revendication émise avec la valeur est égale à *Autoriser*|
|Refuser l’accès aux utilisateurs avec cette revendication entrante|Si le type de revendication entrante est égal à *type de revendication spécifié* et que la valeur est égale à *valeur de revendication spécifiée*, la revendication émise avec la valeur est égale à*Refuser*|

Pour plus d’informations sur ces options de règles et la logique, voir [When to Use an Authorization Claim Rule](https://technet.microsoft.com/library/ee913560.aspx).

Dans AD FS dans Windows Server 2012 R2, le contrôle d’accès a été amélioré avec plusieurs facteurs, notamment les données d’utilisateur, appareil, emplacement et l’authentification. Cela est possible grâce à une plus grande diversité de types de revendication disponibles pour les règles de revendication d’autorisation.  En d’autres termes, vous pouvez appliquer dans AD FS dans Windows Server 2012 R2, le contrôle d’accès conditionnel basé sur utilisateur identité ou appartenance de groupe, l’emplacement du réseau, appareil (s’il est rattaché, pour plus d’informations, consultez [Join to Workplace à partir d’un Appareil pour l’authentification unique et transparente deuxième facteur Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)) et l’état d’authentification (si l’authentification multifacteur (MFA) a été effectuée).

Contrôle d’accès conditionnel dans AD FS dans Windows Server 2012 R2 offre les avantages suivants :

-   Stratégies d’autorisation par application souples et expressives, par lesquelles vous pouvez autoriser ou refuser l’accès selon l’utilisateur, l’appareil, l’emplacement réseau et l’état d’authentification

-   Création de règles d’autorisation d’émission pour les applications de partie de confiance

-   Expérience de l’interface utilisateur améliorée pour les scénarios de contrôle d’accès conditionnel courants

-   Langage des revendications riche et prise en charge de Windows PowerShell pour les scénarios de contrôle d’accès conditionnel avancés

-   Personnalisé (par partie de confiance des applications de tiers) messages « Accès refusé ». Pour plus d'informations, voir [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx). En étant en mesure de personnaliser ces messages, vous pouvez expliquer pourquoi l’accès est refusé à un utilisateur et également faciliter la mise à jour libre-service lorsque cela est possible, par exemple demander aux utilisateurs de rattacher leurs appareils à l’espace de travail. Pour plus d'informations, voir [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Le tableau suivant inclut tous les types de revendications disponibles dans AD FS dans Windows Server 2012 R2 à utiliser pour l’implémentation de contrôle d’accès conditionnel.

|Type de revendication|Description|
|--------------|---------------|
|Adresse de messagerie|Adresse de messagerie de l’utilisateur.|
|Prénom|Prénom de l’utilisateur.|
|Nom|Nom unique de l’utilisateur.|
|UPN|Nom d’utilisateur principal (UPN) de l’utilisateur.|
|Nom commun|Nom commun de l’utilisateur.|
|Adresse de messagerie AD FS 1.x|Adresse de messagerie de l’utilisateur lorsqu’il interagit avec AD FS 1.1 ou AD FS 1.0.|
|Regrouper|Groupe auquel appartient l’utilisateur.|
|UPN AD FS 1.x|UPN de l’utilisateur lorsqu’il interagit avec AD FS 1.1 ou AD FS 1.0.|
|Rôle|Un rôle de l’utilisateur.|
|Nom|Nom de l’utilisateur.|
|PPID|Identificateur privé de l’utilisateur.|
|ID de nom|Identificateur de nom SAML de l’utilisateur.|
|Horodatage de l’authentification|Permet d’afficher l’heure et la date auxquelles l’utilisateur a été authentifié.|
|Méthodes d'authentification|Méthode qui permet d’authentifier l’utilisateur.|
|SID de groupe en refus seul|SID de groupe en refus seul de l’utilisateur.|
|SID principal en refus seul|SID principal en refus seul de l’utilisateur.|
|SID de groupe principal en refus seul|SID de groupe principal en refus seul de l’utilisateur.|
|SID de groupe|SID de groupe de l’utilisateur.|
|SID de groupe principal|SID de groupe principal de l’utilisateur.|
|SID principal|SID principal de l’utilisateur.|
|Nom de compte Windows|Nom du compte de domaine de l’utilisateur sous la forme domaine\utilisateur.|
|Est un utilisateur inscrit|L’utilisateur a été inscrit pour utiliser cet appareil.|
|Identificateur de l’appareil|Identificateur de l’appareil.|
|Identificateur d’inscription de l’appareil|Identificateur pour l’inscription de l’appareil.|
|Nom complet d’inscription d’appareil|Nom complet de l’inscription de l’appareil.|
|Type de système d’exploitation de l’appareil|Type de système d’exploitation de l’appareil.|
|Version du système d’exploitation de l’appareil|Version du système d’exploitation de l’appareil.|
|Est un appareil géré|L’appareil est géré par un service de gestion.|
|IP transférée du client|Adresse IP de l’utilisateur.|
|Application cliente|Type de l’application cliente.|
|Agent utilisateur du client|Type d’appareil utilisé par le client pour accéder à l’application.|
|IP du client|Adresse IP du client.|
|Chemin du point de terminaison|Chemin absolu du point de terminaison, pouvant être utilisé pour distinguer les clients actifs des clients passifs.|
|Proxy|Nom DNS du serveur proxy de fédération qui a transmis la demande.|
|Identificateur d’application|Identificateur de la partie de confiance.|
|Stratégies d’application|Stratégies d’application du certificat.|
|Identificateur de clé de l’autorité|Extension d’identificateur de clé d’autorité du certificat signataire d’un certificat émis.|
|Contrainte de base|L’une des contraintes de base du certificat.|
|Utilisation améliorée de la clé|Décrit l’une des principales utilisations améliorées de la clé du certificat.|
|Émetteur|Nom de l’autorité de certification qui a émis le certificat X.509.|
|Nom de l’émetteur|Nom unique de l’émetteur du certificat.|
|Utilisation de la clé|L’une des utilisations de la clé du certificat.|
|Pas après|Date après laquelle un certificat n’est plus valide, en heure locale.|
|Pas avant|Date à laquelle un certificat devient valide, en heure locale.|
|Stratégies de certificat|Stratégies sous lesquelles le certificat a été émis.|
|Clé publique|Clé publique du certificat.|
|Données brutes du certificat|Données brutes du certificat.|
|Autre nom de l’objet|L’un des autres noms du certificat.|
|Numéro de série|Numéro de série du certificat.|
|Algorithme de signature|Algorithme utilisé pour créer la signature d’un certificat.|
|Objet|Sujet du certificat.|
|Identificateur de la clé du sujet|Identificateur de la clé du sujet du certificat.|
|Nom de sujet|Nom unique du sujet d’un certificat.|
|Nom du modèle V2|Nom du modèle de certificat version 2 utilisé lors de l’émission ou du renouvellement d’un certificat. Il s’agit d’une valeur spécifique à Microsoft.|
|Nom du modèle V1|Nom du modèle de certificat version 1 utilisé lors de l’émission ou du renouvellement d’un certificat. Il s’agit d’une valeur spécifique à Microsoft.|
|Empreinte numérique|Empreinte numérique du certificat.|
|Version X.509|Version du certificat au format X.509.|
|Dans le périmètre du réseau d’entreprise|Permet d’indiquer si une demande provient ou non du réseau d’entreprise.|
|Heure d’expiration du mot de passe|Permet d’afficher l’heure d’expiration du mot de passe.|
|Jours avant expiration du mot de passe|Permet d’afficher le nombre de jours avant l’expiration du mot de passe.|
|Mettre à jour l’URL du mot de passe|Permet d’afficher l’adresse Web du service de mise à jour du mot de passe.|
|Références des méthodes d’authentification|Permet d’indiquer toutes les méthodes d’authentification utilisées pour authentifier l’utilisateur.|

## <a name="BKMK_2"></a>Gestion des risques avec contrôle d’accès conditionnel
À l’aide des paramètres disponibles, il existe de nombreuses façons de gérer les risques en implémentant le contrôle d’accès conditionnel.

### <a name="common-scenarios"></a>Scénarios courants
Par exemple, imaginez un scénario simple de l’implémentation de contrôle d’accès conditionnel basé sur les données d’appartenance au groupe de l’utilisateur pour une application particulière (tiers de confiance). En d’autres termes, vous pouvez configurer une règle d’autorisation d’émission sur votre serveur de fédération pour permettre aux utilisateurs qui appartiennent à un certain groupe dans AD domaine l’accès à une application particulière sécurisée par AD FS.  Étape par étape instructions détaillées (à l’aide de l’interface utilisateur et Windows PowerShell) pour l’implémentation de ce scénario sont abordées dans [Guide pas à pas : Gérer les risques avec contrôle d’accès conditionnel](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md). Pour effectuer les étapes décrites dans cette procédure pas à pas, vous devez configurer un environnement de laboratoire et suivez les étapes de [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

### <a name="advanced-scenarios"></a>Scénarios avancés
Autres exemples d’implémentation du contrôle d’accès conditionnel dans AD FS dans Windows Server 2012 R2 sont les suivants :

-   Autoriser l’accès à une application sécurisée par AD FS uniquement si l’identité de cet utilisateur a été validée avec l’authentification Multifacteur

    Vous pouvez utiliser le code suivant :

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Autoriser l’accès à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un appareil joint à un espace de travail qui est inscrit à l’utilisateur

    Vous pouvez utiliser le code suivant :

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Autoriser l’accès à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un appareil joint à un espace de travail qui est inscrit à un utilisateur dont l’identité a été validée avec l’authentification Multifacteur

    Vous pouvez utiliser le code suivant :

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Autoriser l’accès extranet à une application sécurisée par AD FS uniquement si la demande d’accès provient d’un utilisateur dont l’identité a été validée avec l’authentification Multifacteur.

    Vous pouvez utiliser le code suivant :

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>Voir aussi
[Guide pas à pas : Gérer les risques avec contrôle d’accès conditionnel](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)
[configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



