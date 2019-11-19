---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: FAQ AD FS
description: Forum aux questions (FAQ) pour AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0a2bbeeb459fd364db728579dc20015a2474fd25
ms.sourcegitcommit: e5df3fd267352528eaab5546f817d64d648b297f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2019
ms.locfileid: "74163094"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS Forum aux questions (FAQ)


La documentation suivante est un Forum aux questions concernant Services ADFS.  Le document a été divisé en groupes en fonction du type de question.

## <a name="deployment"></a>Déploiement

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Comment puis-je mettre à niveau/migrer à partir de versions précédentes de AD FS
Vous pouvez mettre à niveau AD FS à l’aide de l’une des options suivantes :


- Windows Server 2012 R2 AD FS à Windows Server 2016 AD FS ou version ultérieure. Notez que la méthodologie est la même si vous effectuez une mise à niveau de Windows Server 2016 AD FS vers Windows Server 2019 AD FS. 
    - [Mise à niveau vers AD FS dans Windows Server 2016 à l’aide d’une base de données WID](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Mise à niveau vers AD FS dans Windows Server 2016 à l’aide d’une base de données SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS à Windows Server 2012 R2 AD FS
    - [Migrer vers AD FS sur Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2,0 à Windows Server 2012 AD FS
    - [Migrer vers AD FS sur Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1. x pour AD FS 2,0
    - [Mise à niveau de AD FS 1. x vers AD FS 2,0](https://technet.microsoft.com/library/ff678035.aspx)

Si vous devez effectuer une mise à niveau à partir de AD FS 2,0 ou 2,1 (Windows Server 2008 R2 ou Windows Server 2012), vous devez utiliser les scripts intégrés (situés dans C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Pourquoi AD FS installation nécessite-t-elle un redémarrage du serveur ?

La prise en charge de HTTP/2 a été ajoutée dans Windows Server 2016, mais HTTP/2 ne peut pas être utilisé pour l’authentification par certificat client.  Étant donné que de nombreux scénarios de AD FS utilisent l’authentification par certificat client, et qu’un nombre significatif de clients ne prennent pas en charge les demandes de nouvelle tentative à l’aide de HTTP/1.1, AD FS configuration de la batterie de serveurs reconfigure les paramètres HTTP du serveur local sur HTTP/1.1.  Cela nécessite un redémarrage du serveur.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>Utilise-t-il des serveurs WAP Windows 2016 pour publier la batterie de AD FS sur Internet sans mettre à niveau la batterie de AD FS principale prise en charge ?
Oui, cette configuration est prise en charge, mais aucune nouvelle AD FS fonctionnalités 2016 ne serait prise en charge dans cette configuration.  Cette configuration est destinée à être temporaire au cours de la phase de migration de AD FS 2012 R2 à AD FS 2016 et ne doit pas être déployée pour de longues périodes de temps.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>Est-il possible de déployer AD FS pour Office 365 sans publier de proxy vers Office 365 ?
Oui, ceci est pris en charge. Toutefois, en tant qu’effet secondaire

1. Vous devrez gérer manuellement les certificats de signature de jetons de mise à jour, car Azure AD ne pourra pas accéder aux métadonnées de Fédération. Pour plus d’informations sur la mise à jour manuelle des [certificats de signature de jetons, consultez renouvellement des certificats de Fédération pour Office 365 et Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. Vous ne pourrez pas tirer parti des flux d’authentification hérités (par exemple, ExO proxy auth Flow)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>Quelles sont les exigences en matière d’équilibrage de charge pour les AD FS et les serveurs WAP ?

AD FS est un système sans État. Par conséquent, l’équilibrage de charge est relativement simple pour les connexions. Voici les principales recommandations pour les systèmes d’équilibrage de charge.


- Les équilibrages de charge ne doivent pas être configurés avec l’affinité IP. Cela peut poser une charge excessive sur un sous-ensemble de vos serveurs dans certains scénarios Exchange Online.
- Les équilibrages de charge ne doivent pas arrêter les connexions HTTPs et relancer une nouvelle connexion au serveur ADFS.
- Les équilibrages de charge doivent s’assurer que l’adresse IP de connexion doit être traduite en tant qu’adresse IP source dans le paquet HTTP lors de son envoi à ADFS. Dans le cas où un équilibreur de charge ne peut pas envoyer l’adresse IP source dans le paquet HTTP, l’équilibreur de charge doit ajouter l’adresse IP à l’en-tête x-forwarded-for, ou l’ajouter à l’en-tête. Cela est nécessaire pour la gestion correcte de certaines fonctionnalités liées à IP (adresse IP interdite,,... de verrouillage intelligent extranet) et peut entraîner une réduction de la sécurité en cas de configuration incorrecte.
- Les équilibrages de charge doivent prendre en charge SNI. Si ce n’est pas le cas, assurez-vous que AD FS est configuré pour créer des liaisons HTTPs pour gérer les clients non-SNI.
- Les équilibreurs de charge doivent utiliser le point de terminaison de sonde d’intégrité HTTP AD FS pour détecter si les AD FS ou les serveurs WAP sont en cours d’exécution et les excluent si un 200 OK n’est pas retourné.

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>Quelles sont les configurations à plusieurs forêts prises en charge par AD FS ?

AD FS prend en charge plusieurs configurations à plusieurs forêts et s’appuie sur le réseau de confiance AD DS sous-jacent pour authentifier les utilisateurs sur plusieurs domaines approuvés. Nous recommandons vivement des approbations de forêts bidirectionnelles, car il s’agit d’une configuration plus simple pour garantir que le sous-système d’approbation fonctionne correctement sans problème. En outre

- Dans le cas d’une approbation de forêt unidirectionnelle telle qu’une forêt DMZ contenant des identités de partenaires, nous vous recommandons de déployer ADFS dans la forêt Corp et de traiter la forêt DMZ comme une autre approbation de fournisseur de revendications locale connectée via LDAP. Dans ce cas, l’authentification intégrée de Windows ne fonctionne pas pour les utilisateurs de la forêt DMZ et il est nécessaire d’effectuer l’authentification par mot de passe, car il s’agit du seul mécanisme pris en charge pour LDAP. Si vous ne pouvez pas poursuivre cette option, vous devez configurer un autre AD FS dans la forêt DMZ et l’ajouter en tant qu’approbation de fournisseur de revendications dans ADFS dans la forêt Corp. Les utilisateurs devront effectuer une découverte de domaine d’hébergement, mais l’authentification intégrée de Windows et l’authentification par mot de passe fonctionnent. Apportez les modifications appropriées dans les règles d’émission dans ADFS dans la forêt DMZ, car ADFS dans la forêt Corp ne pourra pas obtenir d’informations utilisateur supplémentaires sur l’utilisateur à partir de la forêt DMZ.
- Bien que les approbations de niveau domaine soient prises en charge et puissent fonctionner, nous vous recommandons vivement de passer à un modèle d’approbation de niveau forêt. En outre, vous devez vous assurer que le routage UPN et la résolution de noms NETBIOS des noms doivent fonctionner correctement.

>[!NOTE]  
>Si l’authentification par élection est utilisée avec une configuration d’approbation bidirectionnelle, assurez-vous que l’utilisateur de l’appelant dispose de l’autorisation « autoriser l’authentification » sur le compte de service cible. 


## <a name="design"></a>Concevoir

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Quels fournisseurs d’authentification multifacteur tiers sont disponibles pour la AD FS ?
AD FS fournit un mécanisme extensible pour l’intégration des fournisseurs MFA tiers. Il n’existe pas de programme de certification pour cette configuration. Il est supposé que le fournisseur a effectué les validations nécessaires avant la mise en service. 

La liste des fournisseurs qui ont notifié Microsoft est publiée auprès des [fournisseurs MFA pour AD FS](../operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).  Il se peut qu’il y ait toujours des fournisseurs disponibles dont nous n’avons pas connaissance et que nous allons mettre à jour la liste au fur et à mesure de leur découverte.

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>Les proxys tiers sont-ils pris en charge avec AD FS ?
Oui, les proxys tiers peuvent être placés devant le proxy d’application Web, mais tout proxy tiers doit prendre en charge le [protocole MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) à utiliser à la place du proxy d’application Web.

Vous trouverez ci-dessous la liste des fournisseurs tiers que nous avons pris en charge.  Il se peut qu’il y ait toujours des fournisseurs disponibles dont nous n’avons pas connaissance et que nous allons mettre à jour la liste au fur et à mesure de leur découverte.

- [Gestionnaire de stratégie d’accès F5](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Où se trouve la feuille de calcul de dimensionnement de la capacité pour AD FS 2016 ?
La AD FS version 2016 de la feuille de calcul peut être téléchargée [ici](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Vous pouvez également l’utiliser pour AD FS dans Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Comment puis-je m’assurer que mes AD FS et les serveurs WAP prennent en charge les exigences ATP d’Apple ?

Apple a publié un ensemble de spécifications appelées sécurité de transport d’application (ATS) qui peuvent avoir un impact sur les appels des applications iOS qui s’authentifient auprès de AD FS.  Vous pouvez vous assurer que vos serveurs AD FS et WAP sont conformes en vous assurant qu’ils prennent en charge la [Configuration requise pour la connexion à l’aide d’ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
En particulier, vous devez vérifier que vos AD FS et les serveurs WAP prennent en charge TLS 1,2 et que la suite de chiffrement négociée de la connexion TLS prendra en charge le secret de transfert parfait.

Vous pouvez activer et désactiver SSL 2,0 et 3,0 et les versions TLS 1,0, 1,1 et 1,2 à l’aide [de gérer les protocoles SSL dans AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Pour vous assurer que votre AD FS et vos serveurs WAP négocient uniquement les suites de chiffrement TLS qui prennent en charge ATP, vous pouvez désactiver toutes les suites de chiffrement qui ne figurent pas dans la [liste des suites de chiffrement compatibles ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Pour ce faire, utilisez les [applets de commande PowerShell de Windows TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index).

## <a name="developer"></a>Développeur

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>Lors de la génération d’un id_token avec ADFS pour un utilisateur authentifié par rapport à AD, comment la revendication « Sub » est-elle générée dans le id_token ?
La valeur de la revendication « Sub » est le hachage de la valeur de l’ID client et de la revendication d’ancrage.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>Quelle est la durée de vie du jeton d’actualisation/jeton d’accès lorsque l’utilisateur se connecte par le biais d’une approbation de fournisseur de revendications distante sur WS-FED/SAML-P ?
La durée de vie du jeton d’actualisation correspond à la durée de vie du jeton que ADFS a obtenu de l’approbation du fournisseur de revendications distantes. La durée de vie du jeton d’accès sera la durée de vie du jeton de la partie de confiance pour laquelle le jeton d’accès est émis.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>J’ai besoin de retourner des étendues de profil et de messagerie, en plus de l’étendue OpenId. Puis-je obtenir des informations supplémentaires à l’aide des étendues ? Comment le faire dans AD FS ?
Vous pouvez utiliser des id_token personnalisés pour ajouter des informations pertinentes dans le id_token lui-même. Pour plus d’informations, consultez l’article [personnaliser les revendications à émettre dans id_token](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>Comment émettre des objets BLOB JSON dans des jetons JWT ?
Un ValueType spécial («<http://www.w3.org/2001/XMLSchema#json>») et un caractère d’échappement (\x22) pour cela ont été ajoutés à AD FS 2016. Veuillez l’exemple ci-dessous pour la règle d’émission et la sortie finale du jeton d’accès.

Exemple de règle d’émission :

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Revendication émise dans le jeton d’accès :

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>Puis-je passer une valeur de ressource dans le cadre de la valeur d’étendue comme la façon dont les requêtes sont effectuées sur Azure AD ?
Avec AD FS sur le serveur 2019, vous pouvez désormais transmettre la valeur de ressource incorporée dans le paramètre d’étendue. Le paramètre d’étendue peut désormais être organisé comme une liste séparée par des espaces, où chaque entrée est structure en tant que ressource/étendue. Par exemple,  
**< créer un exemple de demande valide >**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS prend-il en charge l’extension PKCE ?
AD FS du serveur 2019 prend en charge la clé de vérification pour l’échange de code (PKCE) pour le workflow d’octroi de code d’autorisation OAuth

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>Quelles sont les étendues autorisées prises en charge par AD FS ?
- aza-si vous utilisez des [extensions de protocole OAuth 2,0 pour les clients de service Broker](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) et si le paramètre scope contient l’étendue « aza », le serveur émet un nouveau jeton d’actualisation principal et le définit dans le champ refresh_token de la réponse, ainsi que la définition du champ refresh_token_expires_in sur la durée de vie du nouveau jeton d’actualisation principal, le cas échéant.
- OpenID : permet à l’application de demander l’utilisation du protocole d’autorisation OpenID Connect.
- logon_cert : l’étendue des logon_cert permet à une application de demander des certificats d’ouverture de session, qui peuvent être utilisés pour ouvrir une session de manière interactive sur des utilisateurs authentifiés. Le serveur AD FS omet le paramètre access_token de la réponse et fournit à la place une chaîne de certificats CMS encodée en base64 ou une réponse PKI complète CMC. Plus de détails disponibles [ici](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation : l’étendue de la user_impersonation est nécessaire pour demander un jeton d’accès au nom de AD FS. Pour plus d’informations sur l’utilisation de cette étendue, consultez [créer une application à plusieurs niveaux à l’aide de OBO (au nom de) à l’aide d’OAuth avec AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert : l’étendue des vpn_cert permet à une application de demander des certificats VPN, qui peuvent être utilisés pour établir des connexions VPN à l’aide de l’authentification EAP-TLS. Cela n’est plus pris en charge.
- e-mail : permet à l’application de demander une revendication de courrier électronique pour l’utilisateur connecté. Cela n’est plus pris en charge. 
- Profil : permet à l’application de demander des revendications liées au profil pour l’utilisateur de connexion. Cela n’est plus pris en charge. 


## <a name="operations"></a>Opérations

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Comment faire remplacer le certificat SSL pour AD FS ?
Le certificat SSL AD FS n’est pas le même que le certificat de communication du service AD FS trouvé dans le composant logiciel enfichable Gestion des AD FS.  Pour modifier le certificat SSL AD FS, vous devez utiliser PowerShell. Suivez les instructions de l’article ci-dessous :

[Gestion des certificats SSL dans AD FS et WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Comment puis-je activer ou désactiver les paramètres TLS/SSL pour AD FS
Pour désactiver ou activer les protocoles SSL et les suites de chiffrement, utilisez les éléments suivants :

[Gérer les protocoles SSL dans AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Le certificat SSL proxy doit-il être identique au certificat SSL AD FS ?  
Utilisez les instructions suivantes en ce qui concerne le certificat SSL proxy et le certificat SSL AD FS :


- Si le proxy est utilisé pour effectuer un proxy AD FS les demandes qui utilisent l’authentification intégrée de Windows, le certificat SSL proxy doit être identique (utiliser la même clé) que le certificat SSL du serveur de Fédération.
- Si la propriété AD FS « ExtendedProtectionTokenCheck » est activée (paramètre par défaut dans AD FS), le certificat SSL proxy doit être identique (utiliser la même clé) que le certificat SSL du serveur de Fédération.
- Dans le cas contraire, le certificat SSL proxy peut avoir une clé différente de celle du certificat SSL AD FS, mais il doit répondre aux mêmes [exigences](../overview/AD-FS-2016-Requirements.md) .

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>Pourquoi ne vois-je qu’un mot de passe de connexion sur AD FS et pas mes autres méthodes d’authentification que j’ai configurées ? 
AD FS n’affiche qu’une seule méthode d’authentification dans l’écran de connexion lorsque l’application requiert explicitement un URI d’authentification spécifique qui correspond à une méthode d’authentification configurée et activée. Il est transmis dans le paramètre « wauth » pour les demandes WS-Federation et le paramètre « RequestedAuthnCtxRef » dans une demande de protocole SAML. Par conséquent, seule la méthode d’authentification demandée est affichée (par exemple, login de mot de passe).

Lorsque AD FS est utilisé avec Azure AD, il est courant que les applications envoient le paramètre prompt = Login à Azure AD. Azure AD par défaut convertit cela en demande d’une nouvelle connexion basée sur un mot de passe à AD FS. Il s’agit de la raison la plus courante de voir une connexion de mot de passe sur AD FS à l’intérieur de votre réseau ou de ne pas voir une option pour vous connecter avec votre certificat. Cela peut être facilement résolu en modifiant les paramètres du domaine fédéré dans Azure AD. 

Pour plus d’informations sur la configuration de ce [paramètre, consultez services ADFS prompt = prise en charge des paramètres de connexion](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-change-the-ad-fs-service-account"></a>Comment puis-je modifier le compte de service AD FS ?
Pour modifier le compte de service AD FS, suivez les instructions à l’aide du [module PowerShell du compte de service](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule)de boîte à outils AD FS.

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Comment puis-je configurer des navigateurs pour utiliser l’authentification intégrée Windows (WIA) avec AD FS ?

Pour plus d’informations sur la façon de configurer des navigateurs [, consultez configurer des navigateurs pour utiliser l’authentification intégrée Windows (WIA) avec AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-turn-off-browserssoenabled"></a>Puis-je désactiver BrowserSsoEnabled ?
Si vous n’avez pas de stratégies de contrôle d’accès basées sur un appareil dans ADFS ou sur l’inscription de certificats Windows Hello entreprise à l’aide d’ADFS ; vous pouvez désactiver BrowserSsoEnabled. BrowserSsoEnabled permet à ADFS de collecter un PRT (jeton d’actualisation principal) du client qui contient des informations sur l’appareil. Sans cette authentification d’appareil ADFS ne fonctionnera pas sur les appareils Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>Quelle est la durée de validité des jetons AD FS ?

Souvent, cette question signifie « combien de temps les utilisateurs obtiennent-ils de l’authentification unique (SSO) sans devoir entrer de nouvelles informations d’identification, et comment puis-je en tant que contrôle d’administration ? »  Ce comportement, ainsi que les paramètres de configuration qui le contrôlent, sont décrits dans l’article [AD FS les paramètres d’authentification unique](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Les durées de vie par défaut des différents cookies et jetons sont répertoriées ci-dessous (ainsi que les paramètres qui régissent les durées de vie) :

**Appareils inscrits**

- PRT et cookies SSO : 90 jours maximum, régis par PSSOLifeTimeMins. (L’appareil fourni est utilisé au moins tous les 14 jours, contrôlés par DeviceUsageWindow)

- Jeton d’actualisation : calculé en fonction de la valeur ci-dessus pour fournir un comportement cohérent

- access_token : 1 heure par défaut, en fonction de la partie de confiance

- id_token : identique au jeton d’accès

**Appareils non inscrits**

- Cookies SSO : 8 heures par défaut, régie par SSOLifetimeMins.  Lorsque l’option maintenir la connexion (KMSI) est activée, la valeur par défaut est de 24 heures et peut être configurée via KMSILifetimeMins.


- Jeton d’actualisation : 8 heures par défaut. 24 heures avec KMSI activé

- access_token : 1 heure par défaut, en fonction de la partie de confiance

- id_token : identique au jeton d’accès

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS prend-il en charge la sécurité de transport strict HTTP (HSTS) ?  

HSTS (HTTP strict transport Security) est un mécanisme de stratégie de sécurité Web qui permet d’atténuer les attaques par rétrogradation de protocole et le piratage de cookies pour les services qui ont des points de terminaison HTTP et HTTPs. Il permet aux serveurs Web de déclarer que les navigateurs Web (ou d’autres agents utilisateur conformes) doivent uniquement interagir avec lui à l’aide de HTTPs et jamais via le protocole HTTP.

Tous les points de terminaison de AD FS pour le trafic d’authentification Web sont ouverts exclusivement via HTTPs.  Par conséquent, AD FS atténue efficacement les menaces fournies par le mécanisme de stratégie de sécurité de transport strict (par conception, il n’existe pas de passage à HTTP, car il n’y a aucun écouteur dans HTTP). En outre, AD FS empêche les cookies d’être envoyés à un autre serveur avec des points de terminaison de protocole HTTP en marquant tous les cookies avec l’indicateur sécurisé.

Par conséquent, l’implémentation de HSTS sur un serveur AD FS n’est pas nécessaire, car elle ne peut jamais être rétrogradée.  À des fins de conformité, les serveurs AD FS répondent à ces exigences, car ils ne peuvent jamais utiliser le protocole HTTP et tous les cookies sont marqués comme sécurisés.

En outre, AD FS 2016 (avec les correctifs les plus récents) et AD FS 2019 prend en charge l’émission de l’en-tête HSTS. Pour configurer cela, consultez [personnaliser les en-têtes de réponse de sécurité http avec AD FS](../operations/customize-http-security-headers-ad-fs.md)

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-MS-forwarded-client-IP ne contient pas l’adresse IP du client, mais contient l’adresse IP du pare-feu devant le proxy. Où puis-je trouver l’adresse IP appropriée du client ?
Il n’est pas recommandé d’effectuer une terminaison SSL avant le WAP. Si la terminaison SSL est effectuée devant le WAP, X-MS-forwarded-client-IP contient l’adresse IP du périphérique réseau devant le WAP. Vous trouverez ci-dessous une brève description des différentes revendications IP prises en charge par AD FS :
 - x-ms-client-IP : adresse IP réseau de l’appareil qui se connecte au STS.  Dans le cas d’une demande extranet, contient toujours l’adresse IP du WAP.
 - x-ms-forwarded-client-IP : revendication à valeurs multiples qui contient toutes les valeurs transmises à ADFS par Exchange Online, ainsi que l’adresse IP de l’appareil qui s’est connecté au WAP.
 - Userip : pour les demandes extranet, cette revendication contient la valeur x-ms-forwarded-client-IP.  Pour les demandes intranet, cette revendication contient la même valeur que x-ms-client-IP.

 En outre, dans AD FS 2016 (avec les correctifs les plus récents) et les versions ultérieures prennent également en charge la capture de l’en-tête x-forwarded-for. N’importe quel équilibrage de charge ou périphérique réseau qui n’est pas transféré au niveau de la couche 3 (l’adresse IP est conservée) doit ajouter l’adresse IP cliente entrante à l’en-tête x-forwarded-for standard du secteur. 

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>J’essaie d’obtenir des revendications supplémentaires sur le point de terminaison des informations utilisateur, mais son seul objet retourné. Comment puis-je obtenir des revendications supplémentaires ?
Le point de terminaison AD FS UserInfo retourne toujours la revendication d’objet telle qu’elle est spécifiée dans les normes OpenID. AD FS ne fournit pas de revendications supplémentaires demandées via le point de terminaison UserInfo. Si vous avez besoin de revendications supplémentaires dans le jeton d’ID, reportez-vous à [jetons d’ID personnalisés dans AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>Pourquoi est-ce que je vois un grand nombre d’erreurs 1021 sur mes serveurs AD FS ?
Cet événement est généralement consigné pour un accès aux ressources non valide sur AD FS pour la ressource 00000003-0000-0000-C000-000000000000. Cette erreur est due à un comportement incorrect du client lorsqu’il tente d’obtenir un jeton d’accès pour le service Azure AD Graph. Étant donné que la ressource n’est pas présente sur AD FS, l’ID d’événement 1021 sur les serveurs AD FS est obtenu. Il est possible d’ignorer sans risque les avertissements ou les erreurs de la ressource 00000003-0000-0000-C000-000000000000 sur AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Pourquoi est-ce que je vois un avertissement indiquant que l’échec de l’ajout du compte de service AD FS au groupe administrateurs de clés d’entreprise ?
Ce groupe est créé uniquement lorsqu’un contrôleur de domaine Windows 2016 avec le rôle de contrôleur de domaine principal FSMO existe dans le domaine. Pour résoudre l’erreur, vous pouvez créer le groupe manuellement et suivre les ci-dessous pour accorder l’autorisation requise après avoir ajouté le compte de service en tant que membre du groupe.
1.  Ouvrez **Utilisateurs et ordinateurs Active Directory**.
2.  Dans le volet de navigation, cliquez **avec le bouton droit sur** votre nom de domaine, puis **cliquez sur** propriétés.
3.  **Cliquez sur** Sécurité (si l’onglet sécurité est manquant, activez les fonctionnalités avancées dans le menu Affichage).
4.  **Cliquez sur** Détaillées. **Cliquez sur** Complémentaires. **Cliquez sur** Sélectionnez un principal.
5.  La boîte de dialogue Sélectionner un utilisateur, un ordinateur, un compte de service ou un groupe s’affiche.  Dans la zone de texte Entrez le nom de l’objet à sélectionner, tapez Key admin Group.  Cliquez sur OK.
6.  Dans la zone de liste s’applique à, sélectionnez **objets utilisateur descendants**.
7.  À l’aide de la barre de défilement, faites défiler vers le bas de la page et **cliquez sur** effacer tout.
8.  Dans la section **Propriétés** , sélectionnez **Lire MSDS-KeyCredentialLink** et **écrire MSDS-KeyCredentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Pourquoi l’authentification moderne des appareils Android échoue-t-elle si le serveur n’envoie pas tous les certificats intermédiaires de la chaîne avec le certificat SSL ?

Les utilisateurs fédérés peuvent rencontrer une authentification auprès d’Azure AD pour les applications qui utilisent la bibliothèque ADAL Android qui échouent. L’application obtiendra un **AuthenticationException** lorsqu’il tentera d’afficher la page de connexion. Dans Chrome, la page de connexion AD FS peut être appelée non sécurisée.

Android-sur toutes les versions et tous les appareils : ne prend pas en charge le téléchargement de certificats supplémentaires à partir du champ **authorityInformationAccess** du certificat. C’est également le cas du navigateur Chrome. Tout certificat d’authentification serveur qui ne contient pas de certificats intermédiaires entraîne cette erreur si l’intégralité de la chaîne de certificats n’est pas transmise à partir de AD FS.

Une solution appropriée à ce problème consiste à configurer le AD FS et les serveurs WAP pour envoyer les certificats intermédiaires nécessaires avec le certificat SSL.

Lorsque vous exportez le certificat SSL d’un ordinateur à importer dans le magasin personnel de l’ordinateur, du AD FS et du ou des serveurs WAP, veillez à exporter la clé privée et à sélectionner **échange d’informations personnelles-PKCS #12**.

Il est important que la case à cocher pour **inclure tous les certificats dans le chemin d’accès du certificat si possible** est activée, ainsi que pour **exporter toutes les propriétés étendues**.  

Exécutez certlm. msc sur les serveurs Windows et importez le *. PFX dans le magasin de certificats personnel de l’ordinateur. Ainsi, le serveur transmet la totalité de la chaîne de certificats à la bibliothèque ADAL.

>[!NOTE]
> Le magasin de certificats des équilibreurs de charge réseau doit également être mis à jour pour inclure l’intégralité de la chaîne de certificats, le cas échéant.

### <a name="does-ad-fs-support-head-requests"></a>AD FS prend-il en charge les demandes HEAD ?
AD FS ne prend pas en charge les demandes HEAD.  LES applications ne doivent pas utiliser de demandes HEAD sur AD FS points de terminaison.  Cela peut entraîner des réponses d’erreur HTTP inattendues et/ou différées.  En outre, des événements d’erreur inattendus peuvent s’afficher dans le journal des événements AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Pourquoi ne vois-je pas un jeton d’actualisation lorsque je me connecte avec un fournisseur d’identité distant ?
Aucun jeton d’actualisation n’est émis si le jeton émis par IdP a une Validty inférieure à 1 heure. Pour garantir l’émission d’un jeton d’actualisation, augmentez la validité du jeton émis par le fournisseur d’identité (IdP) à plus de 1 heure.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>Avons-nous un moyen de modifier l’algorithme de chiffrement de jeton de RP ?
Par défaut, le chiffrement du jeton RP est défini sur AES256 et ne peut pas être remplacé par une autre valeur.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>Dans une batterie en mode mixte, j’obtiens une erreur lors de la tentative de définition du nouveau certificat SSL à l’aide de Set-AdfsSslCertificate-empreinte. Comment puis-je mettre à jour le certificat SSL dans une batterie de serveurs AD FS en mode mixte ?
Les batteries de AD FS en mode mixte sont censées être un état de transition. Dans le cadre de votre planification, il est recommandé de remplacer le certificat SSL avant le processus de mise à niveau ou de terminer le processus et d’augmenter le niveau de comportement de la batterie de serveurs avant de mettre à jour le certificat SSL. Si ce n’est pas le cas, les instructions ci-dessous fournissent la possibilité de mettre à jour le certificat SSL. 

Sur les serveurs WAP, vous pouvez toujours utiliser SET-WebApplicationProxySslCertificate. Sur les serveurs ADFS, vous devez utiliser Netsh. Suivez les étapes indiquées ci-dessous :

1. Sélectionner le sous-ensemble de serveurs ADFS 2016 pour la maintenance (par exemple, supprimer de l’équilibreur de charge)
2. Sur les serveurs sélectionnés dans #1, importez le nouveau certificat via MMC.
3. Supprimer les certificats existants

    a. netsh http Delete sslcert hostnameport = FS. contoso. com : 443 b. netsh http Delete sslcert hostnameport = localhost : 443 c. netsh http Delete sslcert hostnameport = FS. contoso. com : 49443

4.  Ajouter le nouveau certificat

    a. netsh http add sslcert hostnameport = FS. contoso. com : 443 certhash = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = MY SslCtlStoreName = AdfsTrustedDevices

    b. netsh http add sslcert hostnameport = localhost : 443 certhash = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = MY SslCtlStoreName = AdfsTrustedDevices

    c. netsh http add sslcert hostnameport = FS. contoso. com : 49443 certhash = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = MY SslCtlStoreName = AdfsTrustedDevices

5. Redémarrer le service ADFS sur le serveur sélectionné
6. Supprimer le sous-ensemble de serveurs WAP pour la maintenance
7. Sur les serveurs WAP sélectionnés, importez le nouveau certificat via MMC.
8. Définir le nouveau certificat sur WAP à l’aide de l’applet de commande

    a. Set-WebApplicationProxySslCertificate-empreinte numérique « CERTTHUMBPRINT »

9. Redémarrer le service sur les serveurs WAP sélectionnés
10. Replacez le WAP sélectionné et les serveurs de AD FS dans l’environnement de production.

Effectuez la mise à jour sur le reste des AD FS et des serveurs WAP de la même manière.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>ADFS est-il pris en charge lorsque les serveurs proxy d’application Web (WAP) se trouvent derrière le pare-feu d’applications Web Azure (WAF) ?
Les serveurs d’applications Web et ADFS prennent en charge tous les pare-feu qui n’effectuent pas d’arrêt SSL sur le point de terminaison. En outre, les serveurs ADFS/WAP disposent de mécanismes intégrés pour empêcher les attaques Web courantes, telles que les scripts inter-sites, le proxy ADFS et satisfont à toutes les exigences définies par le [protocole MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>Je vois un « événement 441 : un jeton avec une clé de liaison de jeton incorrecte a été trouvé ». Que dois-je faire pour résoudre ce cas ?
Dans AD FS 2016, la liaison de jeton est automatiquement activée et provoque plusieurs problèmes connus avec les scénarios de proxy et de Fédération qui génèrent cette erreur. Pour résoudre ce code, exécutez la commande PowerShell suivante et supprimez la prise en charge de la liaison de jeton.

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>J’ai mis à niveau ma batterie depuis AD FS dans Windows Server 2016 vers AD FS dans Windows Server 2019. Le niveau de comportement de la batterie de serveurs AD FS a été augmenté avec succès à 2019, mais la configuration du proxy d’application Web est toujours affichée en tant que Windows Server 2016 ?
Après une mise à niveau vers Windows Server 2019, la version de configuration du proxy d’application Web continuera à s’afficher en tant que Windows Server 2016. Le proxy d’application Web ne dispose pas de nouvelles fonctionnalités spécifiques à la version de Windows Server 2019, et si le niveau de comportement de la batterie de serveurs a été correctement déclenché sur AD FS, le proxy d’application Web continue à s’afficher en tant que Windows Server 2016 par défaut. 
