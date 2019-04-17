---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: "Gérer les risques avec contrôle d’accès conditionnel"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2ad7d1467abd6d69077b515b8c69a65f7e70f19
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-conditional-access-control"></a>Gérer les risques avec contrôle d’accès conditionnel

>S’applique à: Windows Server2012R2


-   [Contrôle d’accès aux clés de concepts-conditionnels dans ADFS](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Gestion des risques avec contrôle d’accès conditionnel](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="BKMK_1"></a>Concepts clés: contrôle d’accès conditionnel dans ADFS
La fonction globale des services ADFS consiste à émettre un jeton d’accès qui contient un ensemble de revendications. La décision concernant les revendications ADFS, puis émises est régie par les règles de revendication.

Le contrôle d’accès dans ADFS est implémenté avec des règles de revendication d’autorisation d’émission qui sont utilisées pour émettre une autorisation ou refuser des revendications qui déterminent si un utilisateur ou un groupe d’utilisateurs pourront accéder aux ressources de sécurisée par ADFS d’ActiveDirectory ou non. Règles d’autorisation ne peuvent être définies sur les approbations de partie de confiance.

|Option de règle|Logique de règle|
|---------------|--------------|
|Autoriser tous les utilisateurs|Si le type de revendication entrante est égal à *tout type de revendication* et que la valeur est égale à *n’importe quelle valeur*, revendication émise avec la valeur est égale à *autoriser*|
|Autoriser l’accès aux utilisateurs avec cette revendication entrante|Si le type de revendication entrante est égal à *type de revendication spécifiée* et que la valeur est égale à *valeur de revendication spécifiée*, revendication émise avec la valeur est égale à *autoriser*|
|Refuser l’accès aux utilisateurs avec cette revendication entrante|Si le type de revendication entrante est égal à *type de revendication spécifiée* et que la valeur est égale à *valeur de revendication spécifiée*, revendication émise avec la valeur est égale à *refuser*|

Pour plus d’informations sur ces options de règle et la logique, voir [quand utiliser une règle de revendication d’autorisation](https://technet.microsoft.com/library/ee913560.aspx).

Dans ADFS dans Windows Server2012R2, le contrôle d’accès a été amélioré avec plusieurs facteurs, notamment les données d’utilisateur, appareil, emplacement et d’authentification. Cela est rendu possible par une plus grande variété de types de revendications disponible pour les règles de revendication d’autorisation.  En d’autres termes, vous pouvez appliquer dans ADFS dans Windows Server2012R2, le contrôle d’accès conditionnel basé sur l’appartenance au groupe ou d’identité de l’utilisateur, l’emplacement réseau, appareil (qu’il s’agisse d’espace de travail joint, pour plus d’informations, voir [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur d’authentification entre les Applications d’entreprise](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)) et l’état d’authentification (si l’authentification multifacteur (MFA) a été effectuée).

Contrôle d’accès conditionnel dans ADFS dans Windows Server2012R2 offre les avantages suivants:

-   Stratégies d’autorisation par application souples et expressives, dans laquelle vous pouvez autoriser ou refuser l’accès en fonction de l’utilisateur, d’appareil, emplacement réseau et l’état d’authentification

-   Création des règles d’autorisation pour les applications tierces partie de confiance d’émission

-   Expérience riche UI pour les scénarios de contrôle d’accès conditionnel courants

-   Langage des revendications riche et Windows PowerShell prise en charge pour les scénarios de contrôle d’accès conditionnel avancés

-   Personnalisé (par partie de confiance une application tierce) messages «Accès refusé». Pour plus d’informations, voir [personnalisation des Pages ADFS Sign-in](https://technet.microsoft.com/library/dn280950.aspx). En étant en mesure de personnaliser ces messages, vous pouvez expliquer pourquoi un utilisateur l’accès est refusé et également faciliter la mise à jour libre-service a où il est possible, par exemple, demander aux utilisateurs un espace de travail joindre leurs appareils. Pour plus d’informations, voir [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur d’authentification entre les Applications d’entreprise](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Le tableau suivant inclut tous les types de revendications disponibles dans ADFS dans Windows Server2012R2 à utiliser pour l’implémentation du contrôle d’accès conditionnel.

|Type de revendication|Description|
|--------------|---------------|
|Adresse de messagerie|Adresse de messagerie de l’utilisateur.|
|Prénom|Prénom de l’utilisateur.|
|Nom|Le nom unique de l’utilisateur,|
|UPN|Le nom d’utilisateur principal (UPN) de l’utilisateur.|
|Nom commun|Le nom commun de l’utilisateur.|
|ADFS 1.x adresse de messagerie|Adresse de messagerie de l’utilisateur lorsqu’il interagit avec ADFS 1.1 ou ADFS 1.0.|
|Groupe|Un groupe de l’utilisateur est membre.|
|ADFS x UPN|UPN de l’utilisateur lorsqu’il interagit avec ADFS 1.1 ou ADFS 1.0.|
|Rôle|Un rôle de l’utilisateur.|
|Nom de famille|Le nom de l’utilisateur.|
|PPID|Identificateur privé de l’utilisateur.|
|Identificateur de nom|L’identificateur de nom SAML de l’utilisateur.|
|Horodatage de l’authentification|Permet d’afficher la date et heure auxquelles l’utilisateur a été authentifié.|
|Méthode d’authentification|La méthode utilisée pour authentifier l’utilisateur.|
|Refuser le SID de groupe uniquement|Le groupe en refus seul SID de l’utilisateur.|
|Refuser uniquement les SID principal|Refus seul SID principal de l’utilisateur.|
|Refuser uniquement SID de groupe principal|Le principal en refus seul SID de groupe de l’utilisateur.|
|SID de groupe|Le SID du groupe de l’utilisateur.|
|SID de groupe principal|Le SID de groupe principal de l’utilisateur.|
|SID principal|SID principal de l’utilisateur.|
|Nom du compte Windows|Le nom de compte de domaine de l’utilisateur sous la forme domaine\utilisateur.|
|Est utilisateur inscrit|Utilisateur est inscrit pour utiliser cet appareil.|
|Identificateur de l’appareil|Identificateur de l’appareil.|
|Identificateur d’inscription de périphérique|Identificateur d’inscription de l’appareil.|
|Nom d’affichage de l’inscription de périphérique|Nom complet de l’inscription de l’appareil.|
|Type de système d’exploitation de périphérique|Type de système d’exploitation de l’appareil.|
|Version du système d’exploitation du périphérique|Version du système d’exploitation de l’appareil.|
|Est un appareil géré|Périphérique est géré par un service de gestion.|
|Transférée du Client IP|Adresse IP de l’utilisateur.|
|Application cliente|Type de l’application cliente.|
|Agent utilisateur du client|Type d’appareil du client utilise pour accéder à l’application.|
|Client IP|Adresse IP du client.|
|Chemin d’accès du point de terminaison|Point de terminaison chemin d’accès absolu qui peut être utilisée pour déterminer les clients passifs.|
|Proxy|Nom DNS du serveur proxy de fédération qui a transmis la demande.|
|Identificateur de l’application|Identificateur de la partie de confiance.|
|Stratégies d’application|Stratégies d’application du certificat.|
|Identificateur de clé d’autorité|L’extension d’identificateur de clé autorité du certificat qui a signé un certificat émis.|
|Contrainte de base|Une des contraintes de base du certificat.|
|Utilisation de clé améliorée|Décrit l’une des principales utilisations améliorées du certificat.|
|Émetteur|Le nom de l’autorité de certification qui a émis le certificat X.509.|
|Nom de l’émetteur|Le nom unique de l’émetteur du certificat.|
|Utilisation de la clé|Une des utilisations de la clé du certificat.|
|Pas après|La date en heure locale après laquelle un certificat n’est plus valide.|
|Pas avant|La date en heure locale à laquelle un certificat devenue valide.|
|Stratégies de certificat|Les stratégies sous lequel le certificat a été émis.|
|Clé publique|Clé publique du certificat.|
|Données brutes du certificat|Les données brutes du certificat.|
|Autre nom d’objet|Un des autres noms du certificat.|
|Numéro de série|Le numéro de série du certificat.|
|Algorithme de signature|L’algorithme utilisé pour créer la signature d’un certificat.|
|Objet|Le sujet du certificat.|
|Identificateur de clé du sujet|Identificateur de clé de sujet du certificat.|
|Nom du sujet|Nom unique du sujet à partir d’un certificat.|
|V2 Nom du modèle|Le nom de la version2 du modèle de certificat utilisé lors de l’émission ou du renouvellement d’un certificat. Il s’agit d’une valeur spécifique à Microsoft.|
|V1 Nom du modèle|Le nom du modèle de certificat version1 utilisé lors de l’émission ou du renouvellement d’un certificat. Il s’agit d’une valeur spécifique à Microsoft.|
|Empreinte numérique|Empreinte numérique du certificat.|
|Version x.509|La version de format X.509 du certificat.|
|Dans le périmètre réseau d’entreprise|Permet d’indiquer si une demande provient du réseau d’entreprise.|
|Délai d’Expiration de mot de passe|Permet d’afficher l’heure d’expiration du mot de passe.|
|Délai d’Expiration de mot de passe|Permet d’afficher le nombre de jours à l’expiration du mot de passe.|
|URL de mot de passe de mise à jour|Permet d’afficher l’adresse web du service de mise à jour de mot de passe.|
|Références des méthodes d’authentification|Permet d’indiquer les méthodes d’authentification al utilisés pour authentifier l’utilisateur.|

## <a name="BKMK_2"></a>Gestion des risques avec contrôle d’accès conditionnel
En utilisant les paramètres disponibles, il existe de nombreuses façons dans laquelle vous pouvez gérer les risques en implémentant le contrôle d’accès conditionnel.

### <a name="common-scenarios"></a>Scénarios courants
Par exemple, imaginons un scénario simple de l’implémentation du contrôle d’accès conditionnel basé sur les données d’appartenance au groupe de l’utilisateur pour une application particulière (partie de confiance). En d’autres termes, vous pouvez configurer une règle d’autorisation d’émission sur votre serveur de fédération pour permettre aux utilisateurs qui appartiennent à un groupe dans AD accès au domaine à une application particulière sécurisée par ADFS.  L’étape par étape des instructions détaillées (à l’aide de l’interface utilisateur et Windows PowerShell) pour l’implémentation de ce scénario sont abordées dans [Guide pas à pas: gérer les risques avec le contrôle d’accès conditionnel](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md). Afin d’effectuer les étapes décrites dans cette procédure pas à pas, vous devez configurer un environnement de laboratoire et suivez les étapes de [configurer l’environnement lab pour ADFS dans Windows Server2012R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

### <a name="advanced-scenarios"></a>Scénarios avancés
Autres exemples de l’implémentation du contrôle d’accès conditionnel dans ADFS dans Windows Server2012R2 sont les suivants:

-   Autoriser l’accès à une application sécurisée par ADFS uniquement si l’identité de cet utilisateur a été validée avec l’authentification Multifacteur

    Vous pouvez utiliser le code suivant:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Autoriser l’accès à une application sécurisée par ADFS uniquement si la demande d’accès provient d’un appareil joint à un espace de travail qui est enregistré à l’utilisateur

    Vous pouvez utiliser le code suivant:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Autoriser l’accès à une application sécurisée par ADFS uniquement si la demande d’accès provient d’un appareil joint à un espace de travail qui est enregistré à un utilisateur dont l’identité a été validée avec l’authentification Multifacteur

    Vous pouvez utiliser le code suivant

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Autoriser l’accès extranet à une application sécurisée par ADFS uniquement si la demande d’accès provient d’un utilisateur dont l’identité a été validée avec l’authentification Multifacteur.

    Vous pouvez utiliser le code suivant:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>Voir aussi
[Guide pas à pas: Gérer les risques avec le contrôle d’accès conditionnel](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)<ph x="2">
</ph>[configurer l’environnement lab pour ADFS dans Windows Server2012R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



