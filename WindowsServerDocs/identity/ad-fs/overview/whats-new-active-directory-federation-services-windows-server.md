---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Nouveautés des services de fédération Active Directory (AD FS) pour Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/26/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: faa0590dc38921a56952aa54bf38243b6ff84d82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867710"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Nouveautés des services de fédération Active Directory (AD FS)


>S'applique à : Windows Server 2019, Windows Server 2016

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Quelles sont les nouveautés dans Active Directory Federation Services pour Windows Server 2019

### <a name="protected-logins"></a>Connexions d’accès protégées
Voici un bref résumé des mises à jour des connexions protégées disponibles dans AD FS 2019 :
- **Fournisseurs d’authentification externe en tant que principal** -les clients peuvent désormais utiliser 3e produits d’authentification tiers en tant que le premier facteur et expose pas les mots de passe en tant que le premier facteur. Dans les cas où un fournisseur d’authentification lication externe peut s’avérer à 2 facteurs, il peut réclamer MFA. 
- **Authentification de mot de passe comme authentification supplémentaire** -les clients ont une option entièrement prise en charge de boîte de réception à utiliser le mot de passe uniquement pour les facteurs supplémentaires après un mot de passe moins option est utilisée en tant que le premier facteur. Cela améliore l’expérience du client à partir de 2016 ADFS où les clients devaient télécharger une carte de github, qui est prise en charge est. 
- **Framework de Module enfichable contre les menaces** -clients peuvent désormais créer leur propres plug-in modules pour bloquer certains types de demandes au cours de l’étape de pré-authentification. Cela rend plus facile pour les clients à utiliser l’intelligence cloud telles que la protection d’identité pour bloquer les connexions pour les utilisateurs à risque ou les transactions à risque.
- **Améliorations de ESL** -améliore le correctif QFE ESL dans 2016 en ajoutant les fonctionnalités suivantes
    - Permet aux clients d’être en mode audit tout protégées par la fonctionnalité de verrouillage extranet « classiques » disponible à partir de 2012 R2 AD FS. Actuellement, les clients 2016 n’aurait aucune protection en mode audit. 
    - Permet le seuil de verrouillage indépendants pour les emplacements connus. Cela rend possible pour plusieurs instances d’applications en cours d’exécution avec un compte de service courants de mots de passe avec le moins d’impact. 

### <a name="additional-security-improvements"></a>Améliorations de sécurité supplémentaires
Les améliorations de sécurité supplémentaires suivantes sont disponibles dans AD FS 2019 :
- **À distance PSH à l’aide de la connexion de la carte à puce** : les clients peuvent désormais utiliser cartes à puce à distance se connecter à AD FS via PSH et utilisation que pour gérer tous les PSH les fonctions inclut des applets de commande PSH plusieurs nœuds.
- **Personnalisation de l’en-tête HTTP** -les clients peuvent maintenant personnaliser des en-têtes HTTP émis au cours des réponses d’ADFS. Cela inclut les en-têtes suivants
     - HSTS : Pour indiquer que les points de terminaison ADFS utilisable uniquement sur les points de terminaison HTTPS pour un navigateur compatible à appliquer
     - x-frame-options : Autorise les administrateurs d’ADFS autoriser les parties de confiance spécifiques incorporer des iFrames pour les pages de connexion interactive ADFS. Cela doit être utilisé avec précaution et uniquement sur les ordinateurs hôtes HTTPS. 
     - En-tête futures : En-têtes futures supplémentaires peuvent aussi être configurés. 

Pour plus d’informations, consultez [en-têtes de réponse de sécurité HTTP personnaliser avec AD FS 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>Fonctionnalités de la stratégie d’authentification
Les fonctionnalités suivantes de la stratégie d’authentification sont dans AD FS 2019 :
- **Spécifiez la méthode d’authentification pour l’authentification par le fournisseur de ressources supplémentaire** -les clients peuvent maintenant utiliser des règles pour déterminer le fournisseur d’authentification supplémentaires à appeler pour le fournisseur d’authentification supplémentaires de revendications. Cela est utile pour les cas d’utilisation 2
    - Les clients sont en cours de transition vers un autre à partir du fournisseur d’une authentification supplémentaire. Cette façon dès leur intégration des utilisateurs à un fournisseur d’authentification plus récente, ils peuvent utiliser les groupes de contrôle d’authentification supplémentaire fournisseur est appelé.
    - Les clients ont des besoins pour un fournisseur d’authentification supplémentaires spécifiques (par exemple, certificat) pour certaines applications. 
- **Restreindre l’authentification d’appareil TLS en fonction uniquement aux applications qui en ont besoin** -les clients peuvent désormais restreindre le client TLS en fonction des authentifications d’appareil sur uniquement l’accès conditionnel basé sur les applications effectuant des appareils. Cela empêche les invites inutiles pour l’authentification de l’appareil (ou les échecs si l’application cliente ne peut pas gérer) pour les applications qui ne nécessitent pas l’authentification TLS en fonction des appareils.
- **Prise en charge MFA fraîcheur** -AD FS prend désormais en charge la capacité à refaire 2e facteur d’informations d’identification basées sur l’actualisation de l’information d’identification facteur 2nd. Cela permet aux clients d’effectuer une transaction initiale avec 2 facteurs et uniquement une invite pour le facteur 2nd sur une base périodique. Cela est uniquement disponible pour les applications qui peuvent fournir un paramètre supplémentaire dans la demande et n’est pas un paramètre configurable dans ADFS. Ce paramètre est pris en charge par Azure AD lorsque « N’oubliez pas mon authentification Multifacteur pour les X jours » est configuré et que l’indicateur 'supportsMFA' est défini sur true sur les paramètres d’approbation de domaine fédéré dans Azure AD. 

### <a name="sign-in-sso-improvements"></a>Amélioration de la connexion SSO
Les améliorations de l’authentification unique de connexion suivantes ont été apportées dans AD FS 2019 :

- [Paginé l’expérience utilisateur avec le thème centré](../operations/AD-FS-paginated-sign-in.md) -ADFS a été déplacées à un flux de l’expérience utilisateur paginé qui permet à ADFS valider et fournir une expérience de connexion plus plus lisse. ADFS utilise désormais une interface utilisateur centrée (au lieu du côté droit de l’écran). Vous pouvez avoir besoin de nouvelles images de logo et d’arrière-plan pour se conformer à cette expérience. Cela reflète également les fonctionnalités offertes dans Azure AD.
- **Correctif de bogue : État persistant de l’authentification unique pour les appareils Windows 10 lors de la PRT auth** cela résout un problème où MFA pas persistance de l’état lors de l’utilisation de l’authentification PRT pour les appareils Windows 10. Le résultat du problème était que les utilisateurs finaux est invité à entrer 2e facteur d’informations d’identification (MFA) fréquemment. Le correctif rend également l’expérience cohérente lors de l’authentification de l’appareil est effectuée avec succès par le biais de client TLS et via le mécanisme PRT. 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Prise en charge pour la création d’applications line of business modernes
La prise en charge suivant pour la création d’applications métier modernes a été ajouté à AD FS 2019 :

 - **Flux OAuth appareil/profil** -AD FS prend désormais en charge le profil de flux OAuth appareil pour effectuer des connexions sur les appareils qui n’ont pas d’une interface utilisateur surface d’exposition pour prendre en charge des expériences de connexion riches. Cela permet à l’utilisateur terminer l’expérience de connexion sur un autre appareil. Cette fonctionnalité est requise pour une expérience d’Azure CLI dans Azure Stack et peut être utilisée dans d’autres cas. 
 - **Suppression du paramètre de 'Resource'** -AD FS a supprimé la nécessité de spécifier un paramètre de ressource qui est conforme aux spécifications Oauth en cours. Les clients peuvent désormais fournir l’identificateur d’approbation de partie de confiance en tant que le paramètre d’étendue de plus pour les autorisations demandées. 
 - **Les en-têtes CORS dans les réponses d’AD FS** -clients peuvent désormais créer des Applications à Page unique qui permettent à client bibliothèques JS valider la signature du jeton id_token en recherchant les clés de signature à partir du document de découverte OIDC sur AD FS. 
 - **Prise en charge PKCE** -AD FS ajoute la prise en charge PKCE pour fournir un flux de code d’authentification sécurisée dans OAuth. Cela ajoute une couche supplémentaire de sécurité à ce flux pour empêcher le code de piratage et de le relire à partir d’un autre client. 
 - **Correctif de bogue : Envoi de revendication x5t et kid** -il s’agit d’un bogue mineur. AD FS envoie désormais en outre la revendication « kid » pour désigner l’indicateur de l’id de clé pour vérifier la signature. Déjà AD FS uniquement envoyés cela comme revendication « x5t ».

### <a name="supportability-improvements"></a>Améliorations de la prise en charge
Les améliorations suivantes de la prise en charge ne font pas partie d’AD FS 2019 :
- **Envoyer les détails de l’erreur pour les administrateurs d’AD FS** -permet aux administrateurs de configurer les utilisateurs finaux pour envoyer des journaux de débogage relatives à un échec de l’authentification utilisateur final à stocker comme archivé un compressé pour faciliter leur utilisation. Administrateurs peuvent également configurer une connexion SMTP à automail le fichier compressé à un compte de messagerie de triage ou automatiquement créer un ticket basé sur le courrier électronique. 

### <a name="deployment-updates"></a>Déploiement des mises à jour
Les mises à jour de déploiement suivantes sont désormais incluses dans AD FS 2019 :
- **2019 de niveau de comportement de batterie de serveurs** : comme avec AD FS 2016, il existe une nouvelle version de niveau de comportement de batterie de serveurs est nécessaire pour activer les nouvelles fonctionnalités abordées ci-dessus. Cela permet la transition de :
    - 2012 R2-> 2019
    - 2016 -> 2019   

### <a name="saml-updates"></a>Mises à jour SAML
La mise à jour suivante SAML est dans AD FS 2019 :
- **Correctif de bogue : Corriger les bogues dans la fédération agrégée** -prise en charge de la fédération agrégées des nombreux correctifs de bogues ont été (par exemple, InCommon). Les correctifs ont été autour de ce qui suit : 
  - Amélioration de mise à l’échelle pour un grand nombre d’entités dans le document de métadonnées de fédération agrégées. Auparavant, ce code échouerait avec une erreur de « ADMIN0017 ». 
  - Requête à l’aide du paramètre « ScopeGroupID » via l’applet de commande Get-AdfsRelyingPartyTrustsGroup PSH. 
  - Gestion des conditions d’erreur autour entityID en double


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Spécification de ressource Azure AD style dans le paramètre d’étendue 
AD FS nécessitaient auparavant, la ressource souhaitée et la portée se trouver dans un paramètre distinct dans toute demande d’authentification. Par exemple, une demande oauth typique ressemblerait à ci-dessous : 7 **https :&#47;&#47;fs.contoso.com/adfs/oauth2/authorize ?</br> response_type = code & client_id = claimsxrayclient & ressource = urn : microsoft :</br>adfs:claimsxray & scope = oauth & redirect_uri = https :&#47;&#47;adfshelp.microsoft.com/</br> ClaimsXray / TokenResponse & prompt = login**
 
Avec AD FS sur 2019 de serveur, vous pouvez maintenant passer la valeur de la ressource incorporée dans le paramètre d’étendue. Cela est cohérent avec comment vous pouvez également effectuer l’authentification auprès d’Azure AD. 

Le paramètre d’étendue peut maintenant être organisé comme une liste séparée par des espaces où chaque entrée a une structure en tant que portée de la ressource. Exemple :  

**< créer une demande d’exemple valide >**
> [!NOTE]
> Une seule ressource peut être spécifiée dans la demande d’authentification. Si plus d’une ressource est incluse dans la demande, AD FS retournera qu'une erreur et l’authentification ne réussiront pas. 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Clé de vérification pour la prise en charge de Code Exchange (PKCE) pour oAuth 
Les clients publics OAuth à l’aide de l’octroi de Code d’autorisation sont vulnérables à l’attaque de l’interception de code d’autorisation.  L’attaque est également décrite dans RFC 7636. Pour atténuer ce type d’attaque, AD FS dans Server 2019 prend en charge clé de preuve pour le Code Exchange (PKCE) pour les flux d’octroi de Code d’autorisation OAuth. 
 
Pour tirer parti de la prise en charge PKCE, cette spécification ajoute des paramètres supplémentaires pour les demandes de jeton d’accès OAuth 2.0 Authorization.

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. Le client crée et enregistre un secret nommé le « code_verifier » et est dérivée d’une version transformée « t(code_verifier) » (également appelé le « au code_challenge »), qui est envoyée dans la demande d’autorisation de 2.0 de OAuth, ainsi que la méthode de transformation « t_m ». 

B. Le point de terminaison d’autorisation répond comme d’habitude, mais les enregistrements « t(code_verifier) » et la méthode de transformation. 

C. Ensuite, le client envoie le code d’autorisation dans l’accès demande de jeton, comme d’habitude mais inclut le secret « code_verifier » généré à (A). 

D. Les services AD FS transforme « code_verifier » et le compare à « t(code_verifier) » à partir de (B).  L’accès est refusé s’ils ne sont pas égaux. 

#### <a name="faq"></a>Forum Aux Questions 
**Q.** Puis-je passer la valeur de ressource dans le cadre de la valeur d’étendue, comme la façon dont les demandes sont effectuées auprès d’Azure AD ? 
</br>**A.** Avec AD FS sur 2019 de serveur, vous pouvez maintenant passer la valeur de la ressource incorporée dans le paramètre d’étendue. Le paramètre d’étendue peut maintenant être organisé comme une liste séparée par des espaces où chaque entrée a une structure en tant que portée de la ressource. Exemple :  
**< créer une demande d’exemple valide >**

**Q.** AD FS prend-elle en charge l’extension PKCE ?
</br>**A.** AD FS dans Server 2019 prend en charge la clé de preuve pour Code Exchange (PKCE) pour les flux d’octroi de Code d’autorisation OAuth 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Nouveautés des services de fédération Active Directory (AD FS) pour Windows Server 2016   
Si vous recherchez plus d’informations sur les versions antérieures d’AD FS, consultez les articles suivants :  
 [AD FS dans Windows Server 2012 ou 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) et [AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Active Directory Federation Services fournit le contrôle d’accès et l’authentification unique dans une vaste gamme d’applications, notamment Office 365, cloud basée sur les applications SaaS et applications sur le réseau d’entreprise.  
* Pour l’organisation informatique, il vous permet de fournir authentification et contrôle d’accès aux applications modernes et héritées, en local et dans le cloud, basé sur le même ensemble de stratégies et les informations d’identification.    
* Pour l’utilisateur, il fournit une authentification transparente à l’aide des informations d’identification de compte familière, même.  
* Pour les développeurs, il fournit un moyen simple pour authentifier les utilisateurs dont les identités résident dans le répertoire d’organisation afin que vous pouvez concentrer vos efforts sur votre application, pas d’authentification ou d’identité.  

Cet article décrit quelles sont les nouveautés dans AD FS dans Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Éliminer les mots de passe à partir de l’Extranet  
AD FS 2016 permet trois nouvelles options pour l’authentification sans mot de passe, qui permet aux organisations éviter le risque de réseau compromettent de piratage, volées ou le volés de mots de passe. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Connectez-vous à Azure multi-factor Authentication
AD FS 2016 s’appuie sur l’authentification multifacteur fonctionnalités (MFA) d’AD FS dans Windows Server 2012 R2 en autorisant l’authentification à l’aide d’un code d’authentification Multifacteur Azure uniquement, sans entrer d’abord dans un nom d’utilisateur et le mot de passe.

* Avec Azure MFA en tant que la méthode d’authentification principale, l’utilisateur est invité à entrer son nom d’utilisateur et le code secret à usage unique à partir de l’application Azure Authenticator.  
* Avec Azure MFA comme méthode d’authentification secondaires ou supplémentaires, l’utilisateur fournit des informations d’identification (à l’aide de l’authentification intégrée de Windows, nom d’utilisateur et mot de passe, carte à puce ou certificat utilisateur ou périphérique) de l’authentification principale, puis voit une invite pour le texte, connexion d’Azure MFA basée sur un voix ou secret à usage unique.  
* Avec le nouvel adaptateur Azure MFA intégré, le programme d’installation et configuration pour Azure MFA avec AD FS a jamais été plus simple.
* Les organisations peuvent tirer parti de l’authentification Multifacteur Azure sans recourir à un serveur Azure MFA local.
* Azure MFA peut être configuré pour l’intranet ou extranet ou dans le cadre d’une stratégie de contrôle d’accès.

Pour plus d’informations sur Azure MFA avec AD FS
*  [Configurer AD FS 2016 et Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Accès sans mot de passe à partir d’appareils conformes
AD FS 2016 s’appuie sur les fonctionnalités d’inscription précédente pour activer l’authentification et contrôle d’accès en fonction de l’état de conformité d’appareil. Les utilisateurs peuvent se connecter à l’aide de l’information d’identification de l’appareil et ré-évaluation de la conformité lors de la modification d’attributs de l’appareil, afin que vous pouvez toujours vous assurer les stratégies sont appliquées.  Cela permet des stratégies telles que

* Activer l’accès uniquement à partir d’appareils qui sont conformes et/ou non managées  
* Activer l’accès Extranet uniquement à partir d’appareils qui sont conformes et/ou non managées  
* Exiger une authentification multifacteur pour les ordinateurs qui ne sont pas gérés ou non conformes  

AD FS fournit le composant de site sur des stratégies d’accès conditionnel dans un scénario hybride. Lorsque vous inscrivez des appareils avec Azure AD pour l’accès conditionnel aux ressources du cloud, l’identité d’appareil peut être utilisée pour les stratégies AD FS, ainsi.

![Ces nouvelles fonctionnalités](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Pour plus d’informations sur l’utilisation de périphériques en fonction des accès conditionnel dans le cloud   
 *  [Accès conditionnel Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

Pour plus d’informations sur l’utilisation de périphériques en fonction d’accès conditionnel avec AD FS
*  [Planification de l’appareil en fonction de l’accès conditionnel avec AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Stratégies de contrôle d’accès dans AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Connectez-vous à Windows Hello for Business   
Appareils Windows 10 introduisent Windows Hello et Windows Hello entreprise, en remplaçant les mots de passe utilisateur avec informations d’identification forts liée à l’appareil utilisateur protégées par un mouvement d’un utilisateur (un code confidentiel, un mouvement biométrique comme empreinte digitale ou reconnaissance faciale). AD FS 2016 prend en charge ces nouvelles fonctionnalités de Windows 10 afin que les utilisateurs peuvent se connecter aux applications d’AD FS à partir de l’intranet ou l’extranet sans la nécessité de fournir un mot de passe.

Pour plus d’informations sur l’utilisation de Microsoft Windows Hello entreprise dans votre organisation
*  [Activer Windows Hello entreprise dans votre organisation](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Accès sécurisé aux Applications

### <a name="modern-authentication"></a>Authentification moderne
AD FS 2016 prend en charge des protocoles modernes les plus récents qui fournissent une meilleure expérience utilisateur pour Windows 10 ainsi que l’iOS le plus récent et les appareils Android et les applications.  

Pour plus d’informations, consultez [scénarios AD FS pour les développeurs](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurer des stratégies de contrôle d’accès sans avoir à connaître le langage de règles de revendication  
Auparavant, les administrateurs AD FS devaient configurer des stratégies à l’aide du langage de règle de revendication AD FS, rend difficile à configurer et gérer des stratégies. Avec les stratégies de contrôle d’accès, les administrateurs peuvent utiliser des modèles intégrés pour appliquer des stratégies courantes telles que
* Autoriser l’accès intranet uniquement
* Autoriser tout le monde et demander l’authentification MFA à partir de l’Extranet
* Autoriser tout le monde et demander l’authentification Multifacteur à partir d’un groupe spécifique

Les modèles sont faciles à personnaliser à l’aide d’un Assistant piloté par les processus pour ajouter des exceptions ou des règles de stratégies supplémentaires et peuvent être appliqués à une ou plusieurs applications pour appliquer la stratégie cohérente.

Pour plus d’informations, consultez [stratégies de contrôle d’accès dans AD FS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Activer l’authentification avec les annuaires LDAP non - AD  
De nombreuses organisations possèdent une combinaison d’Active Directory et les répertoires par des tiers. Avec l’ajout de prise en charge AD FS pour authentifier les utilisateurs stockés dans des annuaires LDAP v3 conforme, AD FS est désormais utilisable pour :
* Utilisateurs de tiers, les annuaires conformes LDAP v3
* Utilisateurs dans les forêts Active Directory à laquelle une approbation bidirectionnelle Active Directory n’est pas configurée
* Utilisateurs dans Active Directory Lightweight Directory Services (AD LDS)

Pour plus d’informations, consultez [configurer AD FS pour authentifier les utilisateurs stockés dans les annuaires LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Une meilleure expérience de connexion
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personnaliser l’expérience pour les applications AD FS de connexion  
Nous avons entendu dire vous que la possibilité de personnaliser l’expérience d’ouverture de session pour chaque application serait une amélioration de la grande facilité d’utilisation, en particulier pour les organisations qui fournir l’authentification pour les applications qui représentent plusieurs entreprises différentes ou marques.  

Auparavant, AD FS dans Windows Server 2012 R2 fourni un signe courantes sur l’expérience pour toutes les applications de confiance en avec la possibilité de personnaliser un sous-ensemble du texte contenu par application. Avec Windows Server 2016, vous pouvez personnaliser non seulement les messages, mais les images, mais thème web et le logo par application. En outre, vous pouvez créer de nouveaux thèmes web personnalisés et appliquer ces par partie de confiance tiers.  

Pour plus d’informations, consultez [AD FS sign-in personnalisation de l’utilisateur.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>La facilité de gestion et les améliorations  
La section suivante décrit les scénarios opérationnels améliorées qui sont introduites avec les Services de fédération Active Directory dans Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Rationalisation de l’audit pour simplifier la gestion administrative  
Dans AD FS pour Windows Server 2012 R2 il ont de nombreux événements d’audit générés pour une seule requête et les informations pertinentes sur un journal dans ou les activités d’émission de jeton soient absent (dans certaines versions d’AD FS) ou répartie sur plusieurs événements d’audit. Par défaut, les services AD FS, les événements d’audit sont désactivées en raison de leur nature détaillée.  
Avec la version d’AD FS 2016, l’audit est devenue plus simple et moins détaillé.  

Pour plus d’informations, consultez [l’audit des améliorations apportées à AD FS dans Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Amélioration de l’interopérabilité avec SAML 2.0 pour participer à confédérations de l’industrie  
AD FS 2016 contient SAML protocole prise en charge supplémentaire, notamment la prise en charge pour l’importation des approbations en fonction des métadonnées qui contient plusieurs entités. Cela vous permet de configurer AD FS pour participer confédérations de l’industrie telles que la fédération InCommon et d’autres implémentations conformes à l’eGov 2.0 standard.  

Pour plus d’informations, consultez [amélioré l’interopérabilité avec SAML 2.0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Gestion simplifiée du mot de passe pour fédérés les utilisateurs O365  
Vous pouvez configurer Active Directory Federation Services (ADFS) pour envoyer des revendications d’expiration de mot de passe à la confiance (applications) qui est protégés par AD FS. Comment ces revendications sont utilisées dépendent de l’application. Par exemple, avec Office 365 en tant que votre partie de confiance, les mises à jour ont été implémentées pour Exchange et Outlook pour informer les utilisateurs fédérés de leurs mots de passe bientôt expirer.  

Pour plus d’informations, consultez [configurer AD FS pour envoyer des revendications d’expiration de mot de passe.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Migration d’AD FS dans Windows Server 2012 R2 vers AD FS dans Windows Server 2016 est facile  
Auparavant, la migration vers une nouvelle version des services AD FS devait exportation de la configuration à partir de l’ancienne batterie de serveurs et de l’importation vers une toute nouvelle batterie de serveurs parallèle.  

À présent, le déplacement à partir d’AD FS sur Windows Server 2012 R2 vers AD FS sur Windows Server 2016 est devenu beaucoup plus facile. Ajoutez simplement un nouveau serveur Windows Server 2016 à une batterie de serveurs Windows Server 2012 R2, et la batterie de serveurs agira au niveau de comportement de batterie de serveurs Windows Server 2012 R2, afin qu’il se présente et se comporte comme une batterie de serveurs Windows Server 2012 R2.  

Ensuite, ajoutez les nouveaux serveurs Windows Server 2016 à la batterie de serveurs, vérifier la fonctionnalité et supprimer les anciens serveurs d’équilibrage de charge. Une fois que tous les nœuds de batterie de serveurs exécutent Windows Server 2016, vous êtes prêt à mettre à niveau le niveau de comportement de batterie de serveurs vers 2016 et commencer à utiliser les nouvelles fonctionnalités.  

Pour plus d’informations, consultez [la mise à niveau vers AD FS dans Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
