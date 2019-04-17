---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: "Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9ee275e6fe38005394cd071e9cfe9a7999350e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles

>S’applique à: Windows Server2012R2


-   [Configurer l’environnement lab pour ADFS dans Windows Server2012R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [Guide de procédure pas à pas: Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [Configurer des méthodes d’authentification supplémentaires pour ADFS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>Dans ce guide
Ce guide fournit les informations suivantes:

-   [Mécanismes d’authentification dans ADFS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1) -description des mécanismes d’authentification disponibles dans ActiveDirectory Federation Services (ADFS) dans Windows Server2012R2

-   [Vue d’ensemble du scénario](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2) -une description d’un scénario où vous utilisez ActiveDirectory Federation Services (ADFS) pour activer l’authentification multifacteur (MFA) basée sur l’appartenance au groupe de l’utilisateur.

    > [!NOTE]
    > Dans ADFS dans Windows Server2012R2, vous pouvez activer l’authentification Multifacteur selon l’emplacement réseau, identité de l’appareil et l’appartenance au groupe ou d’identité de l’utilisateur.

    Pour obtenir des instructions détaillées de procédure pas à pas pour la configuration et la vérification de ce scénario, voir [Guide pas à pas: gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

## <a name="BKMK_1"></a>Concepts clés: mécanismes d’authentification dans ADFS

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>Avantages des mécanismes d’authentification dans ADFS
ActiveDirectory Federation Services (ADFS) dans Windows Server2012R2 fournit aux administrateurs informatiques avec un ensemble d’outils plus riche et plus souple pour l’authentification des utilisateurs qui souhaitent accéder aux ressources d’entreprise. Il permet aux administrateurs un contrôle flexible sur les principales et les méthodes d’authentification supplémentaires, fournit une expérience de gestion complète pour la configuration de l’authentification stratégies (à la fois par le biais de l’interface utilisateur et de Windows PowerShell) et améliore l’expérience des utilisateurs finaux qui accèdent aux applications et services sécurisés par ADFS. Voici quelques-uns des avantages de la sécurisation de vos applications et services avec ADFS dans Windows Server2012R2:

-   Stratégie d’authentification globale: fonction de gestion centrale, dans lequel un administrateur peut choisir les méthodes d’authentification sont utilisées pour authentifier les utilisateurs en fonction de l’emplacement réseau à partir duquel ils accèdent aux ressources protégées. Cela permet aux administrateurs d’effectuer les opérations suivantes:

    -   Imposer l’utilisation de méthodes d’authentification plus sécurisés pour les demandes d’accès à partir de l’extranet.

    -   Activer l’authentification des appareils pour l’authentification à deux facteurs transparente. Cela lie l’identité l’utilisateur à l’appareil inscrit qui est utilisé pour accéder à la ressource, offre une vérification d’identité composée plus sécurisée avant l’accès aux ressources protégées.

        > [!NOTE]
        > Pour plus d’informations sur l’objet appareil, Device Registration Service, espace de travail et l’appareil comme authentification à deux facteurs transparente et authentification unique, voir [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur d’authentification entre les Applications d’entreprise](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

    -   Définir le critère d’authentification Multifacteur pour tous les accès extranet ou de manière conditionnelle basée sur l’identité de l’utilisateur, l’emplacement réseau ou un périphérique qui est utilisé pour accéder aux ressources protégées.

-   Une plus grande souplesse dans la configuration des stratégies d’authentification: vous pouvez configurer des stratégies d’authentification personnalisées pour obtenir des ressources ADFS sécurisée avec différentes valeurs commerciales. Par exemple, vous pouvez exiger l’authentification Multifacteur pour une application avec un fort impact commercial.

-   Facilité d’utilisation: des outils de gestion simples et intuitifs tels que le composant logiciel enfichable MMC ADFS Management Console basée sur une interface graphique utilisateur et les applets de commande Windows PowerShell permettent aux administrateurs informatiques configurer des stratégies d’authentification avec une relative simplicité. Avec Windows PowerShell, vous pouvez créer un script vos solutions pour une utilisation à l’échelle et automatiser les tâches d’administration courantes.

-   Un meilleur contrôle sur les ressources d’entreprise: dans la mesure où en tant qu’administrateur ADFS permet de configurer une stratégie d’authentification qui s’applique à une ressource spécifique, vous avez davantage de contrôle sur les ressources comment d’entreprise sont sécurisées. Les applications ne peuvent pas remplacer les stratégies d’authentification spécifiées par les administrateurs informatiques. Pour les services et applications sensibles, vous pouvez activer l’authentification Multifacteur exigence, l’authentification des appareils et éventuellement une nouvelle authentification chaque fois que la ressource est accessible.

-   Prise en charge des fournisseurs d’authentification Multifacteur personnalisés: pour les organisations qui bénéficient des méthodes d’authentification Multifacteur tierces, ADFS offrent la possibilité d’incorporer et d’utiliser ces méthodes d’authentification en toute transparence.

### <a name="authentication-scope"></a>Étendue de l’authentification
Dans ADFS dans Windows Server2012R2, vous pouvez spécifier une stratégie d’authentification à l’étendue globale qui s’applique à toutes les applications et services sécurisés par ADFS.  Vous pouvez également définir des stratégies d’authentification pour des applications spécifiques et des services (approbations de partie de confiance) qui sont sécurisés par ADFS. Spécification d’une stratégie d’authentification pour une application particulière (par partie de confiance confiance) ne remplace pas la stratégie d’authentification globale. Si globale ou par partie de confiance approbation de stratégie d’authentification exige l’authentification Multifacteur, l’authentification Multifacteur est déclenchée lorsque l’utilisateur essaie de s’authentifier auprès de cette partie de confiance.  La stratégie d’authentification globale est de secours pour les approbations de confiance (applications et services) qui n’ont pas d’une stratégie d’authentification spécifique.

Une stratégie d’authentification globale s’applique à toutes les parties de confiance qui sont sécurisés par ADFS. Vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification globale:

-   Méthodes d’authentification à utiliser pour l’authentification principale

-   Paramètres et méthodes pour l’authentification Multifacteur

-   Si l’authentification des appareils est activée. Pour plus d’informations, voir [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur d’authentification entre les Applications d’entreprise](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Stratégies d’authentification par partie de confiance partie confiance s’appliquent spécifiquement aux tentatives d’accès de cette partie de confiance (application ou service). Vous pouvez configurer les paramètres suivants dans le cadre de la stratégie d’authentification d’approbation tiers par partie de confiance:

-   Indique si les utilisateurs sont requis pour fournir leurs informations d’identification chaque fois à la connexion

-   Paramètres d’authentification Multifacteur basée sur le groupe d’utilisateurs, inscription de l’appareil et demande d’accès, les données de localisation

### <a name="primary-and-additional-authentication-methods"></a>Méthodes d’authentification principales et supplémentaires
Avec ADFS dans Windows Server2012R2, outre le mécanisme d’authentification principal, les administrateurs peuvent configurer des méthodes d’authentification supplémentaires. Méthodes d’authentification principales sont intégrées et destinées à valider les identités des utilisateurs. Vous pouvez configurer des facteurs d’authentification supplémentaires pour demander que plus d’informations sur l’identité de l’utilisateur est fourni et garantir par conséquent une authentification renforcée.

Avec l’authentification principale dans ADFS dans Windows Server2012R2, vous disposez des options suivantes:

-   Pour les ressources publiées accessibles depuis l’extérieur du réseau d’entreprise, l’authentification par formulaire est sélectionnée par défaut. En outre, vous pouvez également activer l’authentification par certificat (autrement dit, l’authentification par carte à puce ou authentification du certificat client utilisateur qui fonctionne avec les services ADDS).

-   Pour les ressources intranet, l’authentification Windows est sélectionnée par défaut. En outre vous pouvez activer également l’authentification par formulaires et/ou l’authentification par certificat.

En sélectionnant plusieurs méthodes d’authentification, vous permettez aux utilisateurs d’avoir le choix de la méthode pour s’authentifier avec à la page de connexion pour votre application ou service.

Vous pouvez également activer l’authentification des appareils pour l’authentification à deux facteurs transparente. Cela lie l’identité l’utilisateur à l’appareil inscrit qui est utilisé pour accéder à la ressource, offre une vérification d’identité composée plus sécurisée avant l’accès aux ressources protégées.

> [!NOTE]
> Pour plus d’informations sur l’objet appareil, Device Registration Service, espace de travail et l’appareil comme authentification à deux facteurs transparente et authentification unique, voir [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur d’authentification entre les Applications d’entreprise](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Si vous spécifiez la méthode d’authentification de Windows (option par défaut) pour vos ressources intranet, les demandes d’authentification subissent cette méthode en toute transparence sur les navigateurs qui prennent en charge l’authentification Windows.

> [!NOTE]
> L’authentification Windows n’est pas pris en charge sur tous les navigateurs. Le mécanisme d’authentification dans ADFS dans Windows Server2012R2 détecte l’agent utilisateur du navigateur de l’utilisateur et utilise un paramètre configurable pour déterminer si cet agent utilisateur prend en charge l’authentification Windows. Les administrateurs peuvent ajouter à cette liste d’agents utilisateurs (via Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents`commande, afin d’indiquer les chaînes de l’agent utilisateur pour les navigateurs qui prennent en charge l’authentification Windows. Si l’agent d’utilisateur du client ne prend pas en charge l’authentification Windows, la méthode de secours par défaut est l’authentification par formulaire.

### <a name="configuring-mfa"></a>La configuration de l’authentification Multifacteur
Configurer l’authentification Multifacteur dans ADFS dans Windows Server2012R2 se fait en deux parties: spécification des conditions dans lesquelles l’authentification Multifacteur est requise et sélection d’une méthode d’authentification supplémentaire. Pour plus d’informations sur les méthodes d’authentification supplémentaires, voir [configurer des méthodes d’authentification supplémentaires pour ADFS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).

**Paramètres d’authentification Multifacteur**

Les options suivantes sont disponibles pour les paramètres d’authentification Multifacteur (conditions dans lesquelles exiger l’authentification Multifacteur):

-   Vous pouvez exiger l’authentification Multifacteur pour des utilisateurs et groupes dans le domaine ActiveDirectory auquel votre serveur de fédération est joint.

-   Vous pouvez exiger l’authentification Multifacteur pour soit (espace de travail joint) ou annule l’enregistrement (pas espace de travail joint) des périphériques.

    Windows Server2012R2 adopte une approche centrée sur l’utilisateur où les objets appareils représentent une relation entre des appareils modernes user@deviceet une société. Les objets périphériques sont une nouvelle classe dans ActiveDirectory dans Windows Server2012R2 qui peut être utilisé pour offrir une identité composée lors de l’accès aux applications et services. Un nouveau composant d’ADFS - device registration service (DRS) - configure une identité de l’appareil dans ActiveDirectory et définit un certificat sur l’appareil du consommateur qui servira à représenter l’identité de l’appareil. Vous pouvez ensuite utiliser cet appareil identité de l’espace de travail joindre votre appareil, autrement dit, pour connecter votre appareil personnel à ActiveDirectory de votre espace de travail. Lorsque vous joignez votre appareil personnel à votre espace de travail, il devient un appareil connu et fournit une authentification multifacteur transparente aux ressources protégées et les applications. En d’autres termes, une fois l’appareil joint à un espace de travail, identité de l’utilisateur est liée à cet appareil et peut être utilisée pour une vérification d’identité composée transparente avant d’accéder à une ressource protégée.

    Pour plus d’informations sur la jonction et le retrait, voir [joindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur d’authentification entre les Applications d’entreprise](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

-   Vous pouvez exiger l’authentification Multifacteur lors de la demande d’accès pour les ressources protégées provient de l’extranet ou sur l’intranet.

## <a name="BKMK_2"></a>Vue d’ensemble du scénario
Dans ce scénario, vous activez l’authentification Multifacteur selon les données d’appartenance au groupe de l’utilisateur pour une application spécifique. En d’autres termes, vous allez configurer une stratégie d’authentification sur votre serveur de fédération pour exiger l’authentification Multifacteur lorsque des utilisateurs qui appartiennent à un certain groupe demandent l’accès à une application spécifique qui est hébergée sur un serveur web.

Plus spécifiquement, dans ce scénario, vous activez une stratégie d’authentification pour une application de test basée sur les revendications appelée **claimapp**, dans laquelle un utilisateur AD **Robert Hatley** devra subir une authentification Multifacteur car il appartient à un groupe AD **Finance**.

Les instructions étape par étape pour configurer et vérifier ce scénario sont fournies dans [Guide pas à pas: gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md). Afin d’effectuer les étapes décrites dans cette procédure pas à pas, vous devez configurer un environnement de laboratoire et suivez les étapes de [configurer l’environnement lab pour ADFS dans Windows Server2012R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Autres scénarios d’activation de l’authentification Multifacteur dans ADFS sont les suivantes:

-   Activer l’authentification Multifacteur, si la demande d’accès provient de l’extranet. Vous pouvez modifier le code présenté dans la section «Configurer la stratégie d’authentification Multifacteur» de [Guide pas à pas: gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) avec les éléments suivants:

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   Activer l’authentification Multifacteur, si la demande d’accès provient d’un appareil joint à un espace de travail non.  Vous pouvez modifier le code présenté dans la section «Configurer la stratégie d’authentification Multifacteur» de [Guide pas à pas: gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) avec les éléments suivants:

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   Activer l’authentification Multifacteur, si l’accès provient d’un utilisateur avec un appareil qui est l’espace de travail joints, mais non inscrit auprès de cet utilisateur. Vous pouvez modifier le code présenté dans la section «Configurer la stratégie d’authentification Multifacteur» de [Guide pas à pas: gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) avec les éléments suivants:

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>Voir aussi
[Guide pas à pas: Gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)<ph x="2">
</ph>[configurer l’environnement lab pour ADFS dans Windows Server2012R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



