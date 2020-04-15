---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Nouveautés des services de fédération Active Directory (AD FS) pour Windows Server 2016
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/22/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e88297bdbd55d2f834f1bff72b6d05bdf356bb85
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860252"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Nouveautés des services de fédération Active Directory (AD FS)


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Nouveautés des services de fédération Active Directory (AD FS) pour Windows Server 2019

### <a name="protected-logins"></a>Connexions protégées
Voici un bref résumé des mises à jour des connexions protégées disponibles dans AD FS 2019 :
- **Fournisseurs d’authentification externes en tant que fournisseurs principaux** - Les clients peuvent maintenant utiliser des produits d’authentification tiers comme premier facteur et ne pas exposer de mots de passe comme premier facteur. Dans les cas où un fournisseur d’authentification externe peut prouver deux facteurs, il peut revendiquer l’authentification MFA. 
- **Authentification par mot de passe en tant qu’authentification supplémentaire** - Les clients disposent d’une option de boîte de réception entièrement prise en charge afin d’utiliser le mot de passe uniquement pour le facteur supplémentaire après l’utilisation d’une option sans mot de passe comme premier facteur. Cela améliore l’expérience client par rapport à AD FS 2016, où les clients devaient télécharger un adaptateur github qui est désormais pris en charge tel quel. 
- **Module d’évaluation des risques enfichable**  - Les clients peuvent maintenant créer leurs propres modules plug-ins pour bloquer certains types de requêtes lors de la phase de préauthentification. Cela permet aux clients d’utiliser plus facilement l’intelligence du cloud, par exemple la protection d’identité, afin de bloquer les connexions pour les utilisateurs à risque ou les transactions risquées.  Pour plus d’informations, consultez [Créer des plug-ins avec un modèle d’évaluation des risques AD FS 2019](../../ad-fs/development/ad-fs-risk-assessment-model.md). 
- **Améliorations apportées à ESL** - Amélioration du QFE ESL dans la version 2016 avec l’ajout des fonctionnalités suivantes :
    - Permet aux clients d’être en mode audit tout en étant protégé par la fonctionnalité de verrouillage extranet « classique » disponible depuis AD FS 2012 R2. Actuellement, les clients de la version 2016 ne disposeraient d’aucune protection en mode audit. 
    - Active le seuil de verrouillage indépendant pour les emplacements familiers. Cela permet à plusieurs instances d’applications qui s’exécutent avec un compte de service commun de substituer les mots de passe avec le moins d’impact possible. 

### <a name="additional-security-improvements"></a>Améliorations de sécurité supplémentaires
Les améliorations de sécurité supplémentaires suivantes sont disponibles dans AD FS 2019 :
- **PSH à distance à l’aide de la connexion par carte à puce** - Les clients peuvent maintenant utiliser des cartes à puce pour se connecter à distance à AD FS par le biais de PSH, et ainsi gérer toutes les fonctions PSH, notamment les applets de commande PSH multinœuds.
- **Personnalisation d’en-tête HTTP** Les clients peuvent maintenant personnaliser les en-têtes HTTP émis pendant les réponses AD FS. Cela comprend les en-têtes suivants :
     - HSTS : indique que les points de terminaison AD FS peuvent seulement être utilisés sur des points de terminaison HTTPS pour l’application par un navigateur conforme.
     - x-frame-options : permet aux administrateurs AD FS d’autoriser des parties de confiance spécifiques à incorporer des iFrames pour les pages de connexion interactive AD FS. À utiliser avec précaution et uniquement sur les hôtes HTTPS. 
     - En-tête futur : des en-têtes futurs supplémentaires peuvent également être configurés. 

Pour plus d’informations, consultez [Personnaliser des en-têtes de réponse de sécurité HTTP avec AD FS 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md). 

### <a name="authenticationpolicy-capabilities"></a>Fonctionnalités d’authentification/de stratégie
Les fonctionnalités d’authentification/de stratégie suivantes sont disponibles dans AD FS 2019 :
- **Spécifier la méthode d’authentification pour une authentification supplémentaire par RP** -Les clients peuvent maintenant utiliser des règles de revendication pour déterminer le fournisseur d’authentification supplémentaire à appeler. C’est utile pour deux cas d’usage :
    - Les clients effectuent la transition d’un fournisseur d’authentification supplémentaire à un autre. De cette façon, quand ils intègrent des utilisateurs à un fournisseur d’authentification plus récent, ils peuvent utiliser des groupes pour contrôler quel le fournisseur d’authentification supplémentaire est appelé.
    - Les clients ont besoin d’un fournisseur d’authentification supplémentaire spécifique (par exemple un certificat) pour certaines applications. 
- **Limiter l’authentification des appareils basée sur TLS uniquement aux applications qui en ont besoin** - Les clients peuvent maintenant restreindre les authentifications des appareils basées sur le protocole TLS aux seules applications qui effectuent un accès conditionnel basé sur l’appareil. Cela empêche toute invite indésirable pour l’authentification des appareils (ou les échecs si l’application cliente ne peut pas la gérer) pour les applications qui ne nécessitent pas l’authentification des appareils basée sur TLS.
- **Prise en charge de l’actualisation MFA** - AD FS prend maintenant en charge la possibilité de redemander les informations d’identification du deuxième facteur en fonction de leur actualisation. Cela permet aux clients d’effectuer une transaction initiale avec deux facteurs et de se voir demander le deuxième facteur de manière périodique uniquement. Cela est uniquement disponible pour les applications qui peuvent fournir un paramètre supplémentaire dans la demande, et n’est pas un paramètre configurable dans AD FS. Ce paramètre est pris en charge par Azure AD quand l’option « Mémoriser mon authentification multifacteur pour X jours » est configurée et que l’indicateur « supportsMFA » a la valeur true dans les paramètres d’approbation de domaine fédéré dans Azure AD. 

### <a name="sign-in-sso-improvements"></a>Améliorations apportées à l’authentification unique pour la connexion
Les améliorations suivantes ont été apportées à l’authentification unique pour la connexion dans AD FS 2019 :

- [Expérience utilisateur paginée avec thème centré](../operations/AD-FS-paginated-sign-in.md) - AD FS dispose maintenant d’un flux d’expérience utilisateur paginé qui lui permet de valider et de fournir une expérience de connexion plus fluide. AD FS utilise désormais une interface utilisateur centrée (au lieu du côté droit de l’écran). Vous aurez peut-être besoin d’images de logo et d’arrière-plan plus récentes adaptées à cette expérience. Cela reflète également les fonctionnalités proposées dans Azure AD.
- **Résolutions de bogues État d’authentification unique persistant pour les appareils Win10 lors de l’authentification PRT**   Cela résout un problème selon lequel l’état MFA n’était pas conservé lors de l’utilisation de l’authentification PRT pour les appareils Windows 10. La conséquence de ce problème était que les utilisateurs finaux étaient souvent invités à entrer des informations d’identification de deuxième facteur (MFA). Le correctif rend également l’expérience cohérente quand l’authentification de l’appareil est effectuée avec succès par le biais du protocole TLS du client et du mécanisme PRT. 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Prise en charge de la création d’applications métier modernes
La prise en charge suivante pour la création d’applications métier modernes a été ajoutée à AD FS 2019 :

 - **Profil de flux d’appareil OAuth** - AD FS prend maintenant en charge le profil de flux d’appareil OAuth pour effectuer des connexions sur des appareils qui n’ont pas de surface d’interface utilisateur, afin de prendre en charge des expériences de connexion riches. Cela permet à l’utilisateur d’effectuer l’expérience de connexion sur un autre appareil. Cette fonctionnalité est requise pour l’expérience Azure CLI dans Azure Stack, et peut être utilisée dans d’autres cas. 
 - **Suppression du paramètre « Resource »** - AD FS a maintenant supprimé la nécessité de spécifier un paramètre de ressource qui est conforme aux spécifications OAuth actuelles. Les clients peuvent désormais spécifier l’identificateur d’approbation de la partie de confiance en tant que paramètre d’étendue, en plus des autorisations demandées. 
 - **En-têtes CORS dans les réponses AD FS** - Les clients peuvent maintenant créer des applications monopages qui autorisent les bibliothèques JS côté client à valider la signature de l’id_token en interrogeant les clés de signature à partir du document de découverte OIDC sur AD FS. 
 - **Prise en charge de PKCE** - AD FS ajoute la prise en charge de PKCE pour fournir un workflow de code d’authentification sécurisé dans OAuth. Cela ajoute une couche supplémentaire de sécurité à ce flux, afin d’empêcher le détournement du code et sa relecture à partir d’un autre client. 
 - **Résolution de bogue : envoi de x5t et revendication kid** - il s’agit d’une résolution de bogue mineur. AD FS envoie maintenant en plus la revendication « kid » afin de désigner l’indicateur d’ID de clé pour la vérification de la signature. Avant, AD FS l’envoyait uniquement en tant que revendication « x5t ».

### <a name="supportability-improvements"></a>Améliorations de la prise en charge
Les améliorations de prise en charge suivantes ne font pas partie d’AD FS 2019 :
- **Envoyer les détails de l’erreur aux administrateurs AD FS** - Permet aux administrateurs de configurer les utilisateurs finaux pour qu’ils envoient des journaux de débogage relatifs à un échec de l’authentification de l’utilisateur final, afin qu’ils soient stockés en tant que fichiers compressés pour une consommation aisée. Les administrateurs peuvent également configurer une connexion SMTP de façon à envoyer automatiquement le fichier compressé à un compte e-mail de triage ou à créer automatiquement un ticket basé sur l’e-mail. 

### <a name="deployment-updates"></a>Mises à jour de déploiement
Les mises à jour de déploiement suivantes sont maintenant incluses dans AD FS 2019 :
- **Niveau de comportement de batterie de serveurs 2019** - Comme avec AD FS 2016, une nouvelle version du niveau de comportement de batterie de serveurs est nécessaire pour activer les nouvelles fonctionnalités décrites ci-dessus. Cela permet de passer de :
    - 2012 R2-> 2019
    - 2016 -> 2019     

### <a name="saml-updates"></a>Mises à jour SAML
La mise à jour SAML suivante se trouve dans AD FS 2019 :
- **Résolutions de bogues dans la fédération agrégée** - De nombreuses résolutions de bogues ont été appliquées à la prise en charge de la fédération agrégée (par exemple InCommon). Les résolutions concernent les aspects suivants : 
  - Amélioration de la mise à l’échelle pour les quantités élevées d’entités dans le document de métadonnées de fédération agrégée. Auparavant, cela échouait avec l’erreur « ADMIN0017 ». 
  - Interrogation à l’aide du paramètre « ScopeGroupID » par le biais de l’applet de commande PSH AdfsRelyingPartyTrustsGroup. 
  - Gestion des conditions d’erreur liées aux entityID en double


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Spécification de ressource de style Azure AD dans le paramètre d’étendue 
Auparavant, AD FS exigeait que la ressource et l’étendue souhaitées se trouvent dans un paramètre distinct dans toute requête d’authentification. Par exemple, une requête OAuth ordinaire ressemblait à ce qui suit : 7 **https:&#47;&#47;fs.contoso.com/adfs/oauth2/authorize?</br>response_type=code&client_id=claimsxrayclient&resource=urn:microsoft:</br>adfs:claimsxray&scope=oauth&redirect_uri=https:&#47;&#47;adfshelp.microsoft.com/</br> ClaimsXray/TokenResponse&prompt=login**
 
Avec AD FS sur Server 2019, vous pouvez désormais transmettre la valeur de ressource incorporée dans le paramètre d’étendue. C’est cohérent avec la manière dont il est également possible d’effectuer une authentification auprès d’Azure AD. 

Le paramètre d’étendue peut maintenant être organisé sous forme de liste séparée par des espaces, où chaque entrée est structurée en tant que ressource/étendue. 

> [!NOTE]
> Une seule ressource peut être spécifiée dans la demande d’authentification. Si plusieurs ressources sont incluses dans la demande, AD FS retourne une erreur et l’authentification échoue. 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Prise en charge de PKCE (Proof Key for Code Exchange) pour oAuth 
Les clients publics OAuth utilisant l’octroi de code d’autorisation sont exposés à une attaque par interception du code d’autorisation.  L’attaque est bien décrite dans la RFC 7636. Pour l’atténuer, AD FS dans Server 2019 prend en charge PKCE pour le flux d’octroi de codes d’autorisation OAuth. 
 
Pour tirer parti de la prise en charge de PKCE, cette spécification ajoute des paramètres supplémentaires aux requêtes d’autorisation et de jeton d’accès OAuth 2.0.

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. Le client crée et enregistre un secret nommé « code_verifier », et dérive une version transformée « t(code_verifier) » (appelée « code_challenge »), qui est envoyée dans la requête d’autorisation OAuth 2.0 avec la méthode de transformation « t_m ». 

B. Le point de terminaison d’autorisation répond comme d’habitude, mais enregistre « t(code_verifier) » et la méthode de transformation. 

C. Le client envoie alors le code d’autorisation dans la requête de jeton d’accès comme d’habitude, mais il inclut le secret « code_verifier » généré à l’étape A. 

D. AD FS transforme « code_verifier » et le compare à « t(code_verifier) » obtenu à l’étape B.  L’accès est refusé s’ils ne sont pas égaux. 

#### <a name="faq"></a>Forum Aux Questions 
> [!NOTE] 
> Vous pouvez rencontrer cette erreur dans les journaux des événements d’administration ADFS : Requête OAuth non valide reçue. Le client « NOM » n’est pas autorisé à accéder à la ressource avec l’étendue « ugs ». Pour corriger cette erreur : 
> 1. Lancez la console de gestion AD FS. Accédez à « Services > Descriptions d’étendue ».
> 2. Cliquez avec le bouton droit sur « Descriptions d’étendue » et sélectionnez « Ajouter une description d’étendue ».
> 3. Sous le nom, tapez « ugs » et cliquez sur Appliquer > OK.
> 4. Lancez PowerShell en tant qu’administrateur.
> 5. Exécutez la commande « Get-AdfsApplicationPermission ». Recherchez l’élément ScopeNames :{openid, aza} qui contient le ClientRoleIdentifier. Notez la valeur de ObjectIdentifier.
> 6. Exécutez la commande « Set-AdfsApplicationPermission -TargetIdentifier <ObjectIdentifier de l’étape 5> -AddScope 'ugs'
> 7. Redémarrez le service ADFS.
> 8. Sur le client : Redémarrez le client. L’utilisateur est invité à provisionner WHFB.
> 9. Si la fenêtre de provisionnement ne s’affiche pas, collectez les journaux de suivi NGC pour un dépannage supplémentaire.

**Q.** Puis-je transmettre une valeur de ressource dans le cadre de la valeur d’étendue, comme pour les requêtes exécutées sur Azure AD ? 
</br>**A.** Avec AD FS sur Server 2019, vous pouvez désormais transmettre la valeur de ressource incorporée dans le paramètre d’étendue. Le paramètre d’étendue peut maintenant être organisé sous forme de liste séparée par des espaces, où chaque entrée est structurée en tant que ressource/étendue. Par exemple :  
**< create a valid sample request>**

**Q.** AD FS prend-il en charge l’extension PKCE ?
</br>**A.** AD FS dans Server 2019 prend en charge PKCE (Proof Key for Code Exchange) pour le flux d’octroi de codes d’autorisation OAuth. 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Nouveautés des services de fédération Active Directory (AD FS) pour Windows Server 2016   
Si vous recherchez des informations sur les versions antérieures d’AD FS, consultez les articles suivants :  
 [AD FS dans Windows Server 2012 ou 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) et [AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Les services de fédération Active Directory (AD FS) fournissent un contrôle d’accès et une authentification unique parmi un large éventail d’applications, notamment Office 365, les applications SaaS basées sur le cloud et les applications sur le réseau d’entreprise.  
* Du point de vue de l’organisation informatique, cela vous permet de fournir une authentification et un contrôle d’accès aux applications modernes et héritées, localement et dans le cloud, sur la base du même ensemble d’informations d’identification et de stratégies.    
* Du point de vue de l’utilisateur, cela offre une authentification fluide à l’aide des mêmes informations d’identification de compte.  
* Du point de vue du développeur, cela permet d’authentifier facilement les utilisateurs dont les identités résident dans l’annuaire de l’organisation, afin que vous puissiez concentrer vos efforts sur votre application, et non sur l’authentification ou l’identité.  

Cet article décrit les nouveautés d’AD FS dans Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Supprimer les mots de passe de l’extranet  
AD FS 2016 offre trois nouvelles options de connexion sans mot de passe, ce qui permet aux organisations d’éviter les risques de compromission du réseau due à l’hameçonnage, à la fuite ou au vol de mot de passe. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Se connecter avec Azure Multi-Factor Authentication
AD FS 2016 s’appuie sur les fonctionnalités d’authentification multifacteur (MFA) d’AD FS dans Windows Server 2012 R2 en autorisant la connexion à l’aide d’un code Azure MFA uniquement, sans entrée préalable d’un nom d’utilisateur et d’un mot de passe.

* Avec Azure MFA comme méthode d’authentification principale, l’utilisateur est invité à entrer son nom d’utilisateur et le code secret à usage unique de l’application Azure Authenticator.  
* Avec Azure MFA comme méthode d’authentification secondaire ou supplémentaire, l’utilisateur fournit les informations d’identification d’authentification principales (à l’aide de l’authentification Windows intégrée, du nom d’utilisateur et du mot de passe, de la carte à puce ou du certificat d’utilisateur ou d’appareil), puis reçoit une invite de connexion Azure MFA vocale, textuelle ou par code secret à usage unique.  
* Avec le nouvel adaptateur Azure MFA intégré, l’installation et la configuration d’Azure MFA avec AD FS n’a jamais été aussi simple.
* Les organisations peuvent tirer parti d’Azure MFA sans avoir besoin d’un serveur Azure MFA local.
* Azure MFA peut être configuré pour un intranet ou un extranet, ou dans le cadre d’une stratégie de contrôle d’accès.

Pour plus d’informations sur Azure MFA avec AD FS, consultez :
*  [Configurer AD FS 2016 et Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Accès sans mot de passe à partir des appareils conformes
AD FS 2016 s’appuie sur les fonctionnalités d’inscription d’appareils précédentes pour activer l’authentification et le contrôle d’accès en fonction de l’état de conformité de l’appareil. Les utilisateurs peuvent se connecter à l’aide des informations d’identification de l’appareil, et la conformité est réévaluée quand les attributs de l’appareil changent, afin que vous puissiez toujours garantir l’application des stratégies.  Cela permet d’activer des stratégies telles que :

* Activer l’accès uniquement à partir d’appareils managés et/ou conformes  
* Activer l’accès extranet uniquement à partir d’appareils managés et/ou conformes  
* Exiger l’authentification multifacteur pour les ordinateurs qui ne sont pas managés ou sont non conformes  

AD FS fournit le composant local des stratégies d’accès conditionnel dans un scénario hybride. Quand vous inscrivez des appareils auprès d’Azure AD pour l’accès conditionnel aux ressources cloud, l’identité de l’appareil peut également être utilisée pour les stratégies AD FS.

![Nouveautés](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Pour plus d’informations sur l’utilisation de l’accès conditionnel basé sur l’appareil dans le cloud, consultez :   
 *  [Accès conditionnel à Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

Pour plus d’informations sur l’utilisation de l’accès conditionnel basé sur l’appareil avec AD FS, consultez :
*  [Planifier l’accès conditionnel basé sur l’appareil avec AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Stratégies de contrôle d’accès dans AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Se connecter avec Windows Hello Entreprise  

> [!NOTE]
> Actuellement, Google Chrome et les [nouveaux navigateurs de projet open source Microsoft Edge basé sur Chromium](https://www.microsoft.com/edge?form=MB110A&OCID=MB110A) ne sont pas pris en charge pour l’authentification unique basée sur le navigateur avec Microsoft Windows Hello Entreprise. Utilisez Internet Explorer ou une version antérieure de Microsoft Edge.  

Les appareils Windows 10 contiennent Windows Hello et Windows Hello Entreprise, qui remplacent les mots de passe utilisateur par des informations d’identification de l’utilisateur puissantes, protégées par un mouvement de l’utilisateur (un code confidentiel, un mouvement biométrique comme une empreinte digitale ou une reconnaissance faciale). AD FS 2016 prend en charge ces nouvelles fonctionnalités de Windows 10 afin que les utilisateurs puissent se connecter aux applications AD FS à partir de l’intranet ou de l’extranet sans avoir besoin de fournir un mot de passe.

Pour plus d’informations sur l’utilisation de Microsoft Windows Hello Entreprise dans votre organisation, consultez :
*  [Activer Windows Hello Entreprise dans votre organisation](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Sécuriser l’accès aux applications

### <a name="modern-authentication"></a>Authentification moderne
AD FS 2016 prend en charge les protocoles modernes les plus récents qui offrent une meilleure expérience utilisateur pour Windows 10, ainsi que pour les appareils et applications iOS et Android les plus récents.  

Pour plus d’informations, consultez [Scénarios AD FS pour les développeurs](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md).  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurer des stratégies de contrôle d’accès sans avoir à connaître le langage de règles de revendication  
Avant, les administrateurs AD FS devaient configurer des stratégies à l’aide du langage de règles de revendication AD FS, ce qui compliquait la configuration et la maintenance des stratégies. Avec les stratégies de contrôle d’accès, les administrateurs peuvent utiliser des modèles intégrés pour appliquer des stratégies courantes telles que :
* Autoriser l’accès intranet uniquement
* Autoriser tout le monde et exiger l’authentification MFA à partir de l’extranet
* Autoriser tout le monde et exiger l’authentification MFA pour un groupe spécifique

Les modèles sont faciles à personnaliser à l’aide d’un processus piloté par un Assistant qui permet d’ajouter des exceptions ou des règles de stratégie supplémentaires, et ils peuvent être appliqués à une ou plusieurs applications afin d’appliquer les stratégies de manière cohérente.

Pour plus d’informations, consultez [Stratégies de contrôle d’accès dans AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Activer l’authentification avec des annuaires LDAP non-Active Directory  
De nombreuses organisations ont une combinaison d’Active Directory et d’annuaires tiers. Avec l’ajout de la prise en charge dans AD FS de l’authentification des utilisateurs stockés dans des annuaires compatibles LDAP v3, AD FS peut désormais être utilisé pour :
* Les utilisateurs d’annuaires tiers compatibles LDAP v3.
* Les utilisateurs des forêts Active Directory pour lesquelles aucune approbation bidirectionnelle Active Directory n’est configurée.
* Les utilisateurs des services AD LDS (Active Directory Lightweight Directory Services).

Pour plus d’informations, consultez [Configurer AD FS pour authentifier les utilisateurs stockés dans des annuaires LDAP](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md).  

## <a name="better-sign-in-experience"></a>Meilleure expérience de connexion
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personnaliser l’expérience de connexion pour les applications AD FS  
Vous nous avez suggéré que la possibilité de personnaliser l’expérience de connexion pour chaque application serait une bonne idée en terme de convivialité, en particulier pour les organisations qui fournissent une authentification pour les applications qui représentent plusieurs entreprises ou marques différentes.  

Avant, AD FS dans Windows Server 2012 R2 offrait une expérience d’authentification commune pour toutes les applications par partie de confiance, avec la possibilité de personnaliser un sous-ensemble de contenu textuel par application. Avec Windows Server 2016, vous pouvez personnaliser non seulement les messages, mais aussi les images, le logo et le thème web par application. Vous pouvez aussi créer des thèmes web personnalisés et les appliquer par partie de confiance.  

Pour plus d’informations, consultez [Personnalisation de la connexion utilisateur AD FS](../../ad-fs/operations/AD-FS-user-sign-in-customization.md).  



## <a name="manageability-and-operational-enhancements"></a>Améliorations fonctionnelles et de facilité de gestion  
La section suivante décrit les scénarios fonctionnels améliorés introduits avec les services AD FS dans Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Audit rationalisé pour faciliter la gestion administrative  
Dans AD FS pour Windows Server 2012 R2, un grand nombre d’événements d’audit étaient générés pour une demande unique, et les informations pertinentes sur une activité de connexion ou d’émission de jetons étaient soit absentes (dans certaines versions d’AD FS), soit réparties sur plusieurs événements d’audit. Par défaut, les événements d’audit AD FS sont désactivés en raison de leur nature détaillée.  
Avec la publication d’AD FS 2016, l’audit est plus rationalisé et moins détaillé.  

Pour plus d’informations, consultez [Améliorations de l’audit apportées à AD FS dans Windows Server 2016](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md).  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Amélioration de l’interopérabilité avec SAML 2.0 pour la participation aux confédérations  
AD FS 2016 offre une prise en charge du protocole SAML supplémentaire, notamment la prise en charge de l’importation des approbations basées sur des métadonnées contenant plusieurs entités. Cela vous permet de configurer AD FS pour participer à des confédérations telles que la fédération InCommon et d’autres implémentations conformes à la norme eGov 2.0.  

Pour plus d’informations, consultez [Interopérabilité améliorée avec SAML 2.0](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md).  

### <a name="simplified-password-management-for-federated-o365-users"></a>Gestion simplifiée des mots de passe pour les utilisateurs Office 365 fédérés  
Vous pouvez configurer les services AD FS pour envoyer des revendications d’expiration de mot de passe aux approbations de partie de confiance (applications) protégées par AD FS. Le mode d’utilisation de ces revendications dépend de l’application. Par exemple, avec Office 365 comme partie de confiance, des mises à jour ont été implémentées dans Exchange et Outlook pour informer les utilisateurs fédérés de l’expiration prochaine de leurs mots de passe.  

Pour plus d’informations, consultez [Configurer AD FS pour envoyer les revendications d’expiration de mot de passe](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md).  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Simplification du passage d’AD FS dans Windows Server 2012 R2 à AD FS dans Windows Server 2016  
Auparavant, la migration vers une nouvelle version d’AD FS nécessitait l’exportation de la configuration à partir de l’ancienne batterie de serveurs et l’importation vers une nouvelle batterie parallèle.  

Désormais, le passage d’AD FS sur Windows Server 2012 R2 à AD FS sur Windows Server 2016 est beaucoup plus facile. Il vous suffit d’ajouter un nouveau serveur Windows Server 2016 à une batterie de serveurs Windows Server 2012 R2, et la batterie de serveurs agira au niveau du comportement de batterie Windows Server 2012 R2 ; ainsi, elle se présentera et se comportera exactement comme une batterie de serveurs Windows Server 2012 R2.  

Ensuite, ajoutez de nouveaux serveurs Windows Server 2016 à la batterie, vérifiez la fonctionnalité et supprimez les anciens serveurs de l’équilibreur de charge. Une fois que tous les nœuds de la batterie exécutent Windows Server 2016, vous pouvez mettre à niveau le niveau de comportement de batterie de serveurs vers 2016 et commencer à utiliser les nouvelles fonctionnalités.  

Pour plus d’informations, consultez [Mise à niveau vers AD FS dans Windows Server 2016](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md).  
