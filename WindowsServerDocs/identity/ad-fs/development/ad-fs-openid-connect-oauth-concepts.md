---
title: AD FS les concepts OpenID Connect/OAuth
description: En savoir plus sur les concepts d’authentification AD FS modernes.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 26c1635d4218c7d33377b6b8a90bc96ea4ad37b3
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948782"
---
# <a name="ad-fs-openid-connectoauth-concepts"></a>AD FS les concepts OpenID Connect/OAuth
S’applique à AD FS 2016 et versions ultérieures
 
## <a name="modern-authentication-actors"></a>Acteurs d’authentification modernes 

|Acteur| Description|
|-----|-----|
|Utilisateur final|Il s’agit du principal de sécurité (utilisateurs, applications, services et groupes) qui doit accéder à la ressource.|  
|Client|Il s’agit de votre application Web, identifiée par son ID client. Le client est généralement le tiers avec lequel l’utilisateur final interagit et demande des jetons à partir du serveur d’autorisation.
|Serveur d’autorisation/fournisseur d’identité (IdP)| Il s’agit de votre serveur AD FS. Il est chargé de vérifier l’identité des principaux de sécurité qui existent dans l’annuaire d’une organisation. Il émet des jetons de sécurité (jeton d’accès porteur, jeton d’ID, jeton d’actualisation) lors de l’authentification réussie de ces principaux de sécurité.
|Serveur de ressources/fournisseur de ressources/partie de confiance| C’est là que réside la ressource ou les données. Il fait confiance au serveur d’autorisation pour authentifier et autoriser le client en toute sécurité et utilise des jetons d’accès au porteur pour s’assurer que l’accès à une ressource peut être accordé.

Le diagramme suivant fournit la relation la plus simple entre les acteurs :

![Acteurs d’authentification modernes](media/adfs-modern-auth-concepts/concept1.png)

## <a name="application-types"></a>Types d'applications 
 

|Type d’application|Description|Rôle|
|-----|-----|-----|
|Application native|Parfois appelé **client public**, il est destiné à être une application cliente qui s’exécute sur un PC ou un appareil et avec lequel l’utilisateur interagit.|Demande des jetons à partir du serveur d’autorisation (AD FS) pour l’accès des utilisateurs aux ressources. Envoie des requêtes HTTP aux ressources protégées, en utilisant les jetons en tant qu’en-têtes HTTP.| 
|Application serveur (application Web)|Application Web qui s’exécute sur un serveur et qui est généralement accessible aux utilisateurs via un navigateur. Étant donné qu’il est en mesure de maintenir son propre « secret » client ou d’informations d’identification, il est parfois appelé **client confidentiel**. |Demande des jetons à partir du serveur d’autorisation (AD FS) pour l’accès des utilisateurs aux ressources. Avant de demander un jeton, le client (application Web) doit s’authentifier à l’aide de sa clé secrète. | 
|API web|Ressource de fin à laquelle l’utilisateur accède. Considérez-les comme la nouvelle représentation des « parties de confiance ».|Consomme les jetons d’accès du porteur obtenus par les clients| 

## <a name="application-group"></a>Groupe d’applications 
 
Chaque client OAuth (application native ou Web) ou ressource (API Web) configuré avec AD FS doit être associé à un groupe d’applications. Les clients d’un groupe d’applications peuvent être configurés pour accéder aux ressources du même groupe. Un groupe d’applications peut contenir plusieurs clients et ressources.  

## <a name="security-tokens"></a>Jetons de sécurité 
 
L’authentification moderne utilise les types de jetons suivants : 
- **id_token**: jeton JWT émis par le serveur d’autorisation (AD FS) et consommé par le client. Les revendications dans le jeton d’ID contiennent des informations sur l’utilisateur afin que ce dernier puisse l’utiliser.  
- **access_token**: jeton JWT émis par le serveur d’autorisation (AD FS) et destiné à être consommé par la ressource. La revendication « AUD » ou audience de ce jeton doit correspondre à l’identificateur de la ressource ou de l’API Web.  
- **refresh_token**: jeton émis par AD FS que le client doit utiliser pour actualiser le id_token et le access_token. Le jeton est opaque pour le client et ne peut être consommé que par AD FS.  

## <a name="scopes"></a>Portées 
 
Lors de l’inscription d’une ressource dans AD FS, les étendues peuvent être configurées pour permettre à AD FS d’effectuer des actions spécifiques. Outre la configuration de l’étendue, la valeur d’étendue doit également être envoyée dans la demande de AD FS pour effectuer l’action. Par exemple, l’administrateur doit configurer l’étendue comme OpenID lors de l’inscription de la ressource, et l’application (client) doit envoyer Scope = OpenID dans la demande d’authentification de AD FS pour émettre un jeton d’ID. Vous trouverez ci-dessous des informations sur les étendues disponibles dans AD FS 
 
- aza-si vous utilisez des [extensions de protocole OAuth 2,0 pour les clients Service Broker](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) et si le paramètre scope contient l’étendue « aza », le serveur émet un nouveau jeton d’actualisation principal et le définit dans le champ refresh_token de la réponse, ainsi que la définition du champ refresh_token_expires_in sur la durée de vie du nouveau jeton d’actualisation principal, le cas échéant. 
- OpenID : permet à l’application de demander l’utilisation du protocole d’autorisation OpenID Connect. 
- logon_cert : l’étendue des logon_cert permet à une application de demander des certificats d’ouverture de session, qui peuvent être utilisés pour ouvrir une session de manière interactive sur des utilisateurs authentifiés. Le serveur AD FS omet le paramètre access_token de la réponse et fournit à la place une chaîne de certificats CMS encodée en base64 ou une réponse PKI complète CMC. Plus de détails disponibles [ici](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e).
- user_impersonation : l’étendue de la user_impersonation est nécessaire pour demander un jeton d’accès au nom de AD FS. Pour plus d’informations sur l’utilisation de cette étendue, reportez-vous à la [page créer une application à plusieurs niveaux à l’aide de OBO (au nom de) à l’aide d’OAuth avec AD FS 2016](ad-fs-on-behalf-of-authentication-in-windows-server.md). 
- allatclaims : l’étendue allatclaims permet à l’application de demander des revendications dans le jeton d’accès à ajouter également dans le jeton d’ID.   
- vpn_cert : l’étendue des vpn_cert permet à une application de demander des certificats VPN, qui peuvent être utilisés pour établir des connexions VPN à l’aide de l’authentification EAP-TLS. Cela n’est plus pris en charge. 
- e-mail : permet à l’application de demander une revendication de courrier électronique pour l’utilisateur connecté.  
- Profil : permet à l’application de demander des revendications liées au profil pour l’utilisateur de connexion.  

## <a name="claims"></a>Revendications 
 
Les jetons de sécurité (jetons d’accès et d’ID) émis par AD FS contiennent des revendications ou des assertions d’informations sur le sujet qui a été authentifié. Les applications peuvent utiliser des revendications pour diverses tâches, notamment pour les actions suivantes : 
- Valider le jeton 
- Identifier le locataire du répertoire du sujet 
- Afficher les informations utilisateur 
- Déterminez l’autorisation du sujet dans laquelle les revendications présentes dans un jeton de sécurité donné dépendent du type de jeton, du type d’informations d’identification utilisé pour authentifier l’utilisateur et de la configuration de l’application.  
 
## <a name="high-level-ad-fs-authentication-flow"></a>Circuit d’authentification AD FS de haut niveau 

![AD FS le workflow d’authentification](media/adfs-modern-auth-concepts/adfsauthflow.png)


 1. AD FS reçoit une demande d’authentification du client.  
 
 2. AD FS valide l’ID client dans la demande d’authentification avec l’ID client obtenu lors de l’inscription du client et de la ressource dans AD FS. Si vous utilisez un client confidentiel, AD FS également valide la clé secrète client fournie dans la demande d’authentification. AD FS également valider l’URI de redirection du client. 
 
 3. AD FS identifie la ressource à laquelle le client veut accéder via le paramètre de ressource passé dans la demande d’authentification. Si vous utilisez la bibliothèque cliente MSAL, le paramètre de ressource n’est pas envoyé. Au lieu de cela, l’URL de la ressource est envoyée dans le cadre du paramètre d’étendue : *scope = [URL de la ressource]//[valeurs d’étendue, par exemple, OpenID]* . 

    Si la ressource n’est pas passée à l’aide d’un paramètre de ressource ou d’étendue, ADFS utilise une ressource par défaut urn : Microsoft : UserInfo dont les stratégies (par exemple, la stratégie MFA, d’émission ou d’autorisation) ne peuvent pas être configurées. 
 
 4. Ensuite AD FS vérifie si le client dispose des autorisations d’accès à la ressource. AD FS vérifie également si les étendues transmises dans la demande d’authentification correspondent aux étendues configurées lors de l’inscription de la ressource. Si le client n’a pas les autorisations ou si les étendues appropriées ne sont pas envoyées dans la demande d’authentification, le processus d’authentification est terminé.   
 
 5. Une fois les autorisations et les étendues validées, AD FS authentifie l’utilisateur à l’aide de la [méthode d’authentification](../operations/configure-authentication-policies.md)configurée.   

 6. Si une [méthode d’authentification supplémentaire](../operations/configure-additional-authentication-methods-for-ad-fs.md) est requise en fonction de la stratégie de ressource ou de la stratégie d’authentification globale, AD FS déclenche l’authentification supplémentaire. 

 7. AD FS utilise [Azure MFA](../operations/configure-ad-fs-and-azure-mfa.md) ou une authentification [MFA](../operations/additional-authentication-methods-ad-fs.md) tierce pour effectuer l’authentification.   
 
 8. Une fois l’utilisateur authentifié, AD FS applique les [règles de revendication](../deployment/configuring-claim-rules.md) (détermine les revendications envoyées à la ressource dans le cadre des jetons de sécurité) et les [stratégies de contrôle d’accès](../operations/ad-fs-client-access-policies.md) (détermine que l’utilisateur a rempli les conditions requises pour accéder à la ressource).   

 9. Ensuite, AD FS génère les jetons d’accès et d’actualisation. 

 10. AD FS génère également le jeton d’ID. 
 
 11. Si Scope = allatclaims est inclus dans la demande d’authentification, le [jeton d’ID est personnalisé](custom-id-tokens-in-ad-fs.md) pour inclure les revendications dans le jeton d’accès en fonction des règles de revendication définies. 
    
 12. Une fois les jetons requis générés et personnalisés, AD FS répond au client, y compris les jetons. Le jeton d’ID est inclus dans la réponse uniquement si la demande d’authentification inclut Scope = OpenID. Le client peut toujours obtenir le jeton d’ID après l’authentification à l’aide du point de terminaison de jeton. 

## <a name="types-of-libraries"></a>Types de bibliothèques 
  
Deux types de bibliothèques sont utilisés avec AD FS : 
- **Bibliothèques clientes**: les clients natifs et les applications serveur utilisent des bibliothèques clientes pour obtenir des jetons d’accès pour appeler une ressource telle qu’une API Web. La bibliothèque d’authentification Microsoft (MSAL) est la bibliothèque cliente la plus récente et la plus recommandée lors de l’utilisation de AD FS 2019. Bibliothèque d’authentification Active Directory (ADAL) est recommandé pour AD FS 2016.  

- **Bibliothèques de middleware de serveur**: les applications Web utilisent des bibliothèques de middleware de serveur pour la connexion des utilisateurs. Les API web utilisent des bibliothèques de middleware de serveur pour valider les jetons qui sont envoyés par des clients natifs ou par d’autres serveurs. OWIN (Open Web interface pour .NET) est la bibliothèque middleware recommandée. 

## <a name="customize-id-token-additional-claims-in-id-token"></a>Personnaliser le jeton d’ID (revendications supplémentaires dans le jeton d’ID)
 
Dans certains scénarios, il est possible que l’application Web (client) ait besoin de revendications supplémentaires dans un jeton d’ID pour aider à la fonctionnalité. Pour ce faire, vous pouvez utiliser l’une des options suivantes. 

**Option 1 :** Doit être utilisé lors de l’utilisation d’un client public et l’application Web n’a pas de ressource à laquelle il tente d’accéder. L’option requiert 
1.  response_mode défini comme form_post 
2.  L’identificateur de partie de confiance (identificateur d’API Web) est identique à l’identificateur de client

![AD FS option de personnalisation de jeton 1](media/adfs-modern-auth-concepts/option1.png)

**Option 2 :** Doit être utilisé lorsque l’application Web a une ressource à laquelle elle tente d’accéder et qu’elle doit passer des revendications supplémentaires par le biais du jeton d’ID. Les clients publics et confidentiels peuvent être utilisés. L’option requiert 
1.  response_mode défini comme form_post 
2.  KB4019472 est installé sur vos serveurs AD FS 
3.  Étendue allatclaims assignée à la paire client-RP. Vous pouvez assigner l’étendue à l’aide de l’applet de commande PowerShell Grant-ADFSApplicationPermission (utilisez Set-AdfsApplicationPermission si elle a déjà été accordée une fois) comme indiqué dans l’exemple ci-dessous : 

    ``` powershell
    Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
    ```

![AD FS option de personnalisation de jeton 2](media/adfs-modern-auth-concepts/option2.png)

Pour mieux comprendre comment configurer une application Web dans ADFS pour obtenir un jeton d’ID personnalisé, consultez [personnaliser les revendications à émettre dans id_token lors de l’utilisation de OpenID Connect ou OAuth avec AD FS 2016 ou version ultérieure](Custom-Id-Tokens-in-AD-FS.md).

## <a name="single-log-out"></a>Déconnexion unique

La déconnexion unique entraîne la fin de toutes les sessions clientes à l’aide de l’ID de session. AD FS 2016 et versions ultérieures prennent en charge la déconnexion unique pour OpenID Connect/OAuth. Pour plus d’informations [, consultez déconnexion unique pour OpenID Connect avec AD FS](ad-fs-logout-openid-connect.md).


## <a name="ad-fs-endpoints"></a>Points de terminaison de AD FS

|Point de terminaison AD FS|Description|
|-----|-----|
|/Authorize|AD FS retourne un code d’autorisation qui peut être utilisé pour obtenir le jeton d’accès.|
|/Token|AD FS retourne un jeton d’accès qui peut être utilisé pour accéder à la ressource (API Web)|
|/userinfo|AD FS renvoie des revendications relatives à l’utilisateur authentifié|
|/devicecode|AD FS retourne le code de l’appareil et le code utilisateur|
|/logout|AD FS déconnecte l’utilisateur|
|/keys|AD FS les clés publiques utilisées pour signer les réponses|
|/.well-known/openid-configuration|AD FS retourne les métadonnées de connexion OAuth/OpenID|
