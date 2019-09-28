---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Nouveautés des services de fédération Active Directory (AD FS) pour Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/23/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6294c7b6ead0a9fa338f8b2cc8134b750f7e3e8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385548"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Nouveautés des services de fédération Active Directory (AD FS)


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Nouveautés de Services ADFS pour Windows Server 2019

### <a name="protected-logins"></a>Connexions protégées
Voici un bref résumé des mises à jour des connexions protégées disponibles dans AD FS 2019 :
- **Fournisseurs d’authentification externes en tant que clients principaux** : les clients peuvent désormais utiliser les produits d’authentification tiers comme premier facteur et ne pas exposer les mots de passe comme premier facteur. Dans les cas où un fournisseur d’authentification externe peut prouver 2 facteurs, il peut revendiquer l’authentification MFA. 
- **Authentification par mot de passe en tant qu’authentification supplémentaire** : les clients disposent d’une option de boîte de réception entièrement prise en charge pour utiliser le mot de passe uniquement pour le facteur supplémentaire après l’utilisation d’une option de mot de passe pour le premier facteur. Cela améliore l’expérience client d’ADFS 2016, où les clients devaient télécharger un adaptateur GitHub qui est pris en charge comme c’est le cas. 
- **Module d’évaluation des risques enfichables** : les clients peuvent désormais créer leurs propres modules plug-in pour bloquer certains types de demandes lors de la phase de pré-authentification. Cela permet aux clients d’utiliser plus facilement l’intelligence du Cloud, par exemple Identity Protection, pour bloquer les connexions pour les utilisateurs à risque ou les transactions risquées.  Pour plus d’informations, consultez [créer des plug-ins avec le modèle d’évaluation des risques AD FS 2019](../../ad-fs/development/ad-fs-risk-assessment-model.md) 
- **Améliorations de ESL** : améliore le QFE ESL dans 2016 en ajoutant les fonctionnalités suivantes
    - Permet aux clients d’être en mode audit tout en étant protégé par la fonctionnalité de verrouillage extranet « classique » disponible depuis la 2012 R2 ADFS. Actuellement, 2016 clients n’auraient pas de protection en mode audit. 
    - Active le seuil de verrouillage indépendant pour les emplacements familiers. Cela permet à plusieurs instances d’applications qui s’exécutent avec un compte de service commun de restaurer les mots de passe avec le moins d’impact possible. 

### <a name="additional-security-improvements"></a>Améliorations de sécurité supplémentaires
Les améliorations de sécurité supplémentaires suivantes sont disponibles dans AD FS 2019 :
- **PSH à distance à l’aide** de la connexion par carte à puce : les clients peuvent désormais utiliser des cartes à puce pour se connecter à AD FS via PSH et l’utiliser pour gérer toutes les fonctions PSH incluent des applets de commande PSH à nœuds multiples.
- **Personnalisation d’en-tête http** : les clients peuvent désormais personnaliser les en-têtes http émis pendant les réponses ADFS. Cela comprend les en-têtes suivants
     - HSTS Cela indique que les points de terminaison ADFS ne peuvent être utilisés que sur des points de terminaison HTTPs pour qu’un navigateur conforme applique
     - x-Frame-options : Permet aux administrateurs ADFS d’autoriser des parties de confiance spécifiques à incorporer des iFrames pour les pages de connexion interactive ADFS. Elle doit être utilisée avec précaution et uniquement sur les hôtes HTTPs. 
     - En-tête futur : Des en-têtes futurs supplémentaires peuvent également être configurés. 

Pour plus d’informations, consultez [personnaliser les en-têtes de réponse de sécurité http avec AD FS 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>Fonctionnalités d’authentification/de stratégie
Les fonctionnalités d’authentification/de stratégie suivantes se trouvent dans AD FS 2019 :
- **Spécifier la méthode d’authentification pour une authentification supplémentaire par RP** -les clients peuvent maintenant utiliser des règles de revendication pour déterminer le fournisseur d’authentification supplémentaire à appeler pour le fournisseur d’authentification supplémentaire. Cela est utile pour deux cas d’utilisation
    - Les clients passent d’un fournisseur d’authentification supplémentaire à un autre. De cette façon, lorsqu’ils intègrent des utilisateurs à un fournisseur d’authentification plus récent, ils peuvent utiliser des groupes pour contrôler le fournisseur d’authentification supplémentaire appelé.
    - Les clients ont besoin d’un fournisseur d’authentification supplémentaire spécifique (par exemple, un certificat) pour certaines applications. 
- **Limiter l’authentification des appareils basée sur TLS uniquement aux applications qui en ont besoin** : les clients peuvent désormais restreindre les authentifications des appareils basés sur TLS client aux seules applications qui effectuent un accès conditionnel basé sur les appareils. Cela empêche les invites indésirables pour l’authentification des appareils (ou les échecs si l’application cliente ne peut pas gérer) pour les applications qui ne nécessitent pas l’authentification des appareils basée sur TLS.
- **Prise en charge de l’actualisation MFA** : AD FS prend désormais en charge la possibilité de réexécuter les informations d’identification du 2e facteur en fonction de l’actualisation des informations d’identification du 1er facteur. Cela permet aux clients d’effectuer une transaction initiale avec 2 facteurs et de demander le second facteur sur une base périodique. Cela est uniquement disponible pour les applications qui peuvent fournir un paramètre supplémentaire dans la demande et qui n’est pas un paramètre configurable dans ADFS. Ce paramètre est pris en charge par Azure AD lorsque l’option « Mémoriser mon MFA pour X jours » est configurée et que l’indicateur « supportsMFA » a la valeur true dans les paramètres d’approbation de domaine fédéré dans Azure AD. 

### <a name="sign-in-sso-improvements"></a>Améliorations de l’authentification unique pour la connexion
Les améliorations de l’authentification unique de connexion suivantes ont été apportées dans AD FS 2019 :

- Expérience utilisateur [paginée avec thème centré](../operations/AD-FS-paginated-sign-in.md) -ADFS maintenant a été déplacé vers un processus d’expérience utilisateur paginé qui permet à ADFS de valider et de fournir une expérience de connexion plus lisse. ADFS utilise désormais une interface utilisateur centrée (au lieu du côté droit de l’écran). Vous pouvez avoir besoin d’images de logo et d’arrière-plan plus récentes pour l’adapter à cette expérience. Cela reflète également les fonctionnalités proposées dans Azure AD.
- correctif de la @no__t 0Bug : État de l’authentification unique permanente pour les appareils Win10 lors de l’authentification PRT @ no__t-0. cela résout un problème où l’État MFA n’a pas été conservé lors de l’utilisation de l’authentification PRT pour les appareils Windows 10. Le problème est dû au fait que les utilisateurs finaux sont souvent invités à entrer des informations d’identification de 2e facteur (MFA). Le correctif rend également l’expérience cohérente lorsque l’authentification de l’appareil est effectuée avec succès via le protocole TLS du client et via le mécanisme PRT. 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Prise en charge pour la création d’applications métier modernes
La prise en charge suivante de la création d’applications métier modernes a été ajoutée à AD FS 2019 :

 - **Flow/profil d’appareil OAuth** : AD FS prend désormais en charge le profil de workflow d’appareil OAuth pour effectuer des connexions sur des appareils qui n’ont pas de surface d’interface utilisateur pour prendre en charge les expériences de connexion enrichies. Cela permet à l’utilisateur d’effectuer l’expérience de connexion sur un autre appareil. Cette fonctionnalité est requise pour l’expérience Azure CLI dans Azure Stack et peut être utilisée dans d’autres cas. 
 - La **suppression du paramètre « Resource »** -AD FS a maintenant supprimé la nécessité de spécifier un paramètre de ressource qui est conforme aux spécifications OAuth actuelles. Les clients peuvent désormais fournir l’identificateur d’approbation de la partie de confiance en tant que paramètre d’étendue en plus des autorisations demandées. 
 - **En-têtes cors dans les réponses de AD FS** : les clients peuvent désormais créer des applications à page unique qui permettent aux bibliothèques js côté client de valider la signature du id_token en interrogeant les clés de signature à partir du document de découverte OIDC sur AD FS. 
 - **Prise en charge PKCE** : AD FS ajoute la prise en charge de PKCE pour fournir un code d’authentification sécurisé dans OAuth. Cela ajoute une couche supplémentaire de sécurité à ce Flow pour empêcher le détournement du code et sa relecture à partir d’un autre client. 
 - correctif de la @no__t 0Bug : Send x5t et Kid claim @ no__t-0-il s’agit d’un correctif de bogue mineur. AD FS à présent envoie en plus la revendication « Kid » pour désigner l’indicateur d’ID de clé pour la vérification de la signature. Auparavant AD FS envoyée uniquement en tant que revendication « x5t ».

### <a name="supportability-improvements"></a>Améliorations de la prise en charge
Les améliorations de prise en charge suivantes ne font pas partie de AD FS 2019 :
- **Envoyer les détails de l’erreur aux administrateurs de AD FS** : permet aux administrateurs de configurer les utilisateurs finaux pour qu’ils envoient des journaux de débogage relatifs à un échec de l’authentification de l’utilisateur final afin qu’ils soient stockés en tant que fichiers Zippés pour une consommation facile Les administrateurs peuvent également configurer une connexion SMTP pour envoyer automatiquement le fichier zippé à un compte de messagerie de triage ou pour créer automatiquement un ticket basé sur l’e-mail. 

### <a name="deployment-updates"></a>Mises à jour du déploiement
Les mises à jour de déploiement suivantes sont maintenant incluses dans AD FS 2019 :
- **Niveau de comportement de la batterie de serveurs 2019** -comme avec AD FS 2016, une nouvelle version du niveau de comportement de la batterie de serveurs est nécessaire pour activer les nouvelles fonctionnalités décrites ci-dessus. Cela permet de passer par :
    - 2012 R2-> 2019
    - 2016-> 2019   

### <a name="saml-updates"></a>Mises à jour SAML
La mise à jour SAML suivante se trouve dans AD FS 2019 :
- correctif de la @no__t 0Bug : Correction des bogues dans la Fédération agrégée @ no__t-0-de nombreux correctifs de bogues ont été détectés autour de la prise en charge de la Fédération agrégée (par exemple, inhabituel). Les correctifs ont été mis en rapport avec les éléments suivants : 
  - Amélioration de la mise à l’échelle pour les grands nombres d’entités dans le document de métadonnées de Fédération agrégé. Auparavant, cela échouait avec l’erreur « ADMIN0017 ». 
  - Interrogez à l’aide du paramètre « ScopeGroupID » via l’applet de commande AdfsRelyingPartyTrustsGroup PSH. 
  - Gestion des conditions d’erreur autour des entityID en double


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Spécification de ressource de style Azure AD dans le paramètre d’étendue 
Auparavant, AD FS nécessitait que la ressource et la portée souhaitées se trouvent dans un paramètre distinct dans une demande d’authentification. Par exemple, une requête OAuth typique ressemble à ce qui suit : 7 **https :&#47;&#47;FS.contoso.com/ADFS/oauth2/Authorize ? </br>response_type = code & client_id = claimsxrayclient & ressource = urn : Microsoft : </br>adfs : claimsxray & étendue = OAuth & redirect_uri = https :&#47; &#47; adfshelp.microsoft.com/</br> ClaimsXray/TokenResponse & prompt = login**
 
Avec AD FS sur le serveur 2019, vous pouvez désormais transmettre la valeur de ressource incorporée dans le paramètre d’étendue. Cela est cohérent avec la manière dont il est possible d’effectuer une authentification par rapport à Azure AD également. 

Le paramètre d’étendue peut désormais être organisé comme une liste séparée par des espaces, où chaque entrée est structure en tant que ressource/étendue. Exemple :  

**< créer un exemple de demande valide >**
> [!NOTE]
> Une seule ressource peut être spécifiée dans la demande d’authentification. Si plusieurs ressources sont incluses dans la demande, AD FS renvoie une erreur et l’authentification échoue. 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Clé de vérification pour la prise en charge de l’échange de code (PKCE) pour oAuth 
Les clients publics OAuth utilisant l’octroi de code d’autorisation sont exposés à l’attaque d’interception de code d’autorisation.  L’attaque est bien décrite dans le document RFC 7636. Pour atténuer cette attaque, AD FS dans le serveur 2019 prend en charge la clé de vérification pour l’échange de code (PKCE) pour le workflow d’octroi de code d’autorisation OAuth. 
 
Pour tirer parti de la prise en charge PKCE, cette spécification ajoute des paramètres supplémentaires aux demandes d’autorisation et de jeton d’accès OAuth 2,0.

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

R. Le client crée et enregistre un secret nommé « code_verifier » et dérive une version transformée « t (code_verifier) » (appelée « code_challenge »), qui est envoyée dans la demande d’autorisation 2,0 OAuth avec la méthode de transformation « t_m ». 

B. Le point de terminaison d’autorisation répond comme d’habitude, mais enregistre « t (code_verifier) » et la méthode de transformation. 

C. Le client envoie alors le code d’autorisation dans la demande de jeton d’accès comme d’habitude, mais il comprend le secret « code_verifier » généré à (A). 

E. Le AD FS transforme « code_verifier » et le compare à « t (code_verifier) » à partir de (B).  L’accès est refusé s’ils ne sont pas égaux. 

#### <a name="faq"></a>Questions fréquentes (FAQ) 
**QUESTION.** Puis-je passer une valeur de ressource dans le cadre de la valeur d’étendue comme la façon dont les requêtes sont effectuées sur Azure AD ? 
</br>**UN.** Avec AD FS sur le serveur 2019, vous pouvez désormais transmettre la valeur de ressource incorporée dans le paramètre d’étendue. Le paramètre d’étendue peut désormais être organisé comme une liste séparée par des espaces, où chaque entrée est structure en tant que ressource/étendue. Exemple :  
**< créer un exemple de demande valide >**

**QUESTION.** AD FS prend-il en charge l’extension PKCE ?
</br>**UN.** AD FS du serveur 2019 prend en charge la clé de vérification pour l’échange de code (PKCE) pour le workflow d’octroi de code d’autorisation OAuth 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Nouveautés des services de fédération Active Directory (AD FS) pour Windows Server 2016   
Si vous recherchez des informations sur les versions antérieures de AD FS, consultez les articles suivants :  
 [ADFS dans Windows Server 2012 ou 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) et [AD FS 2,0](https://technet.microsoft.com/library/adfs2.aspx)  

 Services ADFS fournit un contrôle d’accès et une authentification unique sur un large éventail d’applications, notamment Office 365, les applications SaaS basées sur le Cloud et les applications sur le réseau d’entreprise.  
* Pour l’organisation informatique, elle vous permet de fournir une authentification et un contrôle d’accès aux applications modernes et héritées, localement et dans le Cloud, en fonction du même ensemble d’informations d’identification et de stratégies.    
* Pour l’utilisateur, il fournit une authentification transparente à l’aide des mêmes informations d’identification de compte familières.  
* Pour le développeur, il offre un moyen simple d’authentifier les utilisateurs dont les identités résident dans l’annuaire d’organisation afin que vous puissiez concentrer vos efforts sur votre application, et non sur l’authentification ou l’identité.  

Cet article décrit les nouveautés de AD FS dans Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Supprimer les mots de passe de l’extranet  
AD FS 2016 offre trois nouvelles options de connexion sans mot de passe, ce qui permet aux organisations d’éviter les risques de compromission du réseau contre les mots de passe de hameçonnage, de fuite ou de vol. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Se connecter avec Azure Multi-Factor Authentication
AD FS 2016 s’appuie sur les fonctionnalités d’authentification multifacteur (MFA) de AD FS dans Windows Server 2012 R2 en autorisant la connexion à l’aide d’un code Azure MFA uniquement, sans entrer d’abord un nom d’utilisateur et un mot de passe.

* Avec Azure MFA comme méthode d’authentification principale, l’utilisateur est invité à entrer son nom d’utilisateur et le code de mot de passe à usage unique à partir de l’application Azure Authenticator.  
* Avec Azure MFA comme méthode d’authentification secondaire ou supplémentaire, l’utilisateur fournit les informations d’identification d’authentification principales (à l’aide de l’authentification intégrée Windows, du nom d’utilisateur et du mot de passe, de la carte à puce ou du certificat d’utilisateur ou d’appareil), puis voit une invite de texte, connexion Azure MFA basée sur un mot de passe à usage unique.  
* Avec le nouvel adaptateur Azure MFA intégré, le programme d’installation et de configuration pour Azure MFA avec AD FS n’a jamais été plus simple.
* Les organisations peuvent tirer parti de l’authentification multifacteur Azure sans avoir besoin d’un serveur Azure MFA local.
* L’authentification multifacteur Azure peut être configurée pour un intranet ou un extranet, ou dans le cadre d’une stratégie de contrôle d’accès.

Pour plus d’informations sur Azure MFA avec AD FS
*  [Configurer AD FS 2016 et Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Accès sans mot de passe à partir des appareils conformes
AD FS 2016 s’appuie sur les fonctionnalités d’inscription d’appareils précédentes pour activer l’authentification et le contrôle d’accès en fonction de l’état de conformité de l’appareil. Les utilisateurs peuvent se connecter à l’aide des informations d’identification de l’appareil, et la conformité est réévaluée lorsque les attributs de l’appareil changent, afin que vous puissiez toujours garantir l’application des stratégies.  Cela permet d’activer des stratégies telles que

* Activer l’accès uniquement à partir d’appareils gérés et/ou conformes  
* Activer l’accès extranet uniquement à partir d’appareils gérés et/ou conformes  
* Exiger l’authentification multifacteur pour les ordinateurs qui ne sont pas gérés ou non conformes  

AD FS fournit le composant local des stratégies d’accès conditionnel dans un scénario hybride. Lorsque vous inscrivez des appareils avec Azure AD pour l’accès conditionnel aux ressources de Cloud, l’identité de l’appareil peut également être utilisée pour les stratégies de AD FS.

![Nouveautés](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Pour plus d’informations sur l’utilisation de l’accès conditionnel basé sur les appareils dans le Cloud   
 *  [Azure Active Directory l’accès conditionnel](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

Pour plus d’informations sur l’utilisation de l’accès conditionnel basé sur les appareils avec AD FS
*  [Planification de l’accès conditionnel basé sur les appareils avec AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Stratégies de Access Control dans AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Se connecter avec Windows Hello entreprise   
Les appareils Windows 10 présentent Windows Hello et Windows Hello entreprise, en remplaçant les mots de passe utilisateur par des informations d’identification de l’utilisateur puissantes, protégées par le mouvement de l’utilisateur (un code confidentiel, un mouvement biométrique comme une empreinte digitale ou une reconnaissance faciale). AD FS 2016 prend en charge ces nouvelles fonctionnalités Windows 10 afin que les utilisateurs puissent se connecter à AD FS applications à partir de l’intranet ou de l’extranet sans avoir besoin de fournir un mot de passe.

Pour plus d’informations sur l’utilisation de Microsoft Windows Hello entreprise dans votre organisation
*  [Activer Windows Hello entreprise dans votre organisation](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Sécuriser l’accès aux applications

### <a name="modern-authentication"></a>Authentification moderne
AD FS 2016 prend en charge les derniers protocoles modernes qui offrent une meilleure expérience utilisateur pour Windows 10, ainsi que pour les appareils et applications iOS et Android les plus récents.  

Pour plus d’informations, consultez [AD FS des scénarios pour les développeurs](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurer des stratégies de contrôle d’accès sans avoir à connaître le langage des règles de revendication  
Auparavant, AD FS administrateurs devaient configurer des stratégies à l’aide du langage de règle de revendication AD FS, ce qui complique la configuration et la maintenance des stratégies. Avec les stratégies de contrôle d’accès, les administrateurs peuvent utiliser des modèles intégrés pour appliquer des stratégies courantes telles que
* Autoriser l’accès intranet uniquement
* Autoriser tout le monde et demander l’authentification MFA à l’extranet
* Autoriser tout le monde et demander l’authentification MFA d’un groupe spécifique

Les modèles sont faciles à personnaliser à l’aide d’un processus piloté par un Assistant pour ajouter des exceptions ou des règles de stratégie supplémentaires et peuvent être appliqués à une ou plusieurs applications pour l’application cohérente des stratégies.

Pour plus d’informations [, consultez stratégies de contrôle d’accès dans AD FS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Activer l’authentification avec des annuaires LDAP non-Active Directory  
De nombreuses organisations ont une combinaison de Active Directory et de répertoires tiers. Avec l’ajout de la prise en charge de AD FS pour l’authentification des utilisateurs stockés dans des annuaires compatibles LDAP v3, AD FS peut désormais être utilisé pour :
* Utilisateurs de tiers, répertoires compatibles LDAP v3
* Les utilisateurs de Active Directory forêts pour lesquelles une approbation Active Directory bidirectionnelle n’est pas configurée
* Utilisateurs de services AD LDS (Active Directory Lightweight Directory Services) (AD LDS)

Pour plus d’informations, consultez [configurer AD FS pour authentifier les utilisateurs stockés dans les annuaires LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Meilleure expérience de connexion
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personnaliser l’expérience de connexion pour les applications AD FS  
Nous avons appris que la possibilité de personnaliser l’expérience de connexion pour chaque application serait une amélioration de la convivialité, en particulier pour les organisations qui fournissent une authentification pour les applications qui représentent plusieurs entreprises ou marques différentes.  

Auparavant, AD FS dans Windows Server 2012 R2 offrait une expérience d’authentification commune pour toutes les applications par partie de confiance, avec la possibilité de personnaliser un sous-ensemble de contenu textuel par application. Avec Windows Server 2016, vous pouvez personnaliser non seulement les messages, mais aussi les images, le logo et le thème Web par application. En outre, vous pouvez créer des thèmes Web personnalisés et les appliquer par partie de confiance.  

Pour plus d’informations, consultez [Personnalisation de la connexion de l’utilisateur AD FS.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>Facilité de gestion et améliorations opérationnelles  
La section suivante décrit les scénarios opérationnels améliorés introduits avec Services ADFS dans Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Audit rationalisé pour faciliter la gestion administrative  
Dans AD FS pour Windows Server 2012 R2, un grand nombre d’événements d’audit ont été générés pour une requête unique et les informations pertinentes sur une activité de connexion ou d’émission de jetons sont absentes (dans certaines versions de AD FS) ou réparties sur plusieurs événements d’audit. Par défaut, les événements d’audit AD FS sont désactivés en raison de leur nature détaillée.  
Avec la sortie de AD FS 2016, l’audit est devenu plus rationalisé et moins détaillé.  

Pour plus d’informations [, consultez améliorations apportées à l’audit de AD FS dans Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Interopérabilité améliorée avec SAML 2,0 pour la participation aux conversions  
AD FS 2016 contient une prise en charge du protocole SAML supplémentaire, notamment la prise en charge de l’importation d’approbations basées sur des métadonnées contenant plusieurs entités. Cela vous permet de configurer AD FS pour participer à des conversions telles que la Fédération inhabituelle et d’autres implémentations conformes à la norme eGov 2,0.  

Pour plus d’informations [, consultez interopérabilité améliorée avec SAML 2,0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Gestion simplifiée des mots de passe pour les utilisateurs Office 365 fédérés  
Vous pouvez configurer Services ADFS (AD FS) pour envoyer des revendications d’expiration de mot de passe aux approbations de partie de confiance (applications) protégées par AD FS. Le mode d’utilisation de ces revendications dépend de l’application. Par exemple, avec Office 365 en tant que partie de confiance, des mises à jour ont été implémentées sur Exchange et Outlook pour notifier les utilisateurs fédérés de leurs mots de passe arrivant à expiration.  

Pour plus d’informations [, consultez configurer AD FS pour envoyer des revendications d’expiration de mot de passe.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Il est plus facile de passer de AD FS dans Windows Server 2012 R2 à AD FS dans Windows Server 2016  
Auparavant, la migration vers une nouvelle version de AD FSait l’exportation de la configuration de l’ancienne batterie de serveurs et l’importation vers une nouvelle batterie parallèle.  

Désormais, le passage d’AD FS sur Windows Server 2012 R2 à AD FS sur Windows Server 2016 est devenu beaucoup plus facile. Ajoutez simplement un nouveau serveur Windows Server 2016 à une batterie de serveurs Windows Server 2012 R2, et la batterie de serveurs agira au niveau du comportement de la batterie de serveurs Windows Server 2012 R2, de sorte qu’elle se présente comme une batterie de serveurs Windows Server 2012 R2.  

Ensuite, ajoutez de nouveaux serveurs Windows Server 2016 à la batterie, vérifiez la fonctionnalité et supprimez les serveurs plus anciens de l’équilibreur de charge. Une fois que tous les nœuds de la batterie de serveurs exécutent Windows Server 2016, vous êtes prêt à mettre à niveau le niveau de comportement de la batterie de serveurs vers 2016 et à commencer à utiliser les nouvelles fonctionnalités.  

Pour plus d’informations [, consultez Mise à niveau vers AD FS dans Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
