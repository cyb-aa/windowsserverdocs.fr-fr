---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a37c0b0a1de06b65e0cb867ec4d160bbb0373a15
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189075"
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles




-   [Configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [Guide pas à pas : Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [Configurer des méthodes d’authentification supplémentaires pour AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>Dans ce guide
Ce guide fournit les informations suivantes :

-   [Mécanismes d’authentification dans AD FS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1) -description des mécanismes d’authentification disponibles dans Active Directory Federation Services (ADFS) dans Windows Server 2012 R2

-   [Vue d’ensemble du scénario](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2) -une description d’un scénario où vous utilisez Active Directory Federation Services (ADFS) pour activer l’authentification multifacteur (MFA) basée sur l’appartenance au groupe de l’utilisateur.

    > [!NOTE]
    > Dans AD FS dans Windows Server 2012 R2 vous pouvez activer l’authentification Multifacteur selon l’emplacement réseau, une identité d’appareil et un utilisateur identité ou appartenance de groupe.

    Pour obtenir des instructions de la procédure pas à pas détaillée pour la configuration et la vérification de ce scénario, consultez [Guide pas à pas : Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

## <a name="BKMK_1"></a>Concepts clés : mécanismes d’authentification dans AD FS

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>Avantages des mécanismes d’authentification dans AD FS
Active Directory Federation Services (ADFS) dans Windows Server 2012 R2 fournit aux administrateurs informatiques avec un ensemble d’outils plus riches et plus flexible pour l’authentification des utilisateurs qui souhaitent accéder aux ressources d’entreprise. Il permet aux administrateurs un contrôle flexible sur les principales et les méthodes d’authentification supplémentaires, fournit une expérience de gestion complète pour la configuration de l’authentification des stratégies (à la fois via l’interface utilisateur et Windows PowerShell) et améliore l’expérience pour les utilisateurs finaux qui accèdent aux applications et services qui sont sécurisés par AD FS. Voici quelques-uns des avantages de la sécurisation de vos applications et services avec AD FS dans Windows Server 2012 R2 :

-   Stratégie d’authentification globale - une fonctionnalité de gestion centralisée, à partir de laquelle un administrateur informatique peut choisir les méthodes d’authentification sont utilisés pour authentifier les utilisateurs selon l’emplacement réseau à partir duquel ils accéder aux ressources protégées. Les administrateurs peuvent ainsi effectuer les opérations suivantes :

    -   Imposer l’utilisation de méthodes d’authentification plus sécurisées pour l’accès aux demandes provenant de l’extranet.

    -   Activer l’authentification de l’appareil pour l’authentification à deux facteurs transparente. Cela lie l’identité l’utilisateur à l’appareil inscrit qui est utilisé pour accéder à la ressource, offrant ainsi une vérification une identité composée plus sécurisée avant de l’accès aux ressources protégées.

        > [!NOTE]
        > Pour plus d’informations sur l’objet de l’appareil, Device Registration Service, jonction et l’appareil comme authentification à deux facteurs transparente et authentification unique, consultez [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et authentification de Second facteur transparente Entre les Applications d’entreprise](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

    -   Définir l’authentification multifacteur pour tous les accès extranet ou conditionnellement en fonction de l’identité l’utilisateur, emplacement réseau ou un périphérique qui est utilisé pour accéder aux ressources protégées.

-   Une plus grande souplesse dans la configuration des stratégies d’authentification : vous pouvez configurer des stratégies d’authentification personnalisées pour les ressources de AD sécurisée par ADFS avec différentes valeurs commerciales. Par exemple, vous pouvez exiger l’authentification multifacteur pour une application avec un fort impact commercial.

-   Facilité d’utilisation : outils de gestion simples et intuitifs tels que le composant logiciel enfichable MMC d’administration basée sur une interface graphique utilisateur AD FS et les applets de commande Windows PowerShell activer les administrateurs informatiques à configurer des stratégies d’authentification assez facilement. Avec Windows PowerShell, vous pouvez générer des scripts de vos solutions pour une utilisation dans les bonnes proportions et une automatisation des tâches d’administration courantes.

-   Plus grand contrôle sur les ressources de l’entreprise : dans la mesure où en tant qu’administrateur AD FS permet de configurer une stratégie d’authentification qui s’applique à une ressource spécifique, vous avez davantage contrôler sur les ressources de l’entreprise comment sont sécurisées. Les applications ne peuvent pas remplacer les stratégies d’authentification spécifiées par les administrateurs informatiques. Pour les services et applications sensibles, vous pouvez activer le critère d’authentification multifacteur, l’authentification de l’appareil et éventuellement une nouvelle authentification chaque fois que la ressource fait l’objet d’un accès.

-   Prise en charge des fournisseurs d’authentification Multifacteur personnalisés : pour les organisations qui bénéficient des méthodes d’authentification Multifacteur tierces, AD FS offre la possibilité d’incorporer et d’utiliser ces méthodes d’authentification en toute transparence.

### <a name="authentication-scope"></a>Étendue de l’authentification
Dans AD FS dans Windows Server 2012 R2, vous pouvez spécifier une stratégie d’authentification à une étendue globale qui s’applique à toutes les applications et services sécurisés par AD FS.  Vous pouvez également définir des stratégies d’authentification pour des applications et services (approbations de partie de confiance) qui sont sécurisés par AD FS. La spécification d’une stratégie d’authentification pour une application particulière (par approbation de partie de confiance) ne remplace pas la stratégie d’authentification globale. Si la stratégie d’authentification globale ou par approbation de partie de confiance exige l’authentification multifacteur, celle-ci est déclenchée lorsque l’utilisateur essaie de s’authentifier auprès de cette approbation de partie de confiance.  La stratégie d’authentification globale est une solution de secours pour les approbations de partie de confiance (applications et services) pour lesquelles aucune stratégie d’authentification spécifique n’est configurée.

Une stratégie d’authentification globale s’applique à toutes les parties de confiance qui sont sécurisés par AD FS. Vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification globale :

-   Méthodes d’authentification à utiliser pour l’authentification principale

-   Paramètres et méthodes pour l’authentification multifacteur

-   Activation ou non de l’authentification de l’appareil. Pour plus d’informations, consultez [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Les stratégies d’authentification par approbation de partie de confiance s’appliquent en particulier aux tentatives d’accès à cette approbation de partie de confiance (application ou service). Vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification par approbation de partie de confiance :

-   Nécessité ou non pour les utilisateurs de fournir leurs informations d’identification à chaque connexion

-   Paramètres d’authentification multifacteur selon l’utilisateur/le groupe, l’inscription de l’appareil et les données de l’emplacement de la demande d’accès

### <a name="primary-and-additional-authentication-methods"></a>Méthodes d’authentification principales et supplémentaires
Avec AD FS dans Windows Server 2012 R2, outre le mécanisme d’authentification principale, les administrateurs peuvent configurer des méthodes d’authentification supplémentaires. Méthodes d’authentification principales sont intégrées et destinées à valider les identités des utilisateurs. Vous pouvez configurer des facteurs d’authentification supplémentaires pour demander que plus d’informations sur l’identité de l’utilisateur sont fournis et garantir par conséquent une authentification renforcée.

Avec l’authentification principale dans AD FS dans Windows Server 2012 R2, vous disposez des options suivantes :

-   Pour les ressources publiées accessibles depuis l’extérieur du réseau d’entreprise, l’authentification par formulaires est sélectionnée par défaut. En outre, vous pouvez également activer l’authentification par certificat (autrement dit, une authentification basée sur une carte à puce ou une authentification par certificat client utilisateur qui fonctionne avec AD DS).

-   Pour les ressources intranet, l’authentification Windows est sélectionnée par défaut. En outre, vous pouvez également activer l’authentification par formulaires et/ou l’authentification par certificat.

En sélectionnant plusieurs méthodes d’authentification, vous permettez aux utilisateurs d’avoir le choix de la méthode pour s’authentifier au niveau de la page de connexion pour votre application ou service.

Vous pouvez également activer l’authentification de l’appareil pour l’authentification à deux facteurs transparente. Cela lie l’identité l’utilisateur à l’appareil inscrit qui est utilisé pour accéder à la ressource, offrant ainsi une vérification une identité composée plus sécurisée avant de l’accès aux ressources protégées.

> [!NOTE]
> Pour plus d’informations sur l’objet de l’appareil, Device Registration Service, jonction et l’appareil comme authentification à deux facteurs transparente et authentification unique, consultez [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et authentification de Second facteur transparente Entre les Applications d’entreprise](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Si vous spécifiez la méthode d’authentification Windows (option par défaut) pour vos ressources intranet, les demandes d’authentification subissent cette méthode en toute transparence sur des navigateurs qui prennent en charge l’authentification Windows.

> [!NOTE]
> L’authentification Windows n’est pas prise en charge sur tous les navigateurs. Le mécanisme d’authentification dans AD FS dans Windows Server 2012 R2 détecte l’agent utilisateur du navigateur de l’utilisateur et utilise un paramètre configurable pour déterminer si cet agent utilisateur prend en charge l’authentification Windows. Les administrateurs peuvent procéder à des ajouts dans cette liste d’agents utilisateurs (via la commande Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents`) afin d’indiquer d’autres chaînes de l’agent utilisateur pour les navigateurs qui prennent en charge l’authentification Windows. Si l’agent utilisateur du client ne prend pas en charge l’authentification Windows, la méthode de secours par défaut est l’authentification par formulaire.

### <a name="configuring-mfa"></a>Configuration de l’authentification multifacteur
Il existe deux parties pour configurer l’authentification Multifacteur dans AD FS dans Windows Server 2012 R2 : spécification des conditions dans lesquelles l’authentification Multifacteur est requise, puis en sélectionnant une méthode d’authentification supplémentaires. Pour plus d’informations sur les méthodes d’authentification supplémentaires, consultez [configurer des méthodes d’authentification supplémentaires pour AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).

**Paramètres d’authentification Multifacteur**

Les options suivantes sont disponibles pour les paramètres d’authentification multifacteur (conditions dans lesquelles l’authentification multifacteur doit être requise) :

-   Vous pouvez exiger l’authentification multifacteur pour des utilisateurs et groupes spécifiques dans le domaine Active Directory auquel votre serveur de fédération est rattaché.

-   Vous pouvez exiger l’authentification multifacteur pour des appareils inscrits (rattachés à un espace de travail) ou désinscrits (non rattachés à un espace de travail).

    Windows Server 2012 R2 adopte une approche orientée utilisateur dans lequel les objets appareils représentent une relation entre des appareils modernes user@device et une société. Objets d’appareil sont une nouvelle classe dans Active Directory dans Windows Server 2012 R2 qui peut être utilisé pour offrir une identité composée lors de l’accès aux applications et services. Un nouveau composant d’AD FS, DRS (Device Registration Service), configure une identité d’appareil dans Active Directory et définit un certificat sur l’appareil du consommateur qui sera utilisé pour représenter l’identité de l’appareil. Vous pouvez ensuite utiliser cette identité d’appareil pour rattacher votre appareil à l’espace de travail, autrement dit pour connecter votre appareil personnel à Active Directory sur votre espace de travail. Lorsque vous rattachez votre appareil personnel à votre espace de travail, il devient un appareil connu et fournit une authentification multifacteur transparente aux applications et ressources protégées. En d’autres termes, une fois un appareil est rattaché, identité de l’utilisateur est liée à cet appareil et peut être utilisée pour une vérification d’identité composée transparente avant d’accéder à une ressource protégée.

    Pour plus d’informations sur la jonction et le retrait, consultez [jointure au poste de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

-   Vous pouvez exiger l’authentification multifacteur lorsque la demande d’accès pour les ressources protégées provient de l’extranet ou de l’intranet.

## <a name="BKMK_2"></a>Vue d’ensemble du scénario
Dans ce scénario, vous allez autoriser MFA basée sur les données d’appartenance au groupe de l’utilisateur pour une application spécifique. Autrement dit, vous allez configurer une stratégie d’authentification sur votre serveur de fédération pour exiger une authentification multifacteur lorsque des utilisateurs qui appartiennent à un certain groupe demandent l’accès à une application spécifique qui est hébergée sur un serveur Web.

Plus spécifiquement, dans ce scénario, vous activez une stratégie d’authentification pour une application de test basée sur des revendications appelée **claimapp**, où l’utilisateur Active Directory **Robert Hatley** devra subir une authentification multifacteur, car il appartient à un groupe Active Directory nommé **Finance**.

Les instructions étape par étape pour configurer et vérifier ce scénario sont fournies dans [Guide pas à pas : Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md). Pour effectuer les étapes décrites dans cette procédure pas à pas, vous devez configurer un environnement de laboratoire et suivez les étapes de [configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Autres scénarios d’activer l’authentification Multifacteur dans AD FS sont les suivantes :

-   Activer l’authentification multifacteur si la demande d’accès provient de l’extranet. Vous pouvez modifier le code présenté dans la section « Configurer MFA la stratégie » de [Guide pas à pas : Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) par le code suivant :

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   Activer l’authentification multifacteur si la demande d’accès provient d’un appareil non rattaché à un espace de travail.  Vous pouvez modifier le code présenté dans la section « Configurer MFA la stratégie » de [Guide pas à pas : Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) par le code suivant :

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   Activer l’authentification multifacteur si la demande d’accès provient d’un utilisateur avec un appareil qui est rattaché à un espace de travail, mais non inscrit auprès de cet utilisateur. Vous pouvez modifier le code présenté dans la section « Configurer MFA la stratégie » de [Guide pas à pas : Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) par le code suivant :

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>Voir aussi
[Guide pas à pas : Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



