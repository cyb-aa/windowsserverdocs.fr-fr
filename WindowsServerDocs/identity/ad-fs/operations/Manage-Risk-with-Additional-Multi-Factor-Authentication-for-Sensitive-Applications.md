---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 79319f54ceb14195dffd56b5a4dfe1b17f048df9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407522"
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles




-   [Configurer l’environnement lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [Guide pas à pas : Gérer les risques avec des Multi-Factor Authentication supplémentaires pour les applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [Configurer des méthodes d’authentification supplémentaires pour AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>Dans ce guide
Ce guide fournit les informations suivantes :

-   [Mécanismes d’authentification dans AD FS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1) -Description des mécanismes d’authentification disponibles dans Services ADFS (AD FS) dans Windows Server 2012 R2

-   [Vue d’ensemble du scénario](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2) : description d’un scénario dans lequel vous utilisez Services ADFS (AD FS) pour activer l’authentification multifacteur (MFA) en fonction de l’appartenance à un groupe de l’utilisateur.

    > [!NOTE]
    > Dans AD FS de Windows Server 2012 R2, vous pouvez activer l’authentification multifacteur en fonction de l’emplacement réseau, de l’identité de l’appareil et de l’identité de l’utilisateur ou de l’appartenance au groupe.

    Pour obtenir des instructions pas à pas détaillées pour la configuration et la vérification de ce scénario, consultez [Walkthrough Guide : Gérer les risques avec des Multi-Factor Authentication supplémentaires pour](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)les applications sensibles.

## <a name="BKMK_1"></a>Concepts clés : mécanismes d’authentification dans AD FS

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>Avantages des mécanismes d’authentification dans AD FS
Services ADFS (AD FS) dans Windows Server 2012 R2 offre aux administrateurs informatiques un ensemble d’outils plus riches et plus flexibles pour l’authentification des utilisateurs qui souhaitent accéder aux ressources de l’entreprise. Elle offre aux administrateurs un contrôle flexible sur les méthodes d’authentification principales et supplémentaires, fournit une expérience de gestion complète pour la configuration des stratégies d’authentification (à la fois via l’interface utilisateur et Windows PowerShell) et améliore expérience des utilisateurs finaux qui accèdent aux applications et services sécurisés par AD FS. Voici quelques-uns des avantages de la sécurisation de votre application et de vos services avec AD FS dans Windows Server 2012 R2 :

-   Stratégie d’authentification globale : fonction de gestion centralisée, à partir de laquelle un administrateur informatique peut choisir les méthodes d’authentification utilisées pour authentifier les utilisateurs en fonction de l’emplacement réseau à partir duquel ils accèdent aux ressources protégées. Les administrateurs peuvent ainsi effectuer les opérations suivantes :

    -   Imposer l’utilisation de méthodes d’authentification plus sécurisées pour l’accès aux demandes provenant de l’extranet.

    -   Activer l’authentification de l’appareil pour l’authentification à deux facteurs transparente. Cela lie l’identité de l’utilisateur à l’appareil inscrit qui est utilisé pour accéder à la ressource, offrant ainsi une vérification de l’identité composée plus sécurisée avant l’accès aux ressources protégées.

        > [!NOTE]
        > Pour plus d’informations sur l’objet appareil, le service d’inscription d’appareils, le Workplace Join et l’appareil comme authentification à deux facteurs transparente et authentification unique, consultez [joindre à l’espace de travail à partir de n’importe quel appareil pour l’authentification unique et l’authentification de second facteur transparente dans l’entreprise Applications](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

    -   Définissez les conditions d’authentification multifacteur pour tous les accès extranet ou conditionnelles en fonction de l’identité de l’utilisateur, de l’emplacement réseau ou d’un appareil utilisé pour accéder aux ressources protégées.

-   Plus grande souplesse dans la configuration des stratégies d’authentification : vous pouvez configurer des stratégies d’authentification personnalisées pour des ressources sécurisées par AD FS avec différentes valeurs commerciales. Par exemple, vous pouvez exiger l’authentification multifacteur pour une application avec un fort impact commercial.

-   Simplicité d’utilisation : des outils de gestion simples et intuitifs tels que le composant logiciel enfichable MMC de gestion AD FS basée sur l’interface graphique utilisateur et les applets de commande Windows PowerShell permettent aux administrateurs informatiques de configurer des stratégies d’authentification avec une facilité relative. Avec Windows PowerShell, vous pouvez générer des scripts de vos solutions pour une utilisation dans les bonnes proportions et une automatisation des tâches d’administration courantes.

-   Contrôle accru sur les ressources de l’entreprise : étant donné que, en tant qu’administrateur, vous pouvez utiliser AD FS pour configurer une stratégie d’authentification qui s’applique à une ressource spécifique, vous avez davantage de contrôle sur la façon dont les ressources d’entreprise sont sécurisées. Les applications ne peuvent pas remplacer les stratégies d’authentification spécifiées par les administrateurs informatiques. Pour les services et applications sensibles, vous pouvez activer le critère d’authentification multifacteur, l’authentification de l’appareil et éventuellement une nouvelle authentification chaque fois que la ressource fait l’objet d’un accès.

-   Prise en charge des fournisseurs d’authentification multifacteur personnalisés : pour les organisations qui tirent parti des méthodes d’authentification multifacteur tierces, AD FS offre la possibilité d’incorporer et d’utiliser ces méthodes d’authentification en toute transparence.

### <a name="authentication-scope"></a>Étendue de l’authentification
Dans AD FS de Windows Server 2012 R2, vous pouvez spécifier une stratégie d’authentification au niveau d’une étendue globale applicable à l’ensemble des applications et services sécurisés par AD FS.  Vous pouvez également définir des stratégies d’authentification pour des applications et des services spécifiques (approbations de partie de confiance) qui sont sécurisés par AD FS. La spécification d’une stratégie d’authentification pour une application particulière (par approbation de partie de confiance) ne remplace pas la stratégie d’authentification globale. Si la stratégie d’authentification globale ou par approbation de partie de confiance exige l’authentification multifacteur, celle-ci est déclenchée lorsque l’utilisateur essaie de s’authentifier auprès de cette approbation de partie de confiance.  La stratégie d’authentification globale est une solution de secours pour les approbations de partie de confiance (applications et services) pour lesquelles aucune stratégie d’authentification spécifique n’est configurée.

Une stratégie d’authentification globale s’applique à toutes les parties de confiance qui sont sécurisées par AD FS. Vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification globale :

-   Méthodes d’authentification à utiliser pour l’authentification principale

-   Paramètres et méthodes pour l’authentification multifacteur

-   Activation ou non de l’authentification de l’appareil. Pour plus d’informations, consultez [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Les stratégies d’authentification par approbation de partie de confiance s’appliquent en particulier aux tentatives d’accès à cette approbation de partie de confiance (application ou service). Vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification par approbation de partie de confiance :

-   Nécessité ou non pour les utilisateurs de fournir leurs informations d’identification à chaque connexion

-   Paramètres d’authentification multifacteur selon l’utilisateur/le groupe, l’inscription de l’appareil et les données de l’emplacement de la demande d’accès

### <a name="primary-and-additional-authentication-methods"></a>Méthodes d’authentification principales et supplémentaires
Avec AD FS dans Windows Server 2012 R2, outre le mécanisme d’authentification principal, les administrateurs peuvent configurer des méthodes d’authentification supplémentaires. Les méthodes d’authentification principales sont intégrées et sont destinées à valider les identités des utilisateurs. Vous pouvez configurer des facteurs d’authentification supplémentaires pour demander que d’autres informations sur l’identité de l’utilisateur soient fournies et donc garantir une authentification renforcée.

Avec l’authentification principale dans AD FS dans Windows Server 2012 R2, vous disposez des options suivantes :

-   Pour les ressources publiées accessibles depuis l’extérieur du réseau d’entreprise, l’authentification par formulaires est sélectionnée par défaut. En outre, vous pouvez également activer l’authentification par certificat (autrement dit, une authentification basée sur une carte à puce ou une authentification par certificat client utilisateur qui fonctionne avec AD DS).

-   Pour les ressources intranet, l’authentification Windows est sélectionnée par défaut. En outre, vous pouvez également activer l’authentification par formulaires et/ou l’authentification par certificat.

En sélectionnant plusieurs méthodes d’authentification, vous permettez aux utilisateurs d’avoir le choix de la méthode pour s’authentifier au niveau de la page de connexion pour votre application ou service.

Vous pouvez également activer l’authentification de l’appareil pour l’authentification à deux facteurs transparente. Cela lie l’identité de l’utilisateur à l’appareil inscrit qui est utilisé pour accéder à la ressource, offrant ainsi une vérification de l’identité composée plus sécurisée avant l’accès aux ressources protégées.

> [!NOTE]
> Pour plus d’informations sur l’objet appareil, le service d’inscription d’appareils, le Workplace Join et l’appareil comme authentification à deux facteurs transparente et authentification unique, consultez [joindre à l’espace de travail à partir de n’importe quel appareil pour l’authentification unique et l’authentification de second facteur transparente dans l’entreprise Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Si vous spécifiez la méthode d’authentification Windows (option par défaut) pour vos ressources intranet, les demandes d’authentification subissent cette méthode en toute transparence sur des navigateurs qui prennent en charge l’authentification Windows.

> [!NOTE]
> L’authentification Windows n’est pas prise en charge sur tous les navigateurs. Le mécanisme d’authentification dans AD FS dans Windows Server 2012 R2 détecte l’agent utilisateur du navigateur de l’utilisateur et utilise un paramètre configurable pour déterminer si cet agent utilisateur prend en charge l’authentification Windows. Les administrateurs peuvent procéder à des ajouts dans cette liste d’agents utilisateurs (via la commande Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents`) afin d’indiquer d’autres chaînes de l’agent utilisateur pour les navigateurs qui prennent en charge l’authentification Windows. Si l’agent utilisateur du client ne prend pas en charge l’authentification Windows, la méthode de secours par défaut est l’authentification par formulaire.

### <a name="configuring-mfa"></a>Configuration de l’authentification multifacteur
Il existe deux parties pour configurer l’authentification multifacteur dans AD FS dans Windows Server 2012 R2 : spécification des conditions dans lesquelles l’authentification multifacteur est requise et sélection d’une méthode d’authentification supplémentaire. Pour plus d’informations sur les autres méthodes d’authentification, consultez [configurer des méthodes d’authentification supplémentaires pour AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).

**Paramètres MFA**

Les options suivantes sont disponibles pour les paramètres d’authentification multifacteur (conditions dans lesquelles l’authentification multifacteur doit être requise) :

-   Vous pouvez exiger l’authentification multifacteur pour des utilisateurs et groupes spécifiques dans le domaine Active Directory auquel votre serveur de fédération est rattaché.

-   Vous pouvez exiger l’authentification multifacteur pour des appareils inscrits (rattachés à un espace de travail) ou désinscrits (non rattachés à un espace de travail).

    Windows Server 2012 R2 adopte une approche centrée sur l’utilisateur sur les appareils modernes, où les objets appareils représentent une relation entre user@device et une société. Les objets d’appareil sont une nouvelle classe dans Active Directory dans Windows Server 2012 R2 qui peut être utilisée pour offrir une identité composée lors de l’accès aux applications et services. Un nouveau composant d’AD FS, DRS (Device Registration Service), configure une identité d’appareil dans Active Directory et définit un certificat sur l’appareil du consommateur qui sera utilisé pour représenter l’identité de l’appareil. Vous pouvez ensuite utiliser cette identité d’appareil pour rattacher votre appareil à l’espace de travail, autrement dit pour connecter votre appareil personnel à Active Directory sur votre espace de travail. Lorsque vous rattachez votre appareil personnel à votre espace de travail, il devient un appareil connu et fournit une authentification multifacteur transparente aux applications et ressources protégées. En d’autres termes, une fois qu’un appareil est joint à un espace de travail, l’identité de l’utilisateur est liée à cet appareil et peut être utilisée pour une vérification d’identité composée transparente avant l’accès à une ressource protégée.

    Pour plus d’informations sur la jonction d’espace de travail et la sortie, consultez [joindre à l’espace de travail à partir de n’importe quel appareil pour l’authentification unique et l’authentification de second facteur transparente entre les applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   Vous pouvez exiger l’authentification multifacteur lorsque la demande d’accès pour les ressources protégées provient de l’extranet ou de l’intranet.

## <a name="BKMK_2"></a>Vue d’ensemble du scénario
Dans ce scénario, vous activez l’authentification multifacteur en fonction des données d’appartenance au groupe de l’utilisateur pour une application spécifique. Autrement dit, vous allez configurer une stratégie d’authentification sur votre serveur de fédération pour exiger une authentification multifacteur lorsque des utilisateurs qui appartiennent à un certain groupe demandent l’accès à une application spécifique qui est hébergée sur un serveur Web.

Plus spécifiquement, dans ce scénario, vous activez une stratégie d’authentification pour une application de test basée sur des revendications appelée **claimapp**, où l’utilisateur Active Directory **Robert Hatley** devra subir une authentification multifacteur, car il appartient à un groupe Active Directory nommé **Finance**.

Les instructions pas à pas permettant de configurer et de vérifier ce scénario sont fournies dans @no__t Guide de 0Walkthrough : Gérer les risques avec des Multi-Factor Authentication supplémentaires pour](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)les applications sensibles. Pour effectuer les étapes de cette procédure pas à pas, vous devez configurer un environnement Lab et suivre les étapes décrites dans [configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Parmi les autres scénarios d’activation de l’authentification MFA dans AD FS figurent les suivants :

-   Activer l’authentification multifacteur si la demande d’accès provient de l’extranet. Vous pouvez modifier le code présenté dans la section « configurer la stratégie MFA » du Guide de [Walkthrough : Gérer les risques avec des Multi-Factor Authentication supplémentaires pour les applications sensibles @ no__t-0 avec les éléments suivants :

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   Activer l’authentification multifacteur si la demande d’accès provient d’un appareil non rattaché à un espace de travail.  Vous pouvez modifier le code présenté dans la section « configurer la stratégie MFA » du Guide de [Walkthrough : Gérer les risques avec des Multi-Factor Authentication supplémentaires pour les applications sensibles @ no__t-0 avec les éléments suivants :

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   Activer l’authentification multifacteur si la demande d’accès provient d’un utilisateur avec un appareil qui est rattaché à un espace de travail, mais non inscrit auprès de cet utilisateur. Vous pouvez modifier le code présenté dans la section « configurer la stratégie MFA » du Guide de [Walkthrough : Gérer les risques avec des Multi-Factor Authentication supplémentaires pour les applications sensibles @ no__t-0 avec les éléments suivants :

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>Voir aussi
[Guide pas à pas : Gérer les risques avec des Multi-Factor Authentication supplémentaires pour les applications sensibles @ no__t-0 @ no__t-1[configurer l’environnement Lab pour AD FS dans Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



