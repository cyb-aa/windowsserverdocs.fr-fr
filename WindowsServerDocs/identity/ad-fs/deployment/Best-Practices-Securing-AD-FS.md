---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: Meilleures pratiques pour sécuriser AD FS et Proxy d’Application Web
description: Ce document fournit les meilleures pratiques pour la planification sécurisée et le déploiement d’Active Directory Federation Services (ADFS) et de Proxy d’Application Web.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3cc4194b562dad17d0c2021f4aaf061114e2c94b
ms.sourcegitcommit: 9bece8049b1766bd9bb0d5eb5921413a2de2ca61
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67351296"
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Meilleures pratiques pour la sécurisation d’Active Directory Federation Services


Ce document fournit les meilleures pratiques pour la planification sécurisée et le déploiement d’Active Directory Federation Services (ADFS) et de Proxy d’Application Web.  Il contient des informations sur les comportements par défaut de ces composants et les recommandations pour les configurations de sécurité supplémentaire pour une organisation de cas d’usage spécifiques en matière de sécurité.

Ce document s’applique à AD FS et WAP dans Windows Server 2012 R2 et Windows Server 2016 (version préliminaire).  Ces recommandations peuvent être utilisées si l’infrastructure est déployé dans un réseau local ou dans un environnement de cloud hébergé comme Microsoft Azure.

## <a name="standard-deployment-topology"></a>Topologie de déploiement standard
Pour le déploiement dans les environnements locaux, nous recommandons une topologie de déploiement standard consistant en un ou plusieurs serveurs AD FS sur le réseau d’entreprise interne, avec un ou plusieurs serveurs Proxy d’Application Web (WAP) dans une zone DMZ ou un réseau extranet.  Au niveau de chaque couche, AD FS et WAP, un équilibreur de charge matérielle ou logicielle est placé devant la batterie de serveurs et gère le routage du trafic.  Les pare-feux sont placés selon les besoins à l’avant de l’adresse IP externe de l’équilibreur de charge devant chaque batterie de serveurs (FS et proxy).

![Topologie de AD FS Standard](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Ports requis
Le schéma ci-dessous décrit les ports de pare-feu doivent être activés entre et parmi les composants du déploiement AD FS et WAP.  Si le déploiement n’inclut pas d’Azure AD / Office 365, les exigences de synchronisation peut être ignoré.

>Notez que le port 49443 est uniquement requis si l’authentification de certificat utilisateur est utilisée, ce qui est facultatif pour Azure AD et Office 365.

![Topologie de AD FS Standard](media/Best-Practices-Securing-AD-FS/adfssec2.png)

>[!NOTE]
> Port 808 (Windows Server 2012 R2) ou 1501 (Windows Server 2016 +) est que le port Net.TCP AD FS utilise pour le point de terminaison WCF local pour transférer des données de configuration pour le processus de service et de Powershell. Ce port peut être consulté en exécutant Get-AdfsProperties | Sélectionnez NetTcpPort. Il s’agit d’un port local qui ne devrez pas être ouvert dans le pare-feu, mais est affiché dans une analyse de port. 

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect et serveurs de fédération/WAP
Ce tableau décrit les ports et protocoles requis pour la communication entre le serveur Azure AD Connect et les serveurs de fédération/WAP.  

Protocol |Ports |Description
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Utilisé pour télécharger les listes CRL (listes de certificats) pour vérifier les certificats SSL.
HTTPS|443(TCP/UDP)|Utilisé pour synchroniser avec Azure AD.
WinRM|5985| Écouteur WinRM

### <a name="wap-and-federation-servers"></a>WAP et les serveurs de fédération
Ce tableau décrit les ports et protocoles requis pour la communication entre les serveurs de fédération et les serveurs WAP.

Protocol |Ports |Description
--------- | --------- |---------
HTTPS|443(TCP/UDP)|Utilisé pour l’authentification.

### <a name="wap-and-users"></a>WAP et utilisateurs
Ce tableau décrit les ports et protocoles requis pour la communication entre les utilisateurs et les serveurs WAP.

Protocol |Ports |Description
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|Utilisé pour l’authentification des appareils.
TCP|49443 (TCP)|Utilisé pour l’authentification par certificat.

Pour plus d’informations sur les ports et protocoles requis pour les déploiements hybrides, consultez le document nécessaires [ici](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-ports/).

Pour plus d’informations sur les ports et protocoles requis pour un compte Azure AD et le déploiement d’Office 365, consultez le document [ici](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Points de terminaison activés

Quand AD FS et WAP sont installés, un ensemble de points de terminaison AD FS par défaut sont activées sur le service de fédération et sur le serveur proxy.  Ces valeurs par défaut ont été choisis en fonction des scénarios les plus couramment requises et utilisés et il n’est pas nécessaire de les modifier.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[Facultatif] Ensemble de min de proxy de points de terminaison est activé pour Azure AD / Office 365
Les organisations qui déploient des services AD FS et WAP uniquement pour Azure AD et scénarios Office 365 peuvent limiter le nombre de points de terminaison AD FS est activé sur le serveur proxy pour atteindre une surface d’attaque minimale plus encore.
Voici la liste des points de terminaison qui doit être activé sur le proxy dans ces scénarios :

|Point de terminaison|Objectif
|-----|-----
|/adfs/ls|Flux d’authentification basées sur un navigateur et les versions actuelles de Microsoft Office utilisent ce point de terminaison pour Azure AD et l’authentification Office 365
|/adfs/services/trust/2005/usernamemixed|Utilisé pour Exchange Online avec les clients Office plus anciennes que la mise à jour d’Office 2013 mai 2015.  Les clients ultérieures utilisent le point de terminaison \adfs\ls passif.
|/adfs/services/trust/13/usernamemixed|Utilisé pour Exchange Online avec les clients Office plus anciennes que la mise à jour d’Office 2013 mai 2015.  Les clients ultérieures utilisent le point de terminaison \adfs\ls passif.
|/adfs/oauth2|Celui-ci est utilisé pour toutes les applications modernes (localement ou dans le cloud) que vous avez configuré pour s’authentifier directement auprès d’AD FS (autrement dit, pas via AAD)
|/adfs/services/trust/mex|Utilisé pour Exchange Online avec les clients Office plus anciennes que la mise à jour d’Office 2013 mai 2015.  Les clients ultérieures utilisent le point de terminaison \adfs\ls passif.
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |Configuration requise pour n’importe quel flux passif ; et utilisée par Office 365 / Azure AD pour vérifier les certificats AD FS


Points de terminaison AD FS peuvent être désactivées sur le proxy à l’aide de l’applet de commande PowerShell suivante :
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Exemple :
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Protection étendue pour l’authentification
La protection étendue pour l’authentification est une fonctionnalité qui permet d’atténuer contre les attaques de l’intercepteur (MITM) l’homme et est activée par défaut avec AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Pour vérifier les paramètres, vous pouvez procédez comme suit :
Le paramètre peut être vérifié à l’aide de la ci-dessous l’applet de commande PowerShell.  
    
   `PS:\>Get-ADFSProperties`

La propriété est `ExtendedProtectionTokenCheck`.  Le paramètre par défaut est autoriser, afin que les avantages de sécurité est possible sans les problèmes de compatibilité avec les navigateurs qui ne prennent pas en charge la fonctionnalité.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Contrôle de congestion pour protéger le service de fédération
Le proxy de federation service (partie de WAP) fournit un contrôle de congestion pour protéger le service AD FS à partir d’un flux excessif de demandes.  Le Proxy d’Application Web rejette les demandes d’authentification client externe si le serveur de fédération est surchargé par la latence entre le Proxy d’Application Web et le serveur de fédération.  Cette fonctionnalité est configurée par défaut avec un niveau de seuil de latence recommandée.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Pour vérifier les paramètres, vous pouvez procédez comme suit :
1.  Sur votre ordinateur de Proxy d’Application Web, démarrez une fenêtre de commandes avec élévation de privilèges.
2.  Accédez au répertoire ADFS, % WINDIR%\adfs\config.
3.  Modifier les paramètres de contrôle de congestion dans ses valeurs par défaut pour '<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />'.
4.  Enregistrez et fermez le fichier.
5.  Redémarrez le service AD FS en exécutant « net stop adfssrv », puis « net start adfssrv ».
Pour référence, trouverez des instructions sur cette fonctionnalité [ici](https://msdn.microsoft.com/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Requête HTTP standard vérifie au niveau du proxy
Le proxy effectue également les vérifications standards suivantes par rapport à tout le trafic :

- Le FS-P lui-même s’authentifie auprès d’AD FS via un certificat de courte durée de vie.  Dans un scénario de suspicion d’une compromission des serveurs de réseau de périmètre, AD FS peut « révoquer l’approbation de proxy » afin qu’il n’est plus approuve les demandes entrantes de potentiellement compromis proxys. Révocation de l’approbation de proxy révoque chaque certificat du proxy afin qu’il ne peut pas s’authentifier avec succès à n’importe quel effet sur le serveur AD FS
- Le FS-P met fin à toutes les connexions et crée une connexion HTTP au service AD FS sur le réseau interne. Cela fournit une mémoire tampon au niveau de la session entre les périphériques externes et le service AD FS. Le périphérique externe se connecte jamais directement au service AD FS.
- Le FS-P effectue une validation de demande HTTP qui exclut spécifiquement les en-têtes HTTP qui ne sont pas requises par le service AD FS.

## <a name="recommended-security-configurations"></a>Configurations de sécurité recommandées
Garantir que tous les serveurs AD FS et WAP reçoivent les mises à jour plus récente que la recommandation de sécurité plus importantes pour votre infrastructure AD FS est pour vous assurer un moyen en place pour maintenir à jour avec toutes les mises à jour de sécurité, ainsi que ces facultatif vos serveurs AD FS et WAP mises à jour spécifiés comme important pour AD FS sur cette page.

La méthode recommandée pour les clients Azure AD surveiller et maintenir à jour leur infrastructure est via Azure AD Connect Health pour AD FS, une fonctionnalité d’Azure AD Premium.  Azure AD Connect Health inclut des analyses et des alertes qui se déclenchent si une machine AD FS ou WAP ou il manque une des mises à jour importantes spécifiquement pour AD FS et WAP.

Informations sur l’installation d’Azure AD Connect Health pour AD FS se trouvent [ici](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configurations de sécurité supplémentaires
Les fonctionnalités supplémentaires suivantes peuvent être configurées si vous le souhaitez pour fournir des protections supplémentaires à ceux offerts dans le déploiement par défaut.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Protection par verrouillage de « soft » extranet pour les comptes
Avec la fonctionnalité de verrouillage extranet dans Windows Server 2012 R2, un administrateur de services AD FS peut définir un nombre maximal de demandes d’authentification ayant échoué (ExtranetLockoutThreshold) et une « fenêtre d’observation période (ExtranetObservationWindow). Lorsque ce nombre maximal (ExtranetLockoutThreshold) des demandes d’authentification est atteinte, AD FS cesse d’essayer d’authentifier les informations d’identification de compte fourni par rapport à AD FS pour la période définie (ExtranetObservationWindow). Cette action sécurise ce compte à partir d’un verrouillage de compte Active Directory, en d’autres termes, mais ce compte à partir de perdre l’accès aux ressources d’entreprise qui s’appuient sur AD FS pour l’authentification de l’utilisateur. Ces paramètres s’appliquent à tous les domaines capable d’authentifier le service AD FS.

Vous pouvez utiliser la commande Windows PowerShell suivante pour définir le verrouillage extranet AD FS (par exemple) : 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Pour référence, la documentation publique de cette fonctionnalité est [ici](https://technet.microsoft.com/library/dn486806.aspx ). 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Différencier les stratégies d’accès pour l’accès intranet et extranet
AD FS a la possibilité de différencier les stratégies d’accès pour les demandes qui proviennent les demandes de Visual Studio de réseau local, d’entreprise provenant d’internet via le proxy.  Cela est possible par l’application ou dans le monde entier.  Pour des applications de valeur élevée ou des applications avec des informations sensibles ou personnellement identifiables, envisagez d’exiger l’authentification multifacteur.  Cela est possible via le composant logiciel enfichable Gestion AD FS.  

### <a name="require-multi-factor-authentication-mfa"></a>Exiger l’authentification multifacteur (MFA)
AD FS peut être configuré pour exiger une authentification forte (par exemple, l’authentification multifacteur) spécifiquement pour les requêtes entrantes via le proxy pour des applications individuelles et pour l’accès conditionnel à Azure AD / Office 365 et les ressources locales.  Méthodes prises en charge de l’authentification Multifacteur incluent des fournisseurs de Microsoft Azure MFA et les tiers.  L’utilisateur est invité à fournir les informations supplémentaires (par exemple un texte SMS contenant un code de temps), et AD FS fonctionne avec le plug-in pour autoriser l’accès spécifiques au fournisseur.  

Fournisseurs d’authentification Multifacteur externes pris en charge incluent ceux répertoriés dans [cela](https://technet.microsoft.com/library/dn758113.aspx) page, mais aussi HDI Global.

### <a name="hardware-security-module-hsm"></a>Module de sécurité matériel (HSM)
Dans sa configuration par défaut, les utilisations de clés AD FS pour signer les jetons jamais laissent les serveurs de fédération sur l’intranet.  Ils ne sont jamais présents dans la zone DMZ ou sur les ordinateurs de proxy.  Si vous le souhaitez pour fournir une protection supplémentaire, ces clés peuvent être protégées dans un module de sécurité matériel attaché à AD FS.  Microsoft ne produit pas un produit HSM, mais il existe plusieurs sur le marché qui prennent en charge AD FS.  Pour implémenter cette recommandation, suivez les instructions du fournisseur pour créer le X509 certificats de signature et de chiffrement, puis utiliser AD FS installation applets de commande powershell, en spécifiant vos certificats personnalisés comme suit :

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

où :


- `CertificateThumbprint` est votre certificat SSL
- `SigningCertificateThumbprint` est votre certificat de signature (avec la clé HSM protégée)
- `DecryptionCertificateThumbprint` est votre certificat de chiffrement (avec la clé HSM protégé)



