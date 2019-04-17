---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: ADFS2016 FAQ
description: Forum aux questions pour ADFS2016
author: jenfieldmsft
ms.author: billmath
manager: femila
ms.date: 03/06/2018
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 313447d2c92c15505434ec5c39898ca84aef46db
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>ADFS Forum aux Questions (FAQ)

>S’applique à: Windows Server2016

La documentation suivante est une maison aux questions fréquemment posées en ce qui concerne les Services de fédération ActiveDirectory.  Le document a été divisé en groupes en fonction du type de question.

## <a name="deployment"></a>Déploiement 

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Comment puis-je mise à niveau/migrer depuis des versions antérieures d’ADFS
Vous pouvez mettre à niveau ADFS à l’aide d’une des opérations suivantes:


- ADFS Windows Server2012R2 pour Windows Server2016 ADFS
    - [Mise à niveau vers ADFS dans Windows Server2016 à l’aide d’une base de données WID](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Mise à niveau vers ADFS dans Windows Server2016 à l’aide d’une base de données SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server2012 ADFS pour ADFS Windows Server2012R2
    - [Migrer vers ADFS sur Windows Server2012R2](https://technet.microsoft.com/library/dn486815.aspx)
- ADFS 2.0 pour Windows Server2012 ADFS
    - [Migrer vers ADFS sur Windows Server2012](https://technet.microsoft.com/library/jj647765.aspx)
- ADFS 1.x à ADFS 2.0 
    - [Mise à niveau à partir des services ADFS 1.x à ADFS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Si vous avez besoin mettre à niveau à partir des services ADFS 2.0 ou 2.1 (Windows Server2008R2 ou Windows Server2012), vous devez utiliser les scripts de boîte aux lettres (situés dans C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Pourquoi installation d’ADFS requiert un redémarrage du serveur?

Prise en charge HTTP/2 a été ajoutée dans Windows Server2016, mais HTTP/2 ne peut pas être utilisé pour l’authentification par certificat client.  Étant donné que plusieurs scénarios ADFS utilisation de l’authentification par certificat client et un nombre important de clients ne prend ne pas en charge une nouvelle tentative demande à l’aide de HTTP/1.1, nouveau configure de configuration de batterie de serveurs ADFS les local paramètres du serveur HTTP à HTTP/1.1.  Cela nécessite un redémarrage du serveur.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>Les serveurs Windows2016 WAP utilise pour publier la batterie ADFS à internet sans mise à niveau de la batterie de serveurs ADFS principal pris en charge?
Oui, cette configuration est prise en charge, mais aucuns nouvelles fonctionnalités ADFS2016 n’est pris en charge dans cette configuration.  Cette configuration est destinée à être temporaire lors de la phase de migration depuis ADFS2012R2 à ADFS2016 et ne doit pas être déployée pendant de longues périodes de temps.

## <a name="design"></a>Conception

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Les fournisseurs d’authentification multifacteur tiers sont disponibles pour ADFS? 
Voici une liste des fournisseurs tiers de que nous sommes conscients.  Il peut toujours être disponibles que nous ne connaissent pas et nous mettrons à jour la liste que nous en savoir plus sur les fournisseurs.

- [Services de sécurité et et d’identité Gemalto](http://www.gemalto.com/identity)
- [inWebo service d’authentification de l’entreprise](http://www.inwebo.com/)
- [Connecteur de personnes MFA API Login](https://www.loginpeople.com)
- [Agent d’authentification RSA SecurID pour MicrosoftActiveDirectory Federation Services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [Agent de Service (SAS) SafeNet d’authentification pour ADFS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Service d’authentification Swisscom Mobile ID](http://swisscom.ch/mid)
- [Service de Protection de ID (VIP) et les Symantec Validation](http://www.symantec.com/vip-authentication-service) 

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>Serveurs proxy tiers est pris en charge avec ADFS?
Oui, les proxys tiers peuvent être placés devant le Proxy d’Application Web, mais n’importe quel proxy tiers doit prendre en charge le [protocole MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) pour être utilisé à la place du Proxy d’Application Web.

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>Les proxys tiers sont disponibles pour ADFS qui prennent en charge MS-ADFSPIP?

Voici une liste des fournisseurs tiers de que nous sommes conscients.  Il peut toujours être disponibles que nous ne connaissent pas et nous mettrons à jour la liste que nous en savoir plus sur les fournisseurs.

- [F5 Gestionnaire de stratégie d’accès](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Où est la capacité de planification de la feuille de calcul sur pour ADFS2016?
La version d’ADFS2016 de la feuille de calcul peut être téléchargée [ici](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Cela peut également servir pour ADFS dans Windows Server2012R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Comment puis-je vérifier mon prise en charge de serveurs ADFS et WAP en exigences de ATP d’Apple?

Apple a publié un ensemble d’exigences appelée application Transport Security (ATS) peut avoir un impact sur les appels à partir d’applications iOS qui s’authentifier auprès des services ADFS.  Vous pouvez vous assurer votre ADFS et serveurs WAP conforme en s’assurant qu’ils prennent en charge le [configuration requise pour la connexion à l’aide de ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
En particulier, vous devez vérifier que vos serveurs ADFS et WAP prennent en charge le protocole TLS 1.2 et que suite de chiffrement négociée de la connexion TLS prendra en charge parfaite.

Vous pouvez activer et désactiver des versions de SSL 2.0 et 3.0 et TLS 1.0, 1.1 et 1.2 à l’aide de [gérer les protocoles SSL dans ADFS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Pour vérifier votre ADFS et le WAP serveurs négocient les suites de chiffrement TLS uniquement qui prennent en charge ATP, vous pouvez désactiver toutes les suites de chiffrement qui ne figurent pas dans le [liste des suites de chiffrement conforme ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Pour ce faire, utilisez le [applets de commande Windows PowerShell de TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index). 


## <a name="operations"></a>Opérations

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Comment pour remplacer le certificat SSL pour ADFS? 
Le certificat SSL ADFS n’est pas le même que le certificat de communications de Service ADFS trouvé dans le composant logiciel enfichable Gestion ADFS.  Pour modifier le certificat SSL ADFS, vous devrez utiliser PowerShell. Suivez les instructions indiquées dans l’article ci-dessous:

[La gestion des certificats SSL dans ADFS et 2016 WAP](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Comment puis-je activer ou désactiver les paramètres TLS/SSL pour ADFS
Pour désactiver ou activer les protocoles SSL et suites de chiffrement, utilisez la syntaxe suivante:

[Gérer les protocoles SSL dans ADFS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Le certificat SSL de serveur proxy doit être le même que le certificat SSL ADFS?  
Utilisez les instructions suivantes en ce qui concerne le certificat SSL de proxy et le certificat SSL ADFS:


- Si le proxy est utilisé pour les demandes de proxy ADFS qui utilisent l’authentification Windows intégrée, le certificat SSL de serveur proxy doivent être le même (utilisent la même clé) en tant que le certificat SSL de serveur de fédération
- Si la propriété ADFS «ExtendedProtectionTokenCheck» est activé (paramètre dans ADFS par défaut), le certificat SSL de serveur proxy doit être le même (utilisent la même clé) que le certificat SSL de serveur de fédération
- Dans le cas contraire, le certificat SSL de serveur proxy peut avoir une clé différente du certificat SSL ADFS, mais doit respecter les mêmes [configuration requise](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>Comment puis-je configurer l’invite de commandes = le comportement de connexion pour ADFS?
Pour plus d’informations sur la façon de configurer l’invite = connexion, voir [ActiveDirectory Federation Services invite = prise en charge du paramètre de connexion](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Comment puis-je configurer des navigateurs pour utiliser l’authentification intégrée de Windows (WIA) avec ADFS?

Pour plus d’informations sur la configuration des navigateurs voir [configurer des navigateurs pour utiliser l’authentification intégrée de Windows (WIA) avec ADFS](../operations/Configure-AD-FS-Browser-WIA.md).


### <a name="how-long-are-ad-fs-tokens-valid"></a>La durée pendant laquelle sont des jetons ADFS valides?

Fréquence à laquelle cette question signifie «la durée pendant laquelle les utilisateurs obtiennent-ils d’authentification unique (SSO) sans avoir à entrer de nouvelles informations d’identification, et comment puis-je en tant qu’administrateur un contrôle qui?»  Ce comportement et les paramètres de configuration qui contrôlent, sont décrits dans l’article [ici](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Les durées de vie par défaut des différents jetons et les cookies sont répertoriées ci-dessous (ainsi que les paramètres qui régissent les durées de vie):

**Appareils inscrits**

- Les cookies PRT et l’authentification unique: 90jours au maximum, régies par PSSOLifeTimeMins. (Appareil fourni est utilisé au moins tous les 14jours, qui est contrôlé par DeviceUsageWindow)

- Jeton d’actualisation: dépend de la ci-dessus pour proposer un comportement homogène

- access_token: 1heure par défaut, basée sur la partie de confiance

- id_token: identique au jeton d’accès

**Suppression de l’inscription de périphériques**

- Les cookies d’authentification unique: 8heures par défaut, est régi par SSOLifetimeMins.  Lors de la conserver Me connecté (KMSI) est activé, par défaut est de 24heures et configurable via KMSILifetimeMins.


- Jeton d’actualisation: 8heures par défaut. 24heures avec KMSI activé

- access_token: 1heure par défaut, basée sur la partie de confiance

- id_token: identique au jeton d’accès

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>ADFS ne prend en charge HTTP Strict Transport Security (HSTS)?  

HTTP Strict Transport Security (HSTS) est un mécanisme de stratégie de sécurité web qui vous aide à atténuer les attaques par rétrogradation de protocole et le piratage de cookie pour les services qui ont des points de terminaison HTTP et HTTPS. Il permet de serveurs web déclarer que les navigateurs web (ou autres agents utilisateurs conforme) interagissez uniquement à l’aide de HTTPS et jamais via le protocole HTTP.

Tous les points de terminaison ADFS pour le trafic de l’authentification web sont ouverts exclusivement sur HTTPS.  Par conséquent, ADFS réduit efficacement les menaces qui fournit du mécanisme de stratégie HTTP Strict Transport Security (à la conception n’est pas rétrograder vers HTTP dans la mesure où il n’existe aucun écouteur HTTP). En outre, ADFS empêche les cookies envoyés vers un autre serveur avec les points de terminaison HTTP protocole en marquant tous les cookies avec l’indicateur de sécurité.

Par conséquent, l’implémentation de HSTS sur un serveur ADFS n’est pas requis, car il ne peut jamais être rétrogradé.  Par souci de conformité, serveurs ADFS, répondre à ces exigences, car ils ne peuvent jamais utiliser HTTP et tous les cookies sont marqués sécurisés.

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-transmis-client-ip ne contient-elle pas l’adresse IP du client, mais contient l’adresse IP du pare-feu devant le serveur proxy. Où puis-je obtenir l’adresse IP appropriée du client?
Il n’est pas recommandé pour effectuer la terminaison SSL avant WAP. En cas de terminaison SSL est effectuée sur le devant de la WAP, le X-ms-transmis-client-ip contient l’adresse IP du périphérique réseau devant WAP. Vous trouverez ci-dessous qu'une brève description de l’adresse IP divers liés les revendications qui sont pris en charge par ADFS:
 - x-ms-client-ip: réseau IP de dispositif connecté pour le service STS.  Dans le cas d’une demande d’extranet contient toujours l’adresse IP du WAP.
 - x-ms-transmis-client-ip: revendication à valeurs multiples qui contient toutes les valeurs transférées à ADFS par Exchange Online, ainsi que l’adresse IP de l’appareil connecté pour le protocole WAP.
 - Userip: Pour les demandes d’extranet, cette revendication contiennent la valeur de x-ms-transmis-client-ip.  Pour les demandes de l’intranet, cette revendication contient la même valeur que x-ms-client-ip.

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Vous essayez d’obtenir des revendications supplémentaires sur le point de terminaison des informations utilisateur, mais il renvoie uniquement sujet. Comment puis-je obtenir des revendications supplémentaires?
Le point de terminaison ADFS userinfo retourne toujours la revendication d’objet comme spécifié dans les normes OpenID. ADFS ne fournit pas les demandes supplémentaires via le point de terminaison UserInfo. Si vous avez besoin de revendications supplémentaires dans le jeton d’ID, reportez-vous à [personnalisé ID de jetons dans ADFS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-alot-of-1021-errors-on-my-ad-fs-servers"></a>Pourquoi un grand nombre d’erreurs 1021 sur mes serveurs ADFS?
Cet événement est généralement enregistré pour un accès de ressources non valides sur ADFS pour les ressources 00000003-0000-0000-c000-type "000000000000". Cette erreur est due à un comportement erroné du client où il tente d’obtenir un jeton d’accès pour le service Azure AD graphique. Étant donné que la ressource n’est pas présente sur ADFS, cela entraîne l’événement ID1021 sur les serveurs ADFS. Il est recommandé d’ignorer les avertissements ou erreurs pour les ressources 00000003-0000-0000-c000-type "000000000000" sur ADFS. 

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Pourquoi vois-je un avertissement d’échec ajouter le compte de service ADFS au groupe Administrateurs de l’entreprise clé?
Ce groupe est créé uniquement lorsqu’un contrôleur de domaine Windows2016 avec le rôle FSMO du PDC existe dans le domaine. Pour résoudre l’erreur, vous pouvez créer le groupe manuellement et suivre la ci-dessous pour accorder l’autorisation nécessaire après avoir ajouté le compte de service en tant que membre du groupe.
1.  Ouvrez **utilisateurs et ordinateurs ActiveDirectory**. 
2.  **Avec le bouton droit** votre nom de domaine à partir du volet de navigation et **cliquez** propriétés.
3.  **Cliquez sur** sécurité (si l’onglet sécurité est manquant, activer les fonctionnalités avancées dans le menu Affichage).
4.  **Cliquez sur** avancée. **Cliquez sur** ajouter. **Cliquez sur** sélectionnez un principal.
5.  La boîte de dialogue Sélectionner un utilisateur, ordinateur, compte de Service ou groupe s’affiche.  Dans la zone Entrez le nom d’objet pour la zone de sélection de texte, tapez la clé Admin groupe.  Cliquez sur OK.
6.  Dans s’applique à la zone de liste, sélectionnez **objets utilisateur Descendant**.
7.  À l’aide de la barre de défilement, faites défiler vers le bas de la page et **cliquez** Effacer tout.
8.  Dans le **propriétés** section, sélectionnez **lire msDS-KeyCredentialLink** et **écrire l’attribut msDS-KeyCrendentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Pourquoi l’authentification moderne à partir d’appareils Android n’échoue pas si le serveur n’envoie pas de tous les certificats intermédiaires dans la chaîne avec le certificat SSL?

Les utilisateurs fédérés peuvent rencontrer l’authentification à Azure AD pour les applications qui utilisent l’échec d’une bibliothèque ADAL Android. L’application puisse obtenir un **AuthenticationException** lorsqu’il tente d’afficher la page de connexion. Chrome les services ADFS page de connexion peut être mentionne comme non sûre.

Android - sur toutes les versions et tous les périphériques - ne gère pas télécharger de certificats supplémentaires à partir de la **Accès_info_autorité** champ du certificat. Cela est également vrai du navigateur Chrome. Tout certificat d’authentification serveur manque les certificats intermédiaires entraîne cette erreur si la chaîne de certificats entière n’est pas transmise à partir des services ADFS. 

Une solution appropriée à ce problème consiste à configurer les serveurs ADFS et WAP pour envoyer les certificats intermédiaires nécessaires ainsi que le certificat SSL. 

Lorsque l’exportation du certificat SSL, à partir d’un seul ordinateur, pour être importés dans le magasin personnel de l’ordinateur, des serveurs ADFS et WAP, veillez à exporter privé enfoncée, puis sélectionnez **échange d’informations personnelles - PKCS #12**. 

Il est important que la case à cocher pour **inclure tous les certificats dans le chemin d’accès du certificat, si possible** est activée, mais aussi **exporter toutes les propriétés étendues**. 

Exécutez certlm.msc sur les serveurs Windows et importer le *. PFX dans le magasin de certificats personnel de l’ordinateur. Cela entraîne le serveur passer la chaîne de certificats entière à la bibliothèque ADAL. 

>[!NOTE]
> Le certificat du store des équilibreurs de charge réseau doit également être mis à jour pour inclure la chaîne de certificats entière, le cas échéant

### <a name="does-ad-fs-support-head-requests"></a>ADFS ne prend en charge les demandes de tête?
ADFS ne gère pas les demandes de tête.  Les applications ne doivent pas utiliser les requêtes HEAD contre les points de terminaison ADFS.  Cela peut entraîner des réponses d’erreur HTTP qui sont inattendus et/ou différée.  En outre, vous pouvez voir les événements d’erreur inattendue dans le journal des événements ADFS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Pourquoi je ne vois pas un jeton d’actualisation lorsque j’ai une session avec un IdP à distance?
Un jeton d’actualisation n’est pas émis si le jeton émis par IdP a un validty de moins de 1heure. Pour garantir qu'un jeton d’actualisation est émis, augmentez la validité des jetons émis par l’IdP à plus de 1heure.
