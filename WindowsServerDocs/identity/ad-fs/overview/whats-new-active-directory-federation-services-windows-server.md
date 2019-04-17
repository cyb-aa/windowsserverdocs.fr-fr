---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Quelles sont les nouveautés dans Active Directory Federation Services pour Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b8dd9174a869110d98ba1e65cbf2c2b6ec000b71
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Quelles sont les nouveautés dans Active Directory Federation Services pour Windows Server 2016

>S’applique à: Windows Server2016

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Quelles sont les nouveautés dans Active Directory Federation Services pour Windows Server 2016   
Si vous recherchez des informations sur les versions antérieures d’AD FS, voir les articles suivants:  
 [AD FS dans Windows Server 2012 ou 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) et [AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Active Directory Federation Services fournit le contrôle d’accès et d’authentification unique sur un large éventail d’applications, notamment Office 365, cloud basé SaaS et des applications sur le réseau d’entreprise.  
* Pour le service informatique, il vous permet de fournir l’authentification sur et contrôle d’accès aux applications modernes et héritées, localement et dans le cloud, basée sur le même jeu de stratégies et les informations d’identification.    
* Pour l’utilisateur, il fournit une authentification transparente en utilisant les informations d’identification de compte familière, même.  
* Pour les développeurs, il fournit un moyen simple pour authentifier les utilisateurs dont les identités live dans le répertoire d’organisation afin que vous puissiez concentrer vos efforts sur votre application, pas d’authentification ou d’identité.  

Cet article décrit les nouveautés dans AD FS dans Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Éliminer les mots de passe à partir de l’Extranet  
AD FS 2016 permet trois nouvelles options de session sans mot de passe, qui permet aux entreprises d’éviter le risque de réseau compromettre de récoltées, perdues ou le vols de mots de passe. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Se connecter avec l’authentification multifacteur Azure
AD FS 2016 s’appuie sur l’authentification multifacteur fonctionnalités (MFA) d’AD FS dans Windows Server 2012 R2 en permettant à l’aide d’uniquement un code d’authentification Multifacteur Azure, sans entrer d’abord dans un nom d’utilisateur et un mot de passe d’authentification.

* Avec l’authentification Multifacteur Azure comme méthode d’authentification principal, l’utilisateur est invité à entrer leur nom d’utilisateur et le code secret à usage unique à partir de l’application Azure Authenticator.  
* Avec l’authentification Multifacteur Azure comme méthode d’authentification secondaire ou supplémentaires, l’utilisateur fournit l’authentification principale (à l’aide de l’authentification Windows intégrée, nom d’utilisateur et mot de passe, carte à puce ou certificat utilisateur ou périphérique) des informations d’identification, puis voit une invite pour le texte, voix, ou à usage unique en fonction de connexion Azure MFA.  
* Avec la nouvelle carte Azure MFA intégrée, le programme d’installation et de configuration pour l’authentification Multifacteur Azure avec AD FS a jamais été plus simple.
* Les organisations peuvent tirer parti de l’authentification Multifacteur Azure sans avoir besoin d’un serveur local Azure MFA.
* Azure MFA peut être configuré pour l’intranet ou extranet ou dans le cadre d’une stratégie de contrôle d’accès.

Pour plus d’informations sur l’authentification Multifacteur Azure avec AD FS
*  [Configurer ADFS2016 et Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Accès sans mot de passe à partir d’appareils compatibles
AD FS 2016 repose sur les fonctionnalités de l’inscription des appareils précédente pour activer l’authentification et le contrôle d’accès en fonction de l’état de conformité d’appareil. Les utilisateurs peuvent se connecter à l’aide de l’information d’identification de l’appareil, et la conformité est réévaluée lors de modifier les attributs de l’appareil, afin que vous pouvez toujours vous assurer les stratégies sont appliquées.  Ainsi, les stratégies comme

* Activer l’accès uniquement à partir d’appareils sont gérés et/ou conforme  
* Activer l’accès Extranet uniquement à partir d’appareils sont gérés et/ou conforme  
* Exiger l’authentification multifacteur pour les ordinateurs qui ne sont pas gérés ou non conforme  

AD FS fournit le composant de site sur des stratégies d’accès conditionnel dans un scénario hybride. Lorsque vous inscrivez des périphériques avec Azure AD pour l’accès conditionnel aux ressources cloud, l’identité de l’appareil peut servir pour les stratégies d’AD FS ainsi.

![Ces nouvelles fonctionnalités](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Pour plus d’informations sur l’utilisation de périphériques en fonction des accès conditionnel dans le cloud   
 *  [Accès conditionnel à Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)

Pour plus d’informations sur l’utilisation de périphériques en fonction des accès conditionnel avec AD FS
*  [Planification de l’appareil en fonction de l’accès conditionnel avec AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Stratégies de contrôle d’accès dans ADFS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Se connecter avec Windows Hello entreprise   
Windows Hello et Windows Hello entreprise, en remplaçant les mots de passe utilisateur avec informations d’identification forte liés aux périphériques utilisateur protégées par un mouvement de l’utilisateur (un code confidentiel, un mouvement biométrique comme les empreintes digitales ou la reconnaissance faciale) introduisent des appareils Windows10. AD FS 2016 prend en charge ces nouveaux ces nouvelles fonctionnalités de Windows 10 afin que les utilisateurs peuvent se connecter aux applications AD FS à partir de l’intranet ou l’extranet sans avoir à fournir un mot de passe.

Pour plus d’informations sur l’utilisation de Microsoft Windows Hello entreprise dans votre organisation
*  [Activer Windows Hello entreprise dans votre organisation](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Sécuriser l’accès aux Applications

### <a name="modern-authentication"></a>Authentification moderne
AD FS 2016 prend en charge les derniers protocoles modernes qui fournissent une meilleure expérience utilisateur pour Windows 10 ainsi que les dernières iOS et les appareils Android et les applications.  

Pour plus d’informations, voir [scénarios AD FS pour les développeurs](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurer des stratégies de contrôle d’accès sans avoir à connaître langage des règles de revendication  
Auparavant, AD FS les administrateurs devaient configurer des stratégies à l’aide du langage de règle de revendication AD FS, rendant difficile à configurer et gérer des stratégies. Avec les stratégies de contrôle d’accès, les administrateurs peuvent utiliser des modèles intégrés pour appliquer des stratégies courantes telles que
* Autoriser l’accès intranet uniquement
* Autoriser tout le monde et exiger l’authentification Multifacteur à partir de l’Extranet
* Autoriser tout le monde et exiger l’authentification Multifacteur à partir d’un groupe spécifique

Les modèles sont faciles à personnaliser à l’aide d’un Assistant de processus pour ajouter des règles de stratégie supplémentaires ou des exceptions et peuvent être appliquées à un ou plusieurs applications pour appliquer la stratégie cohérente.

Pour plus d’informations, voir [stratégies de contrôle d’accès dans AD FS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Activer l’authentification avec les annuaires LDAP non - AD  
De nombreuses organisations possèdent une combinaison d’Active Directory et les répertoires de tiers. Avec la prise en charge AD FS pour authentifier les utilisateurs stockés dans des annuaires LDAP v3 conforme, AD FS peuvent désormais être utilisés pour:
* Utilisateurs de tiers, annuaires conformes au LDAP v3
* Utilisateurs dans des forêts Active Directory sur lesquels une approbation bidirectionnelle Active Directory n’est pas configurée
* Utilisateurs dans Active Directory Lightweight Directory Services (AD LDS)

Pour plus d’informations, voir [configurer AD FS pour authentifier les utilisateurs stockés dans les annuaires LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Une meilleure expérience de connexion
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personnaliser le processus de connexion pour les applications AD FS  
Nous avons entendu votre que la possibilité de personnaliser l’expérience d’ouverture de session pour chaque application serait une amélioration grande facilité d’utilisation, en particulier pour les organisations qui fournissent une connexion sur pour les applications qui représentent plusieurs sociétés différentes ou marques.  

Auparavant, AD FS dans Windows Server 2012 R2 fourni un signe courantes sur l’expérience pour toutes les applications de partie de confiance, avec la possibilité de personnaliser un sous-ensemble du texte basé contenu par application. Avec Windows Server 2016, vous pouvez personnaliser non seulement les messages, mais que les images, thème web et le logo par application. En outre, vous pouvez créer de nouveaux thèmes web personnalisés et appliquer ces par partie de confiance tiers.  

Pour plus d’informations, voir [AD FS sign-in personnalisation de l’utilisateur.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>Facilité de gestion et les améliorations  
La section suivante décrit les scénarios opérationnels améliorées qui sont introduits avec Active Directory Federation Services dans Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Rationalisation de l’audit pour faciliter la gestion d’administration  
Dans AD FS pour Windows Server 2012 R2 il ont été nombreux événements d’audit générés pour une seule demande et les informations pertinentes sur un journal dans ou l’activité d’émission de jeton est soit absent (dans certaines versions des services AD FS) ou réparti sur plusieurs événements d’audit. Par défaut, les services ADFS les événements d’audit sont désactivés en raison de leur nature verbose.  
Avec la version d’AD FS 2016, l’audit est devenue plus simple et moins détaillés.  

Pour plus d’informations, voir [l’audit des améliorations apportées à AD FS dans Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Amélioration de l’interopérabilité avec SAML 2.0 de participation aux confédérations de l’industrie  
AD FS 2016 contient SAML protocole prise en charge supplémentaire, y compris la prise en charge pour l’importation des approbations de fonction de métadonnées qui contient plusieurs entités. Cela vous permet de configurer ADFS pour participer confédérations de l’industrie telles que la fédération InCommon et d’autres implémentations conformes à la norme 2.0 eGov.  

Pour plus d’informations, voir [amélioré l’interopérabilité avec SAML 2.0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Gestion simplifiée du mot de passe pour fédéré utilisateurs Office 365  
Vous pouvez configurer Active Directory Federation Services (ADFS) pour envoyer des revendications d’expiration de mot de passe pour les approbations de confiance (applications) qui sont protégées par AD FS. Comment ces revendications sont utilisées dépendent de l’application. Par exemple, avec Office 365 comme votre partie de confiance, les mises à jour ont été implémentés à Exchange et Outlook pour informer les utilisateurs fédérés de leurs mots de passe dès expiré.  

Pour plus d’informations, voir [configurer AD FS pour envoyer des revendications d’expiration de mot de passe.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Il est plus facile de passer d’AD FS dans Windows Server 2012 R2 à AD FS dans Windows Server 2016  
Auparavant, la migration vers une nouvelle version des services AD FS requis exportation de la configuration à partir de l’ancienne batterie de serveurs et d’importation dans une batterie de serveurs entièrement nouveaux, parallèle.  

Désormais, le déplacement d’AD FS dans Windows Server 2012 R2 à AD FS dans Windows Server 2016 est devenu beaucoup plus facile. Ajoutez simplement un nouveau serveur Windows Server 2016 à une batterie de serveurs Windows Server 2012 R2, et la batterie de serveurs s’agir au niveau de comportement de la batterie de serveurs Windows Server 2012 R2, pour qu’il recherche se comporte comme une batterie de serveurs Windows Server 2012 R2.  

Ensuite, ajoutez les nouveaux serveurs Windows Server 2016 à la batterie de serveurs, vérifier les fonctionnalités et supprimer les anciens serveurs à partir de l’équilibrage de charge. Une fois que tous les nœuds de batterie de serveurs exécutant Windows Server 2016, vous êtes prêt à mettre à niveau le niveau de comportement de la batterie de serveurs à 2016 et commencer à utiliser les nouvelles fonctionnalités.  

Pour plus d’informations, voir [mise à niveau vers AD FS dans Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
