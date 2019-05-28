---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: 'Services AD FS : FAQ'
description: Forum aux questions pour AD FS 2016
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fdd31a8b7c2c6ef87d1d22d901b5c6ca69b5c70d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188716"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS, Forum aux Questions (FAQ)


La documentation suivante est un accueil aux questions fréquemment posées en ce qui concerne les Services de fédération Active Directory.  Le document a été divisé en groupes en fonction du type de question.

## <a name="deployment"></a>Déploiement

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Comment puis-je mise à niveau/migrer à partir de versions précédentes d’AD FS
Vous pouvez mettre à niveau AD FS en utilisant l’une des opérations suivantes :


- Windows Server 2012 R2 AD FS pour Windows Server 2016 AD FS
    - [Mise à niveau vers AD FS dans Windows Server 2016 à l’aide d’une base de données WID](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Mise à niveau vers AD FS dans Windows Server 2016 à l’aide d’une base de données SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS pour Windows Server 2012 R2 AD FS
    - [Migrer vers AD FS sur Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 pour Windows Server 2012 AD FS
    - [Migrer vers AD FS sur Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x vers AD FS 2.0
    - [Mise à niveau à partir d’AD FS 1.x vers AD FS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Si vous avez besoin mettre à niveau à partir d’AD FS 2.0 ou 2.1 (Windows Server 2008 R2 ou Windows Server 2012), vous devez utiliser les scripts d’origine (situés dans C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Pourquoi installation d’AD FS nécessite un redémarrage du serveur ?

Prise en charge HTTP/2 a été ajouté dans Windows Server 2016, mais HTTP/2 ne peut pas être utilisé pour l’authentification par certificat client.  Étant donné que les nombreux scénarios AD FS rendent l’utilisation de l’authentification par certificat client et un nombre important de clients ne prend ne pas en charge une nouvelle tentative demande à l’aide de HTTP/1.1, ré-configure de configuration de batterie de serveurs AD FS les paramètres HTTP du serveur local vers la version HTTP/1.1.  Cela nécessite un redémarrage du serveur.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>Les serveurs WAP Windows 2016 utilise pour publier la batterie de serveurs AD FS à internet sans mise à niveau de la batterie de serveurs AD FS principaux pris en charge ?
Oui, cette configuration est prise en charge, mais aucuns nouvelles fonctionnalités AD FS 2016 ne seraient être pris en charge dans cette configuration.  Cette configuration est destinée à être temporaire pendant la phase de migration à partir d’AD FS 2012 R2 à AD FS 2016 et ne doit pas être déployée pour de longues périodes.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>Est-il possible de déployer AD FS pour Office 365 sans avoir à publier un proxy à Office 365 ?
Oui, cela est pris en charge. Toutefois, comme un effet secondaire

1. Vous devez gérer manuellement la mise à jour des certificats de signatures de jeton, car Azure AD ne sera pas en mesure d’accéder aux métadonnées de fédération. Pour plus d’informations sur la mise à jour manuelle de certificat de signature de jetons lire [renouveler les certificats de fédération pour Office 365 et Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. Vous ne serez pas en mesure de tirer parti de l’authentification héritée, flux (par exemple, flux d’authentification proxy ExO)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>Quelles sont les exigences de l’équilibrage de charge pour les serveurs AD FS et WAP ?

AD FS est un système sans état. Par conséquent, il est relativement simple pour les connexions de l’équilibrage de charge. Voici les principales recommandations pour les systèmes d’équilibrage de charge.


- Équilibreurs de charge ne doivent pas être configurés avec l’affinité d’IP. Cela peut placer une charge excessive sur un sous-ensemble de vos serveurs dans certains scénarios Exchange Online.
- Équilibreurs de charge ne doivent pas arrêter les connexions HTTPS et relancer une nouvelle connexion vers le serveur ADFS.
- Équilibreurs de charge doivent s’assurer que l’adresse IP de connexion doit être convertis en tant que l’adresse IP source dans le paquet HTTP lors de l’envoi à ADFS. Dans le cas où un équilibreur de charge ne peut pas envoyer l’adresse IP source dans le paquet HTTP, l’équilibreur de charge doit ajouter (ou ajouter existant dans le cas) l’adresse IP à l’en-tête x-forwarded-for. Cela est nécessaire pour la gestion correcte de certains IP liés des fonctionnalités (adresses IP interdites, Extranet verrouillage intelligent,...) et peut entraîner des problèmes de sécurité si mal configuré.
- Équilibreurs de charge doivent prendre en charge SNI. Dans le cas contraire, vérifiez qu’AD FS est configuré pour créer des liaisons HTTPS pour gérer les clients compatibles non SNI.
- Équilibreurs de charge doivent utiliser le point de terminaison de sonde d’intégrité AD FS HTTP pour détecter si les serveurs AD FS ou WAP ou sont en cours d’exécution et les exclure si une réponse 200 OK n’est pas retournée.

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>Quelles configurations de forêts multiples sont pris en charge par AD FS ?

AD FS prend en charge de la configuration à plusieurs forêts multiples et s’appuie sur le réseau de confiance AD DS sous-jacent pour authentifier les utilisateurs entre plusieurs domaines de confiance. Nous vous recommandons d’approbations de forêts de 2 voies car il s’agit d’une configuration plus simple pour vous assurer que le sous-système d’approbation fonctionne correctement sans problème. En outre,

- En cas d’une approbation de forêt unidirectionnelle, comme une forêt de zone DMZ contenant les identités du partenaire, nous vous recommandons de déployer ADFS dans la forêt corp et en traitant de la forêt du réseau de périmètre comme un autre approbation de fournisseur de revendications local connectée via LDAP. Dans ce cas une authentification Windows intégrée ne fonctionnera pas pour les utilisateurs de la forêt DMZ et elles seront nécessaires pour effectuer l’authentification de mot de passe car c’est le seul mécanisme pris en charge pour le protocole LDAP. Dans le cas vous ne pouvez pas poursuivre cette option, vous devez configurer un autre ADFS dans la forêt du réseau de périmètre et l’ajouter en tant que l’approbation de fournisseur de revendications dans ADFS dans la forêt corp. Les utilisateurs doivent la découverte de domaine d’accueil, mais une authentification intégrée Windows et authentification de mot de passe ne fonctionneront pas. Veuillez apporter les modifications appropriées dans les règles d’émission dans AD FS dans la forêt du réseau de périmètre comme AD FS dans la forêt corp ne sera pas en mesure d’obtenir des informations utilisateur supplémentaires sur l’utilisateur à partir de la forêt du réseau de périmètre.
- Bien que les approbations de niveau domaine sont pris en charge et peuvent fonctionner, nous vous recommandons vivement déplacement vers un modèle d’approbation de niveau forêt. En outre, vous devez vous assurer que UPN routage et résolution de noms NETBIOS des noms doivent fonctionner avec précision.



## <a name="design"></a>Concevoir

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Les fournisseurs de l’authentification multifacteur tiers sont disponibles pour AD FS ?
Voici une liste de fournisseurs tiers que nous sommes conscients de.  Il peut toujours être fournisseurs disponibles que nous ne savons pas sur et nous mettrons à jour la liste que nous en savoir plus à leur sujet.

- [Services de sécurité & et d’identité Gemalto](http://www.gemalto.com/identity)
- [inWebo service d’authentification de l’entreprise](http://www.inwebo.com/)
- [Connecteur de personnes MFA API Login](https://www.loginpeople.com)
- [Agent d’authentification RSA SecurID pour Microsoft Active Directory Federation Services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [Agent de Service (SAP) SafeNet Authentication pour AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Service d’authentification Swisscom Mobile ID](http://swisscom.ch/mid)
- [Validation de Symantec et de Service de Protection d’ID (VIP)](http://www.symantec.com/vip-authentication-service)

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>Les proxys de tiers sont pris en charge avec AD FS ?
Oui, les proxys de tiers peuvent être placés devant le Proxy d’Application Web, mais n’importe quel proxy tiers doit prendre en charge la [protocole de MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) être utilisé à la place du Proxy d’Application Web.

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>Les proxys de tiers sont disponibles pour AD FS qui prennent en charge MS-ADFSPIP ?

Voici une liste de fournisseurs tiers que nous sommes conscients de.  Il peut toujours être fournisseurs disponibles que nous ne savons pas sur et nous mettrons à jour la liste que nous en savoir plus à leur sujet.

- [F5 Access Policy Manager](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Où est la capacité de planification de la feuille de calcul sur pour AD FS 2016 ?
La version d’AD FS 2016 de la feuille de calcul peut être téléchargée [ici](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Cela peut également être utilisé pour AD FS dans Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Comment puis-je m’assurer que ma prise en charge de serveurs AD FS et WAP exigences d’ATP d’Apple ?

Apple a publié un ensemble d’exigences appelée application Transport Security (ATS) qui peut avoir un impact sur les appels à partir d’applications iOS qui s’authentifient auprès d’AD FS.  Vous pouvez vous assurer de votre AD FS et les serveurs WAP sont conformes en vous assurant qu’ils prennent en charge la [configuration requise pour la connexion à l’aide d’ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
En particulier, vous devez vérifier que vos serveurs AD FS et WAP prennent en charge TLS 1.2 et que suite de chiffrement négociée de la connexion TLS prendra en charge de transmission parfaite.

Vous pouvez activer et désactiver SSL 2.0 et 3.0 et TLS versions 1.0, 1.1 et 1.2 à l’aide de [gérer les protocoles SSL dans AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Pour vous assurer de votre AD FS et WAP de serveurs négocient les suites de chiffrement TLS uniquement qui prennent en charge d’ATP, vous pouvez désactiver toutes les suites de chiffrement qui ne figurent pas dans le [liste ATP conforme des suites de chiffrement](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Pour ce faire, utilisez le [applets de commande PowerShell de TLS Windows](https://technet.microsoft.com/itpro/powershell/windows/tls/index).

## <a name="developer"></a>Développeur

### <a name="when-generating-an-idtoken-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-idtoken"></a>Lorsque vous générez un jeton id_token avec ADFS pour un utilisateur authentifié auprès d’AD, comment est la revendication « sub » a généré dans le paramètre id_token ?
La valeur de revendication de « sub » est le hachage de l’ID client + valeur de revendication de point d’ancrage.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>Qu’est la durée de vie du jeton de jeton d’accès de l’actualisation quand l’utilisateur se connecte via une approbation de fournisseur de revendications à distance via WS-Fed/SAML-P ?
La durée de vie du jeton d’actualisation sera la durée de vie du jeton qui ADFS a obtenu à partir de l’approbation de fournisseur de revendications à distance. La durée de vie du jeton d’accès sera la durée de vie de la partie de confiance pour laquelle le jeton d’accès est émis.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>J’ai besoin de retourner des étendues profile et email ainsi en plus de l’étendue d’OpenId. Puis-je obtenir des informations supplémentaires à l’aide d’étendues ? Comment le faire dans AD FS ?
Vous pouvez utiliser id_token personnalisé pour ajouter des informations pertinentes dans le paramètre id_token lui-même. Pour plus d’informations, consultez l’article [personnaliser des revendications à émettre dans id_token](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>Comment émettre des objets BLOB json à l’intérieur des jetons JWT ?
Un type de valeur spéciale (« http://www.w3.org/2001/XMLSchema#json») et de la séquence d’échappement character(\x22) pour que cela a été ajoutée dans AD FS 2016. Consultez l’exemple ci-dessous pour la règle d’émission et de la sortie finale à partir du jeton d’accès.

Exemple de règle d’émission :

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Revendications émises dans le jeton d’accès :

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>Puis-je passer la valeur de ressource dans le cadre de la valeur d’étendue, comme la façon dont les demandes sont effectuées auprès d’Azure AD ?
Avec AD FS sur 2019 de serveur, vous pouvez maintenant passer la valeur de la ressource incorporée dans le paramètre d’étendue. Le paramètre d’étendue peut maintenant être organisé comme une liste séparée par des espaces où chaque entrée a une structure en tant que portée de la ressource. Exemple :  
**< créer une demande d’exemple valide >**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS prend-elle en charge l’extension PKCE ?
AD FS dans Server 2019 prend en charge la clé de preuve pour Code Exchange (PKCE) pour les flux d’octroi de Code d’autorisation OAuth

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>Les étendues autorisées sont pris en charge par AD FS ?
- AZA - si vous utilisez [Extensions du protocole OAuth 2.0 pour les Clients de service Broker](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) et si le paramètre d’étendue contient l’étendue « aza », le serveur émet un nouveau jeton d’actualisation principaux et il définit dans le champ de jeton d’actualisation de la réponse, ainsi que les paramètre le champ refresh_token_expires_in à la durée de vie de nouveau jeton d’actualisation principaux si un est appliqué.
- OpenID - permet à l’application demander l’utilisation du protocole d’autorisation OpenID Connect.
- logon_cert - l’étendue logon_cert permet à une application pour demander des certificats d’ouverture de session, ce qui peut être utilisé pour connecter les utilisateurs authentifiés de manière interactive. Le serveur AD FS omet le paramètre de jeton d’accès de la réponse et une chaîne codée en base64 d’un certificat CMS ou une réponse d’infrastructure à clé publique complète CMC propose à la place. Plus de détails [ici](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation - l’étendue user_impersonation est nécessaire à demander un jeton d’accès on-behalf-of d’AD FS. Pour plus d’informations sur l’utilisation de cette étendue, consultez [créer une application à plusieurs niveaux à l’aide pour le compte (OBO) à l’aide d’OAuth avec AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert - l’étendue vpn_cert permet à une application pour demander des certificats VPN, ce qui peut être utilisé pour établir des connexions VPN à l’aide de l’authentification EAP-TLS. Cela est non plus prises en charge.
- l’e-mail - permet à l’application demander la revendication de courrier électronique pour l’utilisateur connecté. Cela est non plus prises en charge. 
- profil - permet à l’application demander de profil liées des revendications pour l’utilisateur. Cela est non plus prises en charge. 


## <a name="operations"></a>Opérations

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Comment pour remplacer le certificat SSL pour AD FS ?
Le certificat SSL AD FS n’est pas le même que le certificat de communications de Service AD FS trouvé dans le composant logiciel enfichable Gestion AD FS.  Pour modifier le certificat SSL AD FS, vous devrez utiliser PowerShell. Suivez les instructions de l’article ci-dessous :

[Gestion des certificats SSL dans AD FS et WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Comment puis-je activer ou désactiver les paramètres de TLS/SSL pour AD FS
Pour désactiver ou activer des protocoles SSL et suites de chiffrement, procédez comme suit :

[Gérer les protocoles SSL dans AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Le certificat SSL de proxy a le même que le certificat SSL AD FS ?  
Utilisez les instructions suivantes en ce qui concerne le certificat SSL de proxy et le certificat SSL AD FS :


- Si le proxy est utilisé pour les demandes de proxy AD FS qui utilisent l’authentification Windows intégrée, le certificat SSL de proxy doivent être le même (utiliser la même clé) que le certificat SSL de serveur de fédération
- Si la propriété AD FS « ExtendedProtectionTokenCheck » est activé (le paramètre par défaut dans AD FS), le certificat SSL de proxy doit être le même (utiliser la même clé) que le certificat SSL de serveur de fédération
- Sinon, le certificat SSL de proxy peut avoir une autre clé à partir du certificat SSL AD FS, mais doit respecter les mêmes [configuration requise](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>Comment puis-je configurer l’invite de commandes = le comportement de connexion pour AD FS ?
Pour plus d’informations sur la façon de configurer l’invite = connexion, consultez [Active Directory Federation Services invite = prise en charge du paramètre de connexion](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-change-the-ad-fs-service-account"></a>Comment puis-je modifier le compte de service AD FS ?
Pour modifier le compte de service AD FS, suivez les instructions à l’aide de la boîte à outils AD FS [Powershell Module de compte de Service](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule). 

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Comment puis-je configurer des navigateurs d’utiliser l’authentification intégrée de Windows (WIA) avec AD FS ?

Pour plus d’informations sur la configuration des navigateurs, consultez [configurer des navigateurs pour utiliser l’authentification intégrée de Windows (WIA) avec AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-trun-off-browserssoenabled"></a>Faire trun off BrowserSsoEnabled ?
Si vous n’avez des stratégies de contrôle d’accès en fonction de l’appareil sur AD FS ou Windows Hello pour l’inscription de certificat d’entreprise à l’aide d’ADFS ; Vous pouvez désactiver BrowserSsoEnabled. BrowserSsoEnabled permet à ADFS collecter un PRT (jeton d’actualisation principal) à partir du client qui contient des informations sur l’appareil. Sans cet appareil, l’authentification d’ADFS ne fonctionne pas sur les appareils Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>Quelle sont la durée valide les jetons AD FS ?

Cette question signifie généralement « la durée pendant laquelle les utilisateurs bénéficient-ils d’authentification unique (SSO) sans avoir à entrer de nouvelles informations d’identification, et comment puis-je en tant qu’administrateur contrôler qui ? »  Ce comportement et les paramètres de configuration qui contrôlent, sont décrits dans l’article [AD FS Single Sign-On Settings](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Les durées de vie par défaut des différents jetons et les cookies sont répertoriées ci-dessous (ainsi que les paramètres qui régissent les durées de vie) :

**Appareils inscrits**

- Cookies PRT et l’authentification unique : 90 jours au maximum, régie par PSSOLifeTimeMins. (Appareil fourni est utilisé au moins tous les 14 jours, qui est contrôlé par DeviceUsageWindow)

- Jeton d’actualisation : calculé en fonction de la méthode ci-dessus pour fournir un comportement cohérent

- jeton d’accès : 1 heure par défaut, en fonction de la confiance

- id_token : identique au jeton d’accès

**Appareils non inscrits**

- Cookies d’authentification unique : par défaut, régie par SSOLifetimeMins les 8 heures.  Lors de la conserver de la connexion (KMSI) est activée, valeur par défaut est de 24 heures et configurable via KMSILifetimeMins.


- Jeton d’actualisation : 8 heures par défaut. dernières 24 heures avec maintenir la connexion activée

- jeton d’accès : 1 heure par défaut, en fonction de la confiance

- id_token : identique au jeton d’accès

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS prend en charge HTTP Strict Transport Security (HSTS) ?  

HTTP Strict Transport Security (HSTS) est un mécanisme de stratégie de sécurité web qui vous aide à atténuer les attaques de protocole vers une version antérieure et détournement de cookie pour les services qui ont des points de terminaison HTTP et HTTPS. Il permet de serveurs web déclarer que les navigateurs web (ou autres agents utilisateurs conforme) doivent uniquement interagir avec lui à l’aide de HTTPS et jamais via le protocole HTTP.

Tous les points de terminaison AD FS pour le trafic d’authentification web sont ouverts exclusivement via le protocole HTTPS.  Par conséquent, ADFS permet d’atténuer efficacement les menaces qui fournit du mécanisme de stratégie HTTP Strict Transport Security (par conception n’est aucun vers une version antérieure à HTTP, car il n’existe aucun écouteur HTTP). En outre, AD FS empêche les cookies envoyés vers un autre serveur avec les points de terminaison de protocole HTTP par le marquage de tous les cookies avec l’indicateur de sécurité.

Par conséquent, implémenter HSTS sur un serveur AD FS n’est pas nécessaire, car il ne peut jamais être rétrogradé.  Par souci de conformité, les serveurs AD FS, répondre à ces exigences, car ils ne peuvent jamais utiliser HTTP et tous les cookies sont marquées sécurisés.

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-transférés--adresse ip du client ne contient-elle pas l’adresse IP du client, mais contient l’adresse IP du pare-feu devant le serveur proxy. Où puis-je obtenir l’adresse IP appropriée du client ?
Il n’est pas recommandé pour effectuer un arrêt SSL avant de WAP. En cas d’arrêt SSL est effectué devant le proxy d’application Web, le X-ms-transférés-client-ip contiendra l’adresse IP du périphérique réseau devant WAP. Voici qu'une brève description de l’adresse IP différents liés des revendications qui sont pris en charge par AD FS :
 - x-ms-client-ip : Réseau IP du périphérique connecté à un service STS.  Dans le cas d’une demande extranet contient toujours l’adresse IP de WAP.
 - x-ms-forwarded-client-ip : Revendication à valeurs multiples qui contiendra toutes les valeurs transmises à ADFS par Exchange Online, ainsi que l’adresse IP de l’appareil connecté vers le proxy d’application Web.
 - Userip : Pour les demandes extranets cette revendication contient la valeur de x-ms-transférés-client-ip.  Pour les demandes de l’intranet, cette revendication contient la même valeur que x-ms-client-ip.

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Vous essayez d’obtenir des revendications supplémentaires sur le point de terminaison des informations utilisateur, mais son retour uniquement l’objet. Comment puis-je obtenir des revendications supplémentaires ?
Le point de terminaison userinfo ADFS retourne toujours la valeur de la revendication de l’objet tel que spécifié dans les normes OpenID. AD FS ne fournit pas de revendications supplémentaires demandées par le biais du point de terminaison UserInfo. Si vous avez besoin des revendications supplémentaires dans le jeton d’ID, reportez-vous à [jetons d’ID personnalisés dans AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>Pourquoi de nombreuses erreurs de 1021 sur mes serveurs AD FS ?
Cet événement est généralement enregistré pour un accès de ressource non valide sur AD FS pour la ressource 00000003-0000-0000-c000-000000000000. Cette erreur est due à un comportement incorrect du client où il tente d’obtenir un jeton d’accès pour le service Azure AD Graph. Étant donné que la ressource n’est pas présente sur AD FS, cela entraîne l’événement ID 1021 sur les serveurs AD FS. Il est possible d’ignorer les avertissements ou erreurs pour 00000003-0000-0000-c000-000000000000 de ressources sur AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Pourquoi est-ce que je reçois un avertissement d’échec ajouter le compte de service AD FS au groupe Administrateurs de clé d’entreprise ?
Ce groupe est créé uniquement lorsqu’un contrôleur de domaine Windows 2016 avec le rôle FSMO du PDC existe dans le domaine. Pour résoudre l’erreur, vous pouvez créer le groupe manuellement et suivre le ci-dessous afin de donner l’autorisation nécessaire après avoir ajouté le compte de service en tant que membre du groupe.
1.  Ouvrez **Utilisateurs et ordinateurs Active Directory**.
2.  **Avec le bouton droit** votre nom de domaine dans le volet de navigation et **cliquez sur** propriétés.
3.  **Cliquez sur** sécurité (si l’onglet sécurité est manquant, activer les fonctionnalités avancées dans le menu Affichage).
4.  **Cliquez sur** avancé. **Cliquez sur** ajouter. **Cliquez sur** sélectionnez un principal.
5.  La boîte de dialogue Sélectionner un utilisateur, ordinateur, compte de Service ou groupe s’affiche.  Dans la zone Entrez le nom d’objet à la zone de sélection de texte, tapez clé de groupe d’administration.  Cliquez sur OK.
6.  Dans la zone s’applique à liste, sélectionnez **objets utilisateur descendants**.
7.  À l’aide de la barre de défilement, faites défiler vers le bas de la page et **cliquez sur** tout Effacer.
8.  Dans le **propriétés** section, sélectionnez **lire msDS-KeyCredentialLink** et **écrire msDS-KeyCredentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Pourquoi l’authentification moderne à partir d’appareils Android n’échoue pas si le serveur n’envoie pas de tous les certificats intermédiaires dans la chaîne avec le certificat SSL ?

Les utilisateurs fédérés peuvent rencontrer l’authentification à Azure AD pour les applications qui utilisent l’échec de la bibliothèque ADAL Android. L’application obtiendra un **AuthenticationException** quand il essaie d’afficher la page de connexion. Dans les services AD FS de chrome page de connexion peut-être être exigée comme non sûre.

Android - entre toutes les versions et tous les appareils - ne prend pas en charge télécharger les certificats supplémentaires à partir de la **Accès_info_autorité** champ du certificat. Cela est vrai du navigateur Chrome également. N’importe quel certificat d’authentification serveur qui n’a pas de certificats intermédiaires provoquer cette erreur si la chaîne de certificats entière n’est pas passée à partir d’AD FS.

Une solution appropriée à ce problème consiste à configurer les serveurs AD FS et WAP pour envoyer les certificats intermédiaires nécessaires, ainsi que le certificat SSL.

Lorsque exporté le certificat SSL, un même ordinateur, à être importé dans le magasin personnel, d’AD FS et l’ou les serveurs WAP, l’ordinateur veillez à exporter la clé privée et sélectionnez **échange d’informations personnelles - PKCS #12**.

Il est important que la case à cocher pour **inclure tous les certificats dans le chemin d’accès du certificat si possible** est activée, ainsi que **exporter toutes les propriétés étendues**.  

Exécutez certlm.msc sur les serveurs Windows et importer le *. PFX dans le magasin de certificats personnel de l’ordinateur. Cela entraîne le serveur pour transmettre la chaîne de certificat dans son intégralité à la bibliothèque ADAL.

>[!NOTE]
> Le certificat de magasin d’équilibreurs de charge réseau doit également être mis à jour pour inclure la chaîne de certificat dans son intégralité, le cas échéant

### <a name="does-ad-fs-support-head-requests"></a>AD FS prend en charge les demandes HEAD ?
AD FS ne prend pas en charge les demandes HEAD.  Les applications ne devraient pas utiliser les requêtes HEAD par rapport à des points de terminaison AD FS.  Cela peut entraîner des réponses d’erreur HTTP inattendu et/ou retardée.  En outre, vous pourriez voir des événements d’erreur inattendue dans le journal des événements AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Pourquoi je ne vois pas un jeton d’actualisation quand je me connecte avec un fournisseur d’identité à distance ?
Un jeton d’actualisation n’est pas émis si le jeton émis par le fournisseur d’identité a un validty de moins de 1 heure. Pour qu'un jeton d’actualisation est émis, augmentez la validité du jeton émis par le fournisseur d’identité et plus de 1 heure.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>Disposons-nous possible de modifier l’algorithme de chiffrement de jeton de fournisseur de ressources ?
Par défaut le chiffrement de jeton de fournisseur de ressources est défini sur AES256 et il ne peut pas être modifié pour toute autre valeur.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>Sur une batterie de serveurs en mode mixte, j’obtiens une erreur lorsque vous tentez de définir le nouveau certificat SSL à l’aide de Set-AdfsSslCertificate-Thumbprint. Comment puis-je mettre à jour le certificat SSL dans une batterie de serveurs AD FS en mode mixte ?
Sur les serveurs WAP, vous pouvez toujours utiliser Set-WebApplicationProxySslCertificate. Sur les serveurs ADFS, vous devez utiliser l’outil netsh. Suivez les étapes ci-dessous :

1. Sélectionner le sous-ensemble de serveurs AD FS 2016 pour la maintenance (par exemple, supprimez à partir de l’équilibreur de charge)
2. Sur les serveurs sélectionnés dans #1, importer le nouveau certificat via la console MMC
3. Supprimer les certificats existants

    a. netsh http delete sslcert hostnameport=fs.contoso.com:443 b. netsh http delete sslcert hostnameport=localhost:443 c. netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  Ajouter le nouveau certificat

    a. netsh http ajouter sslcert hostnameport=fs.contoso.com:443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

    b. netsh http add sslcert hostnameport=localhost:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    c. netsh http ajouter sslcert hostnameport=fs.contoso.com:49443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

5. Redémarrez le service ADFS sur le serveur sélectionné
6. Supprimer le sous-ensemble de serveurs WAP pour la maintenance
7. Sur les serveurs WAP sélectionnés, importer le nouveau certificat via la console MMC
8. Définir le nouveau certificat sur le proxy d’application Web à l’aide d’applet de commande

    a. Set-WebApplicationProxySslCertificate-empreinte numérique « CERTTHUMBPRINT »

9. Redémarrez le service sur les serveurs WAP sélectionnés
10. Placer les serveurs WAP et AD FS sélectionnés dans l’environnement de production.

Effectuez la mise à jour sur le reste de AD FS et les serveurs WAP de manière similaire.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>ADFS en charge lorsque les serveurs Proxy d’Application Web (WAP) sont trouvent derrière des Firewall(WAF) d’Application Azure Web ?
AD FS et des serveurs d’Application Web prend en charge n’importe quel pare-feu qui n’effectue pas d’un arrêt SSL sur le point de terminaison. En outre, les serveurs AD FS/WAP ont intégré des mécanismes pour éviter les attaques web courantes telles que des scripts, de proxy AD FS intersites et répondent à toutes les exigences définies par le [protocole de MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).
