---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: FAQ AD FS
description: Forum aux questions pour AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a1041bdc189238c7da32896e6f867f730e392d24
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80814429"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>Forum aux questions (FAQ) sur AD FS


La documentation suivante est un forum aux questions concernant les services de fédération Active Directory.  Le document comporte plusieurs parties correspondant à différents types de questions.

## <a name="deployment"></a>Déploiement

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Comment puis-je mettre à niveau/migrer à partir de versions précédentes d’AD FS ?
Vous pouvez mettre à niveau AD FS à l’aide de l’une des options suivantes :


- Windows Server 2012 R2 AD FS vers Windows Server 2016 AD FS ou version ultérieure. Notez que la méthodologie est la même si vous effectuez une mise à niveau de Windows Server 2016 AD FS vers Windows Server 2019 AD FS. 
    - [Mise à niveau vers AD FS dans Windows Server 2016 à l’aide d’une base de données WID](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Mise à niveau vers AD FS dans Windows Server 2016 à l’aide d’une base de données SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS vers Windows Server 2012 R2 AD FS
    - [Migrer vers AD FS sur Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 vers Windows Server 2012 AD FS
    - [Migrer vers AD FS sur Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x pour AD FS 2.0
    - [Mise à niveau d’AD FS 1.x vers AD FS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Si vous devez effectuer une mise à niveau à partir d’AD FS 2.0 ou 2.1 (Windows Server 2008 R2 ou Windows Server 2012), vous devez utiliser les scripts intégrés (situés dans C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Pourquoi l’installation d’AD FS nécessite-t-elle un redémarrage du serveur ?

La prise en charge de HTTP/2 a été ajoutée dans Windows Server 2016, mais HTTP/2 ne peut pas être utilisé pour l’authentification par certificat client.  Étant donné que de nombreux scénarios AD FS utilisent l’authentification par certificat client, et qu’un nombre significatif de clients ne prennent pas en charge les demandes de nouvelle tentative à l’aide de HTTP/1.1, la configuration de la batterie de serveurs AD FS reconfigure les paramètres HTTP du serveur local sur HTTP/1.1.  Cela nécessite un redémarrage du serveur.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>L’utilisation de serveurs WAP Windows 2016 pour publier la batterie de serveurs AD FS sur Internet sans mettre à niveau la batterie AD FS back-end est-elle prise en charge ?
Oui, cette configuration est prise en charge, mais aucune nouvelle fonctionnalité AD FS 2016 ne serait prise en charge dans cette configuration.  Cette configuration est censée être temporaire au cours de la phase de migration d’AD FS 2012 R2 vers AD FS 2016, et ne doit pas être déployée pour une durée prolongée.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>Est-il possible de déployer AD FS pour Office 365 sans publier de proxy sur Office 365 ?
Oui, ceci est pris en charge. Toutefois, il existe un effet secondaire.

1. Vous devrez gérer manuellement la mise à jour des certificats de signature de jetons, car Azure AD ne pourra pas accéder aux métadonnées de fédération. Pour plus d’informations sur la mise à jour manuelle du certificat de signature de jetons, consultez [Renouvellement des certificats de fédération pour Office 365 et Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).
2. Vous ne pourrez pas tirer parti des flux d’authentification hérités (par exemple le flux d’authentification proxy ExO).

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>Quelles sont les exigences en matière d’équilibrage de charge pour les serveurs AD FS et WAP ?

AD FS est un système sans état. Par conséquent, l’équilibrage de charge est relativement simple pour les connexions. Voici les principales recommandations pour les systèmes d’équilibrage de charge.


- Les équilibreurs de charge ne DOIVENT PAS être configurés avec l’affinité IP. Cela peut imposer une charge excessive sur un sous-ensemble de vos serveurs dans certains scénarios Exchange Online.
- Les équilibreurs de charge ne DOIVENT PAS mettre fin aux connexions HTTPS et relancer une nouvelle connexion au serveur AD FS.
- Les équilibreurs de charge équilibreurs s’assurer que l’adresse IP de connexion soit traduite en tant qu’adresse IP source dans le paquet HTTP lors de son envoi à AD FS. Dans le cas où un équilibreur de charge ne peut pas envoyer l’adresse IP source dans le paquet HTTP, il DOIT ajouter l’adresse IP à l’en-tête x-forwarded-for, ou l’ajouter à l’en-tête s’il existe déjà. Cela est nécessaire pour la gestion correcte de certaines fonctionnalités liées à IP (adresse IP interdite, Extranet Smart Lockout, etc.), et peut nuire à la sécurité en cas de configuration incorrecte.
- Les équilibreurs de charge DOIVENT prendre en charge SNI. Si ce n’est pas le cas, vérifiez qu’AD FS est configuré pour créer des liaisons HTTPS pour gérer les clients non-SNI.
- Les équilibreurs de charge DOIVENT utiliser le point de terminaison de sonde d’intégrité HTTP AD FS pour détecter si les serveurs AD FS ou WAP sont en cours d’exécution, et les exclure si une réponse 200 OK n’est pas retournée.

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>Quelles sont les configurations à forêts multiples prises en charge par AD FS ?

AD FS prend en charge plusieurs configurations à forêts multiples, et s’appuie sur le réseau de confiance AD DS sous-jacent pour authentifier les utilisateurs parmi plusieurs domaines approuvés. Nous recommandons vivement les approbations de forêts bidirectionnelles, car il s’agit d’une configuration plus simple pour garantir que le sous-système d’approbation fonctionne correctement sans problème. En outre :

- Dans le cas d’une approbation de forêt unidirectionnelle telle qu’une forêt DMZ contenant des identités de partenaires, nous vous recommandons de déployer AD FS dans la forêt d’entreprise et de traiter la forêt DMZ comme une autre approbation de fournisseur de revendications locale connectée par le biais de LDAP. Dans ce cas, l’authentification Windows intégrée ne fonctionnera pas pour les utilisateurs de la forêt DMZ, et ils devront procéder à une authentification par mot de passe, car il s’agit du seul mécanisme pris en charge pour LDAP. Si vous ne pouvez pas adopter cette approche, vous devez configurer un autre service AD FS dans la forêt DMZ et l’ajouter en tant qu’approbation de fournisseur de revendications dans AD FS dans la forêt d’entreprise. Les utilisateurs devront effectuer une découverte de domaine d’accueil, mais l’authentification Windows intégrée et l’authentification par mot de passe fonctionneront. Apportez les modifications appropriées dans les règles d’émission dans AD FS dans la forêt DMZ, car AD FS dans la forêt d’entreprise ne pourra pas obtenir d’informations utilisateur supplémentaires sur l’utilisateur à partir de la forêt DMZ.
- Bien que les approbations de niveau domaine soient prises en charge et puissent fonctionner, nous vous recommandons vivement de passer à un modèle d’approbation de niveau forêt. De plus, vous devez vous assurer que le routage UPN et la résolution de noms NETBIOS fonctionnent correctement.

>[!NOTE]  
>Si l’authentification sélective est utilisée avec une configuration d’approbation bidirectionnelle, vérifiez que l’utilisateur appelant dispose de l’autorisation « Autoriser l’authentification » sur le compte de service cible. 


## <a name="design"></a>Conception

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Quels sont les fournisseurs d’authentification multifacteur tiers disponibles pour AD FS ?
AD FS fournit un mécanisme extensible pour l’intégration des fournisseurs MFA tiers. Il n’existe pas de programme de certification défini pour cela. Il est supposé que le fournisseur a effectué les validations nécessaires avant la mise en service. 

La liste des fournisseurs qui ont notifié Microsoft est publiée sur le site [Fournisseurs MFA pour AD FS](../operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).  Il se peut toujours qu’il y ait des fournisseurs disponibles dont nous n’avons pas connaissance ; nous mettrons à jour la liste au fur et à mesure de leur découverte.

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>Les proxys tiers sont-ils pris en charge avec AD FS ?
Oui, des proxys tiers peuvent être placés devant le proxy d’application Web, mais tout proxy tiers doit prendre en charge le [protocole MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) à utiliser à la place du proxy d’application Web.

Vous trouverez ci-dessous la liste des fournisseurs tiers dont nous avons connaissance.  Il se peut toujours qu’il y ait des fournisseurs disponibles dont nous n’avons pas connaissance ; nous mettrons à jour la liste au fur et à mesure de leur découverte.

- [Gestionnaire de stratégie d’accès F5](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Où se trouve la feuille de calcul sur la planification de capacité pour AD FS 2016 ?
Vous pouvez télécharger la version AD FS 2016 de la feuille de calcul [ici](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Vous pouvez également l’utiliser pour AD FS dans Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Comment puis-je m’assurer que mes serveurs AD FS et WAP prennent en charge les exigences ATP d’Apple ?

Apple a publié un ensemble de spécifications appelées App Transport Security (ATS), qui peuvent avoir un impact sur les appels des applications iOS qui s’authentifient auprès d’AD FS.  Vous pouvez vous assurer que vos serveurs AD FS et WAP sont conformes en faisant en sorte qu’ils prennent en charge les [exigences pour la connexion à l’aide d’ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
Vous devez notamment vérifier que vos serveurs AD FS et WAP prennent en charge TLS 1.2, et que la suite de chiffrement négociée de la connexion TLS prendra en charge PFS (Perfect Forward Secrecy).

Vous pouvez activer et désactiver SSL 2.0 et 3.0 et les versions TLS 1.0, 1.1 et 1.2 à l’aide de la procédure décrite dans [Gérer les protocoles SSL dans AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Pour vous assurer que vos serveurs AD FS et WAP négocient uniquement les suites de chiffrement TLS qui prennent en charge ATP, vous pouvez désactiver toutes les suites de chiffrement qui ne figurent pas dans la [liste des suites de chiffrement compatibles ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Pour ce faire, utilisez les [applets de commande PowerShell Windows TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index).

## <a name="developer"></a>Développeur

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>Lors de la génération d’un id_token avec AD FS pour un utilisateur authentifié auprès d’AD, comment la revendication « sub » est-elle générée dans l’id_token ?
La valeur de la revendication « sub » est le hachage de la valeur de l’ID client et de la revendication d’ancrage.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>Quelle est la durée de vie du jeton d’actualisation/jeton d’accès quand l’utilisateur se connecte par le biais d’une approbation de fournisseur de revendications distante sur WS-Fed/SAML-P ?
La durée de vie du jeton d’actualisation correspond à la durée de vie du jeton qu’ADFS a obtenu auprès de l’approbation du fournisseur de revendications distante. La durée de vie du jeton d’accès correspond à la durée de vie du jeton de la partie de confiance pour laquelle le jeton d’accès est émis.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>J’ai besoin de retourner des étendues de profil et de messagerie, en plus de l’étendue OpenId. Puis-je obtenir des informations supplémentaires à l’aide des étendues ? Comment procéder dans AD FS ?
Vous pouvez utiliser un id_token personnalisé pour ajouter des informations pertinentes dans l’id_token lui-même. Pour plus d’informations, consultez l’article [Personnaliser les revendications à émettre dans id_token](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>Comment émettre des objets blob JSON dans des jetons JWT ?
Un ValueType spécial (« <http://www.w3.org/2001/XMLSchema#json> ») et un caractère d’échappement (\x22) pour cela ont été ajoutés à AD FS 2016. Consultez l’exemple ci-dessous pour connaître la règle d’émission et la sortie finale du jeton d’accès.

Exemple de règle d’émission :

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Revendication émise dans le jeton d’accès :

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>Puis-je transmettre une valeur de ressource dans le cadre de la valeur d’étendue, comme pour les requêtes exécutées sur Azure AD ?
Avec AD FS sur Server 2019, vous pouvez désormais transmettre la valeur de ressource incorporée dans le paramètre d’étendue. Le paramètre d’étendue peut maintenant être organisé sous forme de liste séparée par des espaces, où chaque entrée est structurée en tant que ressource/étendue. Par exemple :  
**< create a valid sample request>**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS prend-il en charge l’extension PKCE ?
AD FS dans Server 2019 prend en charge PKCE (Proof Key for Code Exchange) pour le flux d’octroi de codes d’autorisation OAuth.

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>Quelles sont les étendues autorisées prises en charge par AD FS ?
- aza : si vous utilisez les [extensions de protocole OAuth 2.0 pour les clients du Service Broker](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) et si le paramètre d’étendue contient l’étendue « aza », le serveur émet un nouveau jeton d’actualisation principal et le définit dans le champ refresh_token de la réponse, et il définit le champ refresh_token_expires_in sur la durée de vie du nouveau jeton d’actualisation principal, le cas échéant.
- OpenID : autorise l’application à demander l’utilisation du protocole d’autorisation OpenID Connect.
- logon_cert : l’étendue logon_cert autorise une application à demander des certificats d’ouverture de session, qui peuvent être utilisés par des utilisateurs authentifiés pour ouvrir une session de manière interactive. Le serveur AD FS omet le paramètre access_token de la réponse, et fournit à la place une chaîne de certificats CMS encodée en base64 ou une réponse PKI complète CMC. Vous trouverez des détails supplémentaires [ici](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation : l’étendue user_impersonation est nécessaire pour demander un jeton d’accès « On-Behalf-Of » à AD FS. Pour plus d’informations sur l’utilisation de cette étendue, consultez [Créer une application à plusieurs niveaux avec OBO (On-Behalf-Of) à l’aide d’OAuth avec AD FS 2016 ou version ultérieure](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert : l’étendue vpn_cert autorise une application à demander des certificats VPN, qui peuvent être utilisés pour établir des connexions VPN à l’aide de l’authentification EAP-TLS. Ceci n’est plus pris en charge.
- e-mail : autorise l’application à demander une revendication d’e-mail pour l’utilisateur connecté. Ceci n’est plus pris en charge. 
- profile : autorise l’application à demander des revendications liées au profil pour l’utilisateur connecté. Ceci n’est plus pris en charge. 


## <a name="operations"></a>Opérations

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Comment remplacer le certificat SSL pour AD FS ?
Le certificat SSL AD FS n’est pas identique au certificat de communication du service AD FS qui se trouve dans le composant logiciel enfichable Gestion AD FS.  Pour changer le certificat SSL AD FS, vous devez utiliser PowerShell. Suivez les instructions de l’article ci-dessous :

[Gestion des certificats SSL dans AD FS et WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Comment activer ou désactiver les paramètres TLS/SSL pour AD FS ?
Pour désactiver ou activer les protocoles SSL et les suites de chiffrement, suivez les instructions fournies dans cet article :

[Gérer les protocoles SSL dans AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Le certificat SSL proxy doit-il être identique au certificat SSL AD FS ?  
Utilisez les instructions suivantes en ce qui concerne le certificat SSL proxy et le certificat SSL AD FS :


- Si le proxy est utilisé pour les requêtes AD FS qui utilisent l’authentification Windows intégrée, le certificat SSL proxy doit être identique (il doit utiliser la même clé) au certificat SSL du serveur de fédération.
- Si la propriété AD FS « ExtendedProtectionTokenCheck » est activée (paramètre par défaut dans AD FS), le certificat SSL proxy doit être identique (il doit utiliser la même clé) au certificat SSL du serveur de fédération.
- Dans le cas contraire, le certificat SSL proxy peut avoir une clé différente de celle du certificat SSL AD FS, mais il doit répondre aux mêmes [exigences](../overview/AD-FS-2016-Requirements.md).

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>Pourquoi je ne vois qu’une connexion par mot de passe sur AD FS, et pas les autres méthodes d’authentification que j’ai configurées ? 
AD FS n’affiche qu’une seule méthode d’authentification sur l’écran de connexion quand l’application exige explicitement un URI d’authentification spécifique qui correspond à une méthode d’authentification configurée et activée. Ceci est spécifié par le paramètre « wauth » pour les requêtes WS-Federation et le paramètre « RequestedAuthnCtxRef » dans une requête de protocole SAML. Par conséquent, seule la méthode d’authentification demandée est affichée (par exemple, connexion par mot de passe).

Quand AD FS est utilisé avec Azure AD, il est courant que les applications envoient le paramètre prompt=login à Azure AD. Par défaut, Azure AD traduit cela en demande de nouvelle connexion basée sur un mot de passe à AD FS. Il s’agit de la cause la plus courante de l’affichage d’une connexion par mot de passe dans AD FS sur votre réseau, ou du non-affichage d’une option permettant de se connecter avec votre certificat. Cela peut être facilement corrigé en modifiant les paramètres du domaine fédéré dans Azure AD. 

Pour plus d’informations sur la configuration de ce paramètre, consultez [Prise en charge du paramètre prompt=login dans les services de fédération Active Directory (AD FS)](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-change-the-ad-fs-service-account"></a>Comment changer le compte de service AD FS ?
Pour changer le compte de service AD FS, suivez les instructions à l’aide du [module PowerShell de compte de service](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule) de la boîte à outils AD FS.

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Comment configurer des navigateurs pour utiliser l’authentification Windows intégrée avec AD FS ?

Pour plus d’informations sur la façon de configurer des navigateurs, consultez [Configurer des navigateurs pour utiliser l’authentification Windows intégrée avec AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-turn-off-browserssoenabled"></a>Puis-je désactiver BrowserSsoEnabled ?
Si vous n’avez pas de stratégies de contrôle d’accès basées sur l’appareil dans AD FS ou d’inscription de certificats Windows Hello Entreprise avec AD FS, vous pouvez désactiver BrowserSsoEnabled. BrowserSsoEnabled permet à AD FS de collecter un jeton d’actualisation principal (PRT, Primary Refresh Token) auprès du client, qui contient des informations sur l’appareil. Sans cette authentification d’appareil, AD FS ne fonctionnera pas sur les appareils Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>Quelle est la durée de validité des jetons AD FS ?

Souvent, cette question signifie « pendant combien de temps les utilisateurs disposent-ils de l’authentification unique (SSO) sans avoir à entrer de nouvelles informations d’identification, et comment, en tant qu’administrateur, puis-je contrôler cette durée ? ».  Ce comportement, ainsi que les paramètres de configuration qui le contrôlent, sont décrits dans l’article [Paramètres d’authentification unique AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Les durées de vie par défaut des différents cookies et jetons sont listées ci-dessous (ainsi que les paramètres qui régissent les durées de vie) :

**Appareils inscrits**

- Cookies PRT et SSO : 90 jours maximum, durée régie par PSSOLifeTimeMins. (L’appareil fourni est utilisé au moins tous les 14 jours, ce qui est contrôlé par DeviceUsageWindow)

- Jeton d’actualisation : durée calculée en fonction de la durée ci-dessus, pour fournir un comportement cohérent

- access_token : une heure par défaut, en fonction de la partie de confiance

- id_token : identique au jeton d’accès

**Appareils non inscrits**

- Cookies SSO : huit heures par défaut, durée régie par SSOLifetimeMins.  Quand l’option Maintenir la connexion est activée, la valeur par défaut est de 24 heures, et elle peut être configurée par le biais de KMSILifetimeMins.


- Jeton d’actualisation : huit heures par défaut. 24 heures avec l’option Maintenir la connexion activée

- access_token : une heure par défaut, en fonction de la partie de confiance

- id_token : identique au jeton d’accès

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS prend-il en charge la sécurité HSTS (HTTP Strict Transport Security) ?  

La sécurité HSTS est un mécanisme de stratégie de sécurité web qui aide à atténuer les attaques par rétrogradation de protocole et le piratage de cookies pour les services qui ont des points de terminaison HTTP et HTTPS. Elle permet aux serveurs web de déclarer que les navigateurs web (ou d’autres agents utilisateur conformes) doivent interagir avec eux uniquement à l’aide de HTTPS et jamais par le biais du protocole HTTP.

Tous les points de terminaison AD FS pour le trafic d’authentification web sont ouverts exclusivement sur HTTPS.  Ainsi, AD FS atténue efficacement les menaces créées par le mécanisme de stratégie HSTS (par conception, il n’existe pas de rétrogradation vers HTTP, car il n’y a aucun écouteur dans HTTP). En outre, AD FS empêche les cookies d’être envoyés à un autre serveur avec des points de terminaison de protocole HTTP en marquant tous les cookies avec l’indicateur sécurisé.

Ainsi, l’implémentation de HSTS sur un serveur AD FS n’est pas nécessaire, car aucune rétrogradation n’est possible.  À des fins de conformité, les serveurs AD FS répondent à ces exigences, car ils ne peuvent jamais utiliser le protocole HTTP et tous les cookies sont marqués comme sécurisés.

De plus, AD FS 2016 (avec les correctifs les plus récents) et AD FS 2019 prend en charge l’émission de l’en-tête HSTS. Pour configurer cela, consultez [Personnaliser des en-têtes de réponse de sécurité HTTP avec AD FS](../operations/customize-http-security-headers-ad-fs.md).

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>x-ms-forwarded-client-ip ne contient pas l’adresse IP du client, mais contient l’adresse IP du pare-feu devant le proxy. Où trouver la bonne adresse IP du client ?
Il n’est pas recommandé d’effectuer une terminaison SSL avant le WAP. Si la terminaison SSL est effectuée devant le WAP, x-ms-forwarded-client-ip contient l’adresse IP du périphérique réseau devant le WAP. Vous trouverez ci-dessous une brève description des différentes revendications IP prises en charge par AD FS :
 - x-ms-client-ip : adresse IP réseau du périphérique qui se connecte au STS.  Dans le cas d’une requête extranet, cette revendication contient toujours l’adresse IP du WAP.
 - x-ms-forwarded-client-ip : revendication à valeurs multiples qui contient toutes les valeurs transmises à AD FS par Exchange Online, ainsi que l’adresse IP du périphérique qui s’est connecté au WAP.
 - Userip : pour les requêtes extranet, cette revendication contient la valeur de x-ms-forwarded-client-ip.  Pour les requêtes intranet, cette revendication contient la même valeur que x-ms-client-ip.

 De plus, AD FS 2016 (avec les correctifs les plus récents) et versions ultérieures prennent également en charge la capture de l’en-tête x-forwarded-for. Tout équilibreur de charge ou périphérique réseau qui ne transfère pas à la couche 3 (l’adresse IP est conservée) doit ajouter l’adresse IP cliente entrante à l’en-tête x-forwarded-for standard du secteur. 

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>J’essaie d’obtenir des revendications supplémentaires sur le point de terminaison des informations utilisateur, mais il ne retourne que l’objet. Comment obtenir des revendications supplémentaires ?
Le point de terminaison UserInfo AD FS retourne toujours la revendication d’objet comme spécifié dans les normes OpenID. AD FS ne fournit pas de revendications supplémentaires demandées par le biais du point de terminaison UserInfo. Si vous avez besoin de revendications supplémentaires dans le jeton d’ID, consultez [Jetons d’ID personnalisés dans AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>Pourquoi est-ce que je reçois un grand nombre d’erreurs 1021 sur mes serveurs AD FS ?
Cet événement concerne généralement un accès aux ressources non valide sur AD FS pour la ressource 00000003-0000-0000-C000-000000000000. Cette erreur est due à un comportement incorrect du client quand il tente d’obtenir un jeton d’accès pour le service Graph Azure AD. La ressource n’étant pas présente sur AD FS, cela génère l’ID d’événement 1021 sur les serveurs AD FS. Vous pouvez sans danger ignorer les avertissements ou les erreurs de la ressource 00000003-0000-0000-C000-000000000000 sur AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Pourquoi je reçois un avertissement indiquant l’échec de l’ajout du compte de service AD FS au groupe Administrateurs clés Enterprise ?
Ce groupe est créé uniquement quand il existe un contrôleur de domaine Windows 2016 détenant le rôle de contrôleur de domaine principal FSMO dans le domaine. Pour résoudre l’erreur, vous pouvez créer le groupe manuellement et suivre les instructions ci-dessous pour accorder l’autorisation requise après avoir ajouté le compte de service en tant que membre du groupe.
1.    Ouvrez **Utilisateurs et ordinateurs Active Directory**.
2.    **Cliquez avec le bouton droit** sur votre nom de domaine dans le volet de navigation, puis cliquez sur **Propriétés**.
3.    **Cliquez** sur Sécurité (si l’onglet Sécurité est manquant, activez les fonctionnalités avancées dans le menu Affichage).
4.    **Cliquez** sur Paramètres avancés. **Cliquez** sur Ajouter. **Cliquez** sur Sélectionnez un principal.
5.    La boîte de dialogue Sélectionner un utilisateur, un ordinateur, un compte de service ou un groupe s’ouvre.  Dans la zone de texte Entrez le nom de l’objet à sélectionner, tapez Administrateurs clés Enterprise.  Cliquez sur OK.
6.    Dans la zone de liste S’applique à, sélectionnez **Objets utilisateur descendants**.
7.    À l’aide de la barre de défilement, effectuez un défilement vers le bas de la page, puis **cliquez** sur Effacer tout.
8.    Dans la section **Propriétés**, sélectionnez **Read msDS-KeyCredentialLink** et **Write msDS-KeyCredentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Pourquoi l’authentification moderne des appareils Android échoue si le serveur n’envoie pas tous les certificats intermédiaires de la chaîne avec le certificat SSL ?

Les utilisateurs fédérés peuvent subir un échec d’authentification auprès d’Azure AD pour les applications qui utilisent la bibliothèque ADAL Android. L’application lève une **AuthenticationException** quand elle tente d’afficher la page de connexion. Dans Chrome, la page de connexion AD FS peut être appelée comme non sécurisée.

Android (sur toutes les versions et tous les appareils) ne prend pas en charge le téléchargement de certificats supplémentaires à partir du champ **authorityInformationAccess** du certificat. C’est également le cas du navigateur Chrome. Tout certificat d’authentification serveur qui ne contient pas de certificats intermédiaires génère cette erreur si la chaîne de certificats entière n’est pas transmise à partir d’AD FS.

Une solution appropriée à ce problème consiste à configurer les serveurs WAP et AD FS pour envoyer les certificats intermédiaires nécessaires avec le certificat SSL.

Quand vous exportez le certificat SSL à partir d’un ordinateur en vue d’une importation dans le magasin personnel de l’ordinateur du ou des serveurs WAP et AD FS, veillez à exporter la clé privée et à sélectionner **Échange d’informations personnelles - PKCS #12**.

Il est important que la case **Inclure tous les certificats dans le chemin du certificat si possible** soit cochée, ainsi que **Exporter toutes les propriétés étendues**.  

Exécutez certlm.msc sur les serveurs Windows et importez le fichier *.PFX dans le magasin de certificats personnel de l’ordinateur. Ainsi, le serveur transmettra la chaîne de certificats entière à la bibliothèque ADAL.

>[!NOTE]
> Le magasin de certificats des équilibreurs de charge réseau doit également être mis à jour pour inclure la chaîne de certificats entière, le cas échéant.

### <a name="does-ad-fs-support-head-requests"></a>AD FS prend-il en charge les requêtes HEAD ?
AD FS ne prend pas en charge les requêtes HEAD.  Les applications ne doivent pas exécuter de requêtes HEAD sur des points de terminaison AD FS.  Cela peut entraîner des réponses d’erreur HTTP inattendues et/ou différées.  En outre, des événements d’erreur inattendus peuvent être consignés dans le journal des événements AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Pourquoi est-ce que je ne vois pas de jeton d’actualisation quand je me connecte avec un fournisseur d’identité distant ?
Aucun jeton d’actualisation n’est émis si le jeton émis par le fournisseur d’identité a une validité inférieure à une heure. Pour garantir l’émission d’un jeton d’actualisation, augmentez la validité du jeton émis par le fournisseur d’identité à plus d’une heure.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>Existe-t-il un moyen de modifier l’algorithme de chiffrement de jeton RP ?
Par défaut, le chiffrement du jeton RP est défini sur AES256 et ne peut pas être remplacé par une autre valeur.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>Dans une batterie en mode mixte, j’obtiens une erreur quand j’essaie de définir le nouveau certificat SSL à l’aide de Set-AdfsSslCertificate -Thumbprint. Comment puis-je mettre à jour le certificat SSL dans une batterie de serveurs AD FS en mode mixte ?
Les batteries AD FS en mode mixte sont censées être un état de transition. Dans le cadre de votre planification, nous vous recommandons de remplacer le certificat SSL avant le processus de mise à niveau ou de terminer le processus et d’augmenter le niveau de comportement de la batterie de serveurs avant de mettre à jour le certificat SSL. Si cela n’a pas été fait, les instructions ci-dessous fournissent la possibilité de mettre à jour le certificat SSL. 

Sur les serveurs WAP, vous pouvez toujours utiliser Set-WebApplicationProxySslCertificate. Sur les serveurs AD FS, vous devez utiliser netsh. Suivez les étapes indiquées ci-dessous :

1. Sélectionnez le sous-ensemble de serveurs AD FS 2016 pour la maintenance (par exemple, supprimez-le de l’équilibreur de charge).
2. Sur les serveurs sélectionnés à l’étape 1, importez le nouveau certificat par le biais de la console MMC.
3. Supprimez les certificats existants.

    a. netsh http delete sslcert hostnameport=fs.contoso.com:443 b. netsh http delete sslcert hostnameport=localhost:443 c. netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  Ajoutez le nouveau certificat.

    a. netsh http add sslcert hostnameport=fs.contoso.com:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    b. netsh http add sslcert hostnameport=localhost:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    c. netsh http add sslcert hostnameport=fs.contoso.com:49443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

5. Redémarrez le service AD FS sur le serveur sélectionné.
6. Supprimez le sous-ensemble de serveurs WAP pour la maintenance.
7. Sur les serveurs WAP sélectionnés, importez le nouveau certificat par le biais de la console MMC.
8. Définissez le nouveau certificat sur WAP à l’aide de l’applet de commande.

    a. Set-WebApplicationProxySslCertificate -Thumbprint " CERTTHUMBPRINT"

9. Redémarrez le service sur les serveurs WAP sélectionnés.
10. Replacez les serveurs AD FS et WAP sélectionnés dans l’environnement de production.

Effectuez la mise à jour sur le reste des serveurs AD FS et WAP de la même manière.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>AD FS est-il pris en charge quand les serveurs Proxy d’application Web (WAP, Web Application Proxy) se trouvent derrière le pare-feu d’applications web Azure (WAF, Web Application Firewall) ?
Les serveurs d’application web et AD FS prennent en charge tout pare-feu qui n’effectue pas de terminaison SSL sur le point de terminaison. De plus, les serveurs AD FS/WAP disposent de mécanismes intégrés pour empêcher les attaques web courantes, telles que les scripts inter-sites, et satisfont à toutes les exigences définies par le [protocole MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>Je reçois un « Événement 441 : Un jeton avec une clé de liaison de jeton erronée a été détecté ». Que dois-je faire pour résoudre ce problème ?
Dans AD FS 2016, la liaison de jeton est activée automatiquement, et provoque plusieurs problèmes connus avec les scénarios de proxy et de fédération qui génèrent cette erreur. Pour résoudre ce problème, exécutez la commande PowerShell suivante et supprimez la prise en charge de la liaison de jeton.

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>J’ai mis à niveau ma batterie de serveurs à partir d’AD FS dans Windows Server 2016 vers AD FS dans Windows Server 2019. Le niveau de comportement de la batterie AD FS a été augmenté avec succès vers 2019, mais la configuration du proxy d’application Web est toujours affichée comme étant Windows Server 2016.
Après une mise à niveau vers Windows Server 2019, la version de configuration du proxy d’application Web affichée continuera à être Windows Server 2016. Le proxy d’application Web ne dispose pas de nouvelles fonctionnalités propres à la version pour Windows Server 2019, et si le niveau de comportement de la batterie de serveurs a été correctement élevé sur AD FS, le proxy d’application Web continuera à s’afficher comme étant Windows Server 2016 par défaut.

### <a name="can-i-estimate-the-size-of-the-adfsartifactstore-before-enabling-esl"></a>Puis-je estimer la taille d’ADFSArtifactStore avant d’activer ESL ?
Avec ESL activé, AD FS effectue le suivi de l’activité du compte et des emplacements connus pour les utilisateurs dans la base de données ADFSArtifactStore. Cette base de données est mise à l’échelle en fonction du nombre d’utilisateurs et d’emplacements connus suivis. Quand vous envisagez d’activer ESL, vous pouvez partir du principe que la taille de la base de données ADFSArtifactStore augmentera de 1 Go maximum tous les 100 000 utilisateurs. Si la batterie AD FS utilise la base de données interne Windows (WID, Windows Internal Database), l’emplacement par défaut des fichiers de base de données est C:\Windows\WID\Data. Pour empêcher le remplissage de ce lecteur, assurez-vous de disposer d’au moins 5 Go de stockage gratuit avant d’activer ESL. En plus du stockage sur disque, planifiez l’augmentation de la mémoire totale de processus après l’activation d’ESL d’une valeur pouvant aller jusqu’à 1 Go de RAM supplémentaire pour les populations de 500 000 utilisateurs ou moins.
