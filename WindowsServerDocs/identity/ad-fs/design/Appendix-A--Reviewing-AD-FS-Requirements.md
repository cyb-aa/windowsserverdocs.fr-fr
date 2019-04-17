---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: 'Annexe A: examen de configuration AD FS requise'
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d5cfb5de77843eebfc152b9c79ac55bab1fa7727
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>Annexe a: examen de configuration ADFS requise

>S’applique à: Windows Server2012

Afin que les partenaires organisationnels dans votre déploiement Active Directory Federation Services (ADFS) puissent collaborer efficacement, vous devez vous assurer que votre infrastructure réseau d’entreprise est configuré pour prendre en charge de la configuration requise pour AD FS pour les comptes, la résolution de noms et de certificats. AD FS présente les conditions requises suivantes:  
  
> [!TIP]  
> Vous pouvez trouver des liens de ressources AD FS supplémentaires à la [AD FS Content Map](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) page sur Microsoft TechNet Wiki. Cette page est gérée par les membres de la Communauté AD FS et est analysée régulièrement par l’équipe de produit AD FS.  
  
## <a name="hardware-requirements"></a>Configuration matérielle requise  
La configuration matérielle minimale et recommandée suivante s’appliquent aux ordinateurs de proxy de serveur de fédération et serveur de fédération.  
  
|Configuration matérielle requise|Configuration minimale requise|Configuration recommandée|  
|------------------------|-----------------------|---------------------------|  
|Vitesse du processeur|Cœur unique, 1 gigahertz (GHz)|Quadruple cœur, 2 GHz|  
|MÉMOIRE RAM|1 GO|4GO|  
|Espace disque|50 MO|100 MO|  
  
## <a name="software-requirements"></a>Configuration logicielle requise  
AD FS repose sur les fonctionnalités de serveur qui sont intégrée dans le système d’exploitation Windows Server® 2012.  
  
> [!NOTE]  
> Les services de rôle Service de fédération et Proxy du Service de fédération ne peut pas coexister sur le même ordinateur.  
  
## <a name="certificate-requirements"></a>Configuration requise des certificats  
Les certificats jouent un rôle déterminant dans la sécurisation des communications entre les serveurs de fédération, serveurs proxy de fédération, les applications prenant en charge les revendications et les clients Web. La configuration requise pour les certificats varie, selon que vous configurez un serveur de fédération ou d’un ordinateur du serveur proxy de fédération, comme décrit dans cette section.  
  
### <a name="federation-server-certificates"></a>Certificats de serveur de fédération  
Les serveurs de fédération requièrent les certificats dans le tableau suivant.  
  
|Type de certificat|Description|Ce que vous devez savoir avant de déployer|  
|--------------------|---------------|------------------------------------------|  
|Sécurité du certificat Secure Sockets Layer (SSL)|Il s’agit d’un certificat Secure Sockets Layer (SSL) standard qui est utilisé pour sécuriser les communications entre les serveurs de fédération et les clients.|Ce certificat doit être lié pour le Site Web par défaut dans Internet Information Services (IIS) pour un serveur de fédération ou un serveur Proxy de fédération.  Pour un serveur Proxy de fédération, la liaison doit être configurée dans IIS avant d’exécuter l’Assistant de Configuration de Proxy de serveur de fédération avec succès.<br /><br />**Recommandation:** , car ce certificat doit être approuvé par les clients d’AD FS, utilisez un certificat d’authentification serveur émis par une autorité de certification (tierce) publique (CA), par exemple, VeriSign. **Conseil:** le nom d’objet de ce certificat est utilisé pour représenter le nom du Service de fédération pour chaque instance d’AD FS que vous déployez. Pour cette raison, vous souhaiterez choisir un nom d’objet sur les nouveaux certificats émis par l’autorité de certification qui reflète le nom de votre entreprise ou organisation auprès des partenaires.|  
|Certificat de communication du service|Ce certificat permet de sécurité de message WCF pour sécuriser les communications entre les serveurs de fédération.|Par défaut, le certificat SSL est utilisé en tant que certificat de communication du service.  Cela peut être modifié à l’aide de la console de gestion AD FS.|  
|Certificat de signature de jetons|Il s’agit d’un X509 standard certificat qui est utilisé pour signer de façon sécurisée tous les jetons que le serveur de fédération émet.|Le certificat de signature de jetons doit contenir une clé privée, et il doit être lié à une racine approuvée dans le Service de fédération. Par défaut, AD FS crée un certificat auto-signé. Toutefois, vous pouvez modifier ce ultérieurement à un certificat émis par l’autorité de certification à l’aide du composant logiciel enfichable Gestion AD FS, selon les besoins de votre organisation.|  
|Certificat de déchiffrement de jeton|Il s’agit d’un certificat SSL standard qui est utilisé pour déchiffrer les jetons entrants chiffrés par un serveur de fédération partenaire. Il est également publié dans les métadonnées de fédération.|Par défaut, AD FS crée un certificat auto-signé. Toutefois, vous pouvez modifier ce ultérieurement à un certificat émis par l’autorité de certification à l’aide du composant logiciel enfichable Gestion AD FS, selon les besoins de votre organisation.|  
  
> [!CAUTION]  
> Les certificats qui sont utilisés pour la signature de jetons et de déchiffrement de jeton sont essentiels à la stabilité du Service de fédération. Étant donné que la perte ou la suppression non planifiée de tous les certificats qui sont configurés pour cette fin peut perturber le service, vous devez sauvegarder tous les certificats qui sont configurés à cet effet.  
  
Pour plus d’informations sur les certificats qui utilisent des serveurs de fédération, consultez [certificats requis pour les serveurs de fédération](Certificate-Requirements-for-Federation-Servers.md).  
  
### <a name="federation-server-proxy-certificates"></a>Certificats des serveurs proxy de fédération  
Serveurs proxy de fédération requièrent les certificats dans le tableau suivant.  
  
|Type de certificat|Description|Ce que vous devez savoir avant de déployer|  
|--------------------|---------------|------------------------------------------|  
|Certificat d’authentification serveur|Il s’agit d’un certificat Secure Sockets Layer (SSL) standard qui est utilisé pour sécuriser les communications entre un client Internet et un serveur proxy de fédération ordinateurs.|Ce certificat doit être lié pour le Site Web par défaut dans Internet Information Services (IIS) avant d’exécuter l’Assistant Configuration Proxy des serveurs de fédération AD FS avec succès.<br /><br />**Recommandation:** , car ce certificat doit être approuvé par les clients d’AD FS, utilisez un certificat d’authentification serveur émis par une autorité de certification (tierce) publique (CA), par exemple, VeriSign.<br /><br />**Conseil:** le nom d’objet de ce certificat est utilisé pour représenter le nom du Service de fédération pour chaque instance d’AD FS que vous déployez. Pour cette raison, vous souhaiterez choisir un nom de sujet qui reflète le nom de votre entreprise ou organisation auprès des partenaires.|  
  
Pour plus d’informations sur les certificats qui utilisent des serveurs proxy de fédération, consultez [certificats requis pour les serveurs proxy de fédération](Certificate-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="browser-requirements"></a>Configuration requise du navigateur  
Bien que tout navigateur Web actuel doté de la fonctionnalité JavaScript peut être configuré en tant que client AD FS, les pages Web qui sont fournis par défaut ont été testées que par rapport à Internet Explorer versions 7.0, 8.0 et 9.0, Mozilla Firefox 3.0 et Safari 3.1 sur Windows. JavaScript doit être activé, et les cookies doivent être activés pour la basée sur navigateur connexion et déconnexion de fonctionner correctement.  
  
L’équipe du produit AD FS chez Microsoft a testé avec succès les configurations de navigateur et le système d’exploitation dans le tableau suivant.  
  
|Navigateur|Windows7|WindowsVista|  
|-----------|-------------|-----------------|  
|Internet Explorer 7.0.|X|X|  
|Internet Explorer 8.0|X|X|  
|Internet Explorer 9.0|X|Non testé|  
|FireFox 3.0|X|X|  
|Safari 3.1|X|X|  
  
> [!NOTE]  
> AD FS prend en charge les versions 64 bits et 32 bits de tous les navigateurs indiqués dans le tableau ci-dessus.  
  
### <a name="cookies"></a>Cookies  
AD FS crée des cookies persistants et de session qui doivent être stockés sur les ordinateurs clients pour fournir la connexion, déconnexion de session (SSO) et d’autres fonctionnalités. Par conséquent, le navigateur client doit être configuré pour accepter les cookies. Les cookies qui sont utilisés pour l’authentification sont toujours les cookies de session HTTPS Secure Hypertext Transfer Protocol () qui sont écrits pour le serveur d’origine. Si le navigateur client n’est pas configuré pour autoriser ces cookies, AD FS ne peut pas fonctionner correctement. Les cookies persistants permettent de préserver l’utilisateur de sélectionner le fournisseur de revendications. Vous pouvez les désactiver à l’aide d’un paramètre de configuration dans le fichier de configuration pour les pages de connexion AD FS.  
  
Prise en charge de TLS/SSL est requis pour des raisons de sécurité.  
  
## <a name="network-requirements"></a>Configuration réseau requise  
Configuration des services réseau suivants correctement est essentielle pour la réussite du déploiement d’AD FS dans votre organisation.  
  
### <a name="tcpip-network-connectivity"></a>Connectivité réseau TCP/IP  
Pour AD FS fonctionne, une connectivité réseau TCP/IP doit exister entre le client; un contrôleur de domaine; et les ordinateurs qui hébergent le Service de fédération, le Proxy du Service de fédération (quand celui-ci est utilisé) et l’Agent Web AD FS.  
  
### <a name="dns"></a>DNS  
Le service réseau principal essentiel au fonctionnement d’AD FS, autres que les Services de domaine Active Directory (AD DS), est le système DNS (Domain Name). Quand DNS est déployé, les utilisateurs peuvent utiliser des noms d’ordinateur conviviaux faciles à retenir pour se connecter aux ordinateurs et autres ressources sur des réseaux IP.  
  
 Windows Server 2008 utilise DNS pour la résolution de nom au lieu de la résolution de noms NetBIOS WINS Windows Internet Name Service () qui a été utilisée dans les réseaux Windows NT 4.0. Il est toujours possible d’utiliser WINS pour les applications qui en ont besoin. Toutefois, les services AD DS et AD FS nécessitent la résolution de noms DNS.  
  
Le processus de configuration DNS pour prendre en charge AD FS varie, selon que:  
  
-   Votre organisation possède déjà une infrastructure DNS existante. Dans la plupart des scénarios, DNS est déjà configuré dans l’ensemble de votre réseau afin que les clients de navigateur Web de votre réseau d’entreprise aient accès à Internet. Étant donné que la résolution de nom et d’accès Internet sont les exigences d’AD FS, cette infrastructure est supposée être en place pour votre déploiement AD FS.  
  
-   Vous envisagez d’ajouter un serveur fédéré à votre réseau d’entreprise. Dans le cadre de l’authentification des utilisateurs du réseau d’entreprise, les serveurs DNS internes dans la forêt du réseau d’entreprise doivent être configurés pour retourner le nom CNAME du serveur interne qui exécute le Service de fédération. Pour plus d’informations, voir [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   Vous envisagez d’ajouter un serveur proxy fédéré à votre réseau de périmètre. Lorsque vous souhaitez authentifier des comptes d’utilisateur qui sont trouvent dans le réseau d’entreprise de votre organisation partenaire d’identité, les serveurs DNS internes dans la forêt du réseau d’entreprise doivent être configurés pour renvoyer le nom CNAME du serveur proxy de fédération interne. Pour plus d’informations sur la façon de configurer DNS pour prendre en charge l’ajout de serveurs proxy de fédération, consultez [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   Vous configurez DNS pour un environnement de laboratoire de test. Si vous envisagez d’utiliser les services ADFS dans un environnement de laboratoire de test dans lequel aucun serveur DNS de racine unique ne fait autorité, il est probable que vous devrez configurer des redirecteurs DNS afin que les requêtes de noms entre deux ou plusieurs forêts soient correctement redirigées. Pour obtenir des informations générales sur la façon de configurer un environnement de laboratoire de test AD FS, consultez [AD FS pas à pas et Guides](https://go.microsoft.com/fwlink/?LinkId=180357).  
  
## <a name="attribute-store-requirements"></a>Configuration requise pour Windows store d’attributs  
AD FS requiert au moins un magasin d’attributs à utiliser pour l’authentification des utilisateurs et l’extraction des revendications de sécurité pour ces utilisateurs. Pour obtenir la liste des attributs magasins pris en charge par AD FS, consultez [The Role of Attribute Stores](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md) dans le Guide de conception AD FS.  
  
> [!NOTE]  
> ADFS crée automatiquement un magasin d’attributs ActiveDirectory, par défaut.  
  
Configuration requise pour Windows store d’attributs dépendent de si votre organisation est tant que le partenaire de compte (hébergeant les utilisateurs fédérés) ou le partenaire de ressource (hébergeant l’application fédérée).  
  
### <a name="ad-ds"></a>SERVICES AD DS  
Pour AD FS fonctionner correctement, les contrôleurs de domaine dans l’organisation partenaire de compte ou de l’organisation partenaire de ressource doivent exécuter Windows Server 2003 SP1, Windows Server 2003 R2, Windows Server 2008 ou Windows Server 2012.  
  
Quand AD FS est installé et configuré sur un ordinateur joint au domaine, le magasin de comptes d’utilisateur Active Directory pour ce domaine est accessible en tant qu’un magasin d’attributs pouvant être sélectionnés.  
  
> [!IMPORTANT]  
> Étant donné que AD FS requiert l’installation d’Internet Information Services (IIS), nous recommandons que vous n'installez pas le logiciel AD FS sur un contrôleur de domaine dans un environnement de production pour des raisons de sécurité. Toutefois, cette configuration est prise en charge par le Service clientèle de Microsoft.  
  
#### <a name="schema-requirements"></a>Configuration requise du schéma  
AD FS ne nécessite pas de modifications de schéma ou du niveau fonctionnel dans AD DS.  
  
#### <a name="functional-level-requirements"></a>Exigences de niveau fonctionnel  
La plupart des fonctionnalités d’AD FS ne nécessitent pas de modification du niveau fonctionnel de domaine Active Directory fonctionnent correctement. Toutefois, fonctionnel de domaine Windows Server 2008 niveau ou une version ultérieure est requis pour l’authentification du certificat client fonctionner correctement si le certificat est mappé explicitement sur un compte d’utilisateur dans AD DS.  
  
#### <a name="service-account-requirements"></a>Exigences relatives aux comptes de service  
Si vous créez une batterie de serveurs de fédération, vous devez d’abord créer un compte de service de domaine dédié dans AD DS que le Service de fédération peut utiliser. Plus tard, vous configurez chaque serveur de fédération dans la batterie de serveurs à utiliser ce compte. Pour plus d’informations sur la procédure à suivre, voir [configurer manuellement un compte de Service pour une batterie de serveurs de fédération](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) dans le Guide de déploiement d’AD FS.  
  
### <a name="ldap"></a>LDAP  
Lorsque vous travaillez avec d’autres magasins d’attributs LDAP Lightweight Directory Access Protocol, vous devez vous connecter à un serveur LDAP qui prend en charge l’authentification Windows intégrée. La chaîne de connexion LDAP doit également être écrite sous la forme d’une URL LDAP, comme décrit dans le document RFC 2255.  
  
### <a name="sql-server"></a>SQLServer  
Pour AD FS fonctionner correctement, les ordinateurs qui hébergent le magasin d’attributs de langage SQL (Structured Query) Server doivent exécuter Microsoft SQL Server 2005 ou SQL Server 2008. Lorsque vous travaillez avec des magasins d’attributs SQL, vous devez également configurer une chaîne de connexion.  
  
### <a name="custom-attribute-stores"></a>Magasins d’attributs personnalisés  
Vous pouvez développer des magasins d’attributs personnalisés pour activer des scénarios avancés. Le langage de stratégie qui est intégré à AD FS peut référencer des magasins d’attributs personnalisés afin qu’un des scénarios suivants peuvent être amélioré:  
  
-   Créer des revendications pour un utilisateur authentifié localement  
  
-   Compléter les revendications pour un utilisateur authentifié de manière externe  
  
-   Autoriser un utilisateur à obtenir un jeton  
  
-   Autoriser un service pour obtenir un jeton d’un utilisateur  
  
Lorsque vous travaillez avec un magasin d’attributs personnalisés, vous devrez peut-être également configurer une chaîne de connexion. Dans ce cas, vous pouvez entrer n’importe quel code personnalisé que permettant d’établir une connexion à votre magasin d’attributs personnalisés. La chaîne de connexion dans cette situation est un ensemble de paires nom/valeur qui sont interprétés comme étant implémentées par le développeur du magasin d’attributs personnalisé.  
  
Pour plus d’informations sur le développement et à l’aide des magasins d’attributs personnalisés, voir [vue d’ensemble du Windows Store attributs](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="application-requirements"></a>Configuration requise d’application  
Les serveurs de fédération peuvent communiquer avec et protéger des applications de fédération, tels que les applications prenant en charge les revendications.  
  
## <a name="authentication-requirements"></a>Configuration requise pour l’authentification  
AD FS intègre naturellement avec l’authentification Windows existante, par exemple, l’authentification Kerberos, NTLM, les cartes à puce et les certificats côté client X.509 v3. Serveurs de fédération utilisent l’authentification Kerberos standard pour authentifier un utilisateur par rapport à un domaine. Les clients peuvent s’authentifier à l’aide de l’authentification par formulaire, l’authentification par carte à puce et l’authentification Windows intégrée, selon la configuration d’authentification.  
  
Le rôle de proxy de serveur de fédération AD FS rend possible un scénario dans lequel l’utilisateur s’authentifie en externe à l’aide de l’authentification client SSL. Vous pouvez également configurer le rôle de serveur de fédération pour exiger l’authentification client SSL, bien que généralement expérience utilisateur la plus transparente est obtenue en configurant le serveur de fédération de comptes pour l’authentification Windows intégrée. Dans cette situation, AD FS a aucun contrôle sur les informations d’identification de l’utilisateur emploie pour la connexion de bureau Windows.  
  
### <a name="smart-card-logon"></a>Connexion par carte à puce  
Bien que les services AD FS peut mettre en œuvre le type d’informations d’identification qu’il utilise pour l’authentification (mots de passe, authentification de client SSL ou authentification intégrée de Windows), ils n’appliquent pas directement l’authentification par carte à puce. Par conséquent, AD FS ne fournit pas une interface utilisateur côté client (IU) pour obtenir des informations d’identification de code confidentiel de carte à puce. Il s’agit, car les clients Windows intentionnellement ne fournissent pas de détails d’informations d’identification de l’utilisateur pour les serveurs de fédération ou de serveurs Web.  
  
### <a name="smart-card-authentication"></a>Authentification par carte à puce  
L’authentification par carte à puce utilise le protocole Kerberos pour s’authentifier auprès d’un serveur de fédération de comptes. AD FS ne peuvent pas être étendus pour ajouter de nouvelles méthodes d’authentification. Le certificat dans la carte à puce n’est pas requis pour la chaîne digne de confiance sur l’ordinateur client. Utilisation d’un certificat basé sur une carte à puce avec AD FS requiert les conditions suivantes:  
  
-   Le lecteur et le fournisseur de services de chiffrement (CSP) pour la carte à puce doivent fonctionner sur l’ordinateur sur lequel se trouve le navigateur.  
  
-   Le certificat de carte à puce doit être lié à une racine approuvée sur le serveur de fédération de compte et le serveur proxy de fédération de compte.  
  
-   Le certificat doit être mappé au compte d’utilisateur dans AD DS par une des méthodes suivantes:  
  
    -   Le nom de sujet du certificat correspond au nom unique LDAP d’un compte d’utilisateur dans AD DS.  
  
    -   L’extension altname objet du certificat possède le nom d’utilisateur principal (UPN) d’un compte d’utilisateur dans AD DS.  
  
Pour prendre en charge certaines exigences de niveau d’authentification dans certains scénarios, il est également possible de configurer AD FS pour créer une revendication qui indique la façon dont l’utilisateur a été authentifié. Une partie de confiance peut ensuite utiliser cette revendication pour prendre une décision d’autorisation.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
