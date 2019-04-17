---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: "Meilleures pratiques pour la sécurisation des services ADFS et le Proxy d’Application Web"
description: "Ce document fournit des meilleures pratiques pour la planification de la sécurité et le déploiement d’ActiveDirectory Federation Services (ADFS) et Proxy d’Application Web."
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6026246228b22fdea6001528ab7621a1704f1983
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Meilleures pratiques pour la sécurisation d’ActiveDirectory Federation Services

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Ce document fournit des meilleures pratiques pour la planification de la sécurité et le déploiement d’ActiveDirectory Federation Services (ADFS) et Proxy d’Application Web.  Il contient des informations sur les comportements par défaut de ces composants et les recommandations pour les configurations de sécurité supplémentaires pour une organisation avec des cas d’utilisation spécifiques en matière de sécurité.

Ce document s’applique à ADFS et WAP dans Windows Server2012R2 et Windows Server2016 (aperçu).  Ces recommandations peuvent être utilisées si l’infrastructure est déployé dans un sur le réseau local ou dans un environnement de nuage hébergé comme MicrosoftAzure.

## <a name="standard-deployment-topology"></a>Topologie de déploiement standard
Pour le déploiement dans les environnements sur site, nous vous recommandons une topologie de déploiement standard composée d’un ou plusieurs serveurs ADFS sur le réseau d’entreprise interne, avec un ou plusieurs serveurs Web Application Proxy (WAP) dans un réseau de périmètre ou un réseau extranet.  Au niveau de chaque couche, ADFS et WAP, un équilibreur de charge matériel ou logiciel est placé sur le devant de la batterie de serveurs et gère le routage du trafic.  Pare-feu sont placés en fonction des besoins au premier plan de l’adresse IP externe de l’équilibrage de charge devant chaque batterie de serveurs (FS et proxy).

![Topologie de ADFS Standard](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Ports requis
Le schéma ci-dessous illustre les ports de pare-feu qui doivent être activés entre et entre les composants du déploiement ADFS et WAP.  Si le déploiement n’inclut pas Azure AD / Office 365, les exigences de synchronisation peut être ignoré.

>Notez que le port 49443 est uniquement requise si l’authentification de certificat utilisateur est utilisée, ce qui est facultatif pour Azure AD et Office 365.

![Topologie de ADFS Standard](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect et serveurs de fédération/WAP
Le tableau suivant décrit les ports et protocoles qui sont requis pour la communication entre le serveur Azure AD Connect et les serveurs de fédération/WAP.  

Protocole |Ports |Description
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Utilisé pour télécharger les listes CRL (listes révocation de certificats) pour vérifier les certificats SSL.
HTTPS|443(TCP/UDP)|Permet de synchroniser avec Azure AD.
WinRM|5985| Écouteur WinRM

### <a name="wap-and-federation-servers"></a>WAP et les serveurs de fédération
Le tableau suivant décrit les ports et protocoles qui sont requis pour la communication entre les serveurs de fédération et WAP.

Protocole |Ports |Description
--------- | --------- |---------
HTTPS|443(TCP/UDP)|Utilisé pour l’authentification.

### <a name="wap-and-users"></a>Utilisateurs et WAP
Le tableau suivant décrit les ports et protocoles qui sont requis pour la communication entre les utilisateurs et les serveurs WAP.

Protocole |Ports |Description
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|Utilisé pour l’authentification des appareils.
TCP|49443 (TCP)|Utilisé pour l’authentification par certificat.

Pour plus d’informations sur les ports requis et les protocoles requis pour les déploiements hybrides, consultez le document [ici ](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-ports/).

Pour plus d’informations sur les ports et protocoles requis pour un Azure AD et de déploiement d’Office 365, consultez le document [ici ](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Points de terminaison activés

Lorsque ADFS et WAP sont installés, un ensemble de points de terminaison ADFS par défaut sont activés sur le service de fédération et sur le serveur proxy.  Ces valeurs par défaut ont été choisies en fonction des scénarios les plus couramment requises et utilisées et il n’est pas nécessaire de les modifier.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[Facultatif] Ensemble de min de proxy de points de terminaison activé pour Azure AD / Office 365
Organisations déployant ADFS et WAP uniquement pour Azure AD et scénarios Office 365 peuvent limiter le nombre de points de terminaison ADFS activés sur le serveur proxy pour obtenir une surface d’attaque minimale plus encore.
Voici la liste des points de terminaison qui doit être activée sur le serveur proxy dans ces scénarios:

|Point de terminaison|Objectif
|-----|-----
|/ adfs/ls|Le flux d’authentification basée sur un navigateur et les versions actuelles de MicrosoftOffice utilisent ce point de terminaison pour Azure AD et l’authentification d’Office 365
|/ADFS/services/Trust/2005/usernamemixed|Utilisé pour Exchange Online avec les clients Office antérieures à la mise à jour Office2013 mai2015.  Les clients ultérieure utilisent le point de terminaison \adfs\ls passif.
|/ADFS/services/Trust/13/usernamemixed|Utilisé pour Exchange Online avec les clients Office antérieures à la mise à jour Office2013 mai2015.  Les clients ultérieure utilisent le point de terminaison \adfs\ls passif.
|/ adfs/oauth2|Celle-ci est utilisée pour toutes les applications modernes (sur site ou dans le cloud), vous avez configuré pour s’authentifier auprès des services ADFS (et non par le biais de AAD)
|/ADFS/services/Trust/MEX|Utilisé pour Exchange Online avec les clients Office antérieures à la mise à jour Office2013 mai2015.  Les clients ultérieure utilisent le point de terminaison \adfs\ls passif.
|/ADFS/ls/FederationMetadata/2007-06/federationmetadata.Xml |Configuration requise pour n’importe quel flux passif; et utilisé par Office 365 / Azure AD pour vérifier des certificats ADFS


Les points de terminaison ADFS peuvent être désactivés sur le serveur proxy à l’aide de l’applet de commande PowerShell suivante:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Par exemple:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Protection étendue pour l’authentification
La protection étendue pour l’authentification est une fonctionnalité qui permet d’atténuer contre les attaques du milieu (intercepteur) et est activée par défaut avec ADFS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Pour vérifier les paramètres, vous pouvez procédez comme suit:
Le paramètre peut être vérifié à l’aide de la ci-dessous applet de commande PowerShell.  
    
   `PS:\>Get-ADFSProperties`

La propriété est `ExtendedProtectionTokenCheck`.  Le paramètre par défaut est autoriser, afin que les avantages de sécurité peuvent être obtenues sans les problèmes de compatibilité avec les navigateurs qui ne prennent pas en charge la fonctionnalité.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Contrôle de congestion pour protéger le service de fédération
Le proxy du service de fédération (partie de la WAP) fournit un contrôle congestion pour protéger le service ADFS à partir d’un flux excessif de requêtes.  Le Proxy d’Application Web rejette les demandes d’authentification client externe si le serveur de fédération est surchargé comme détectés par la latence entre le Proxy d’Application Web et le serveur de fédération.  Ce composant est configuré par défaut avec un niveau de seuil de latence recommandée.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Pour vérifier les paramètres, vous pouvez procédez comme suit:
1.  Sur votre ordinateur de Proxy d’Application Web, démarrez une fenêtre de commandes avec élévation de privilèges.
2.  Accédez au répertoire ADFS, à % WINDIR%\adfs\config.
3.  Modifier les paramètres de contrôle de congestion à partir de ses valeurs par défaut à «<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />».
4.  Enregistrez et fermez le fichier.
5.  Redémarrez le service ADFS en exécutant «net stop adfssrv», puis «net start adfssrv».
Pour référence, vous pouvez trouver les conseils sur cette fonctionnalité [ici](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Demande HTTP standard vérifie au niveau du proxy
Le serveur proxy effectue également les vérifications suivantes standards par rapport à tout le trafic:

- FS-P lui-même s’authentifie auprès d’ADFS via un certificat de courte durée de vie.  Dans un scénario de compromission de suspicion de serveurs de réseau de périmètre, ADFS peut «révoquer l’approbation de proxy» afin qu’il n’est plus approuve les demandes entrantes de potentiellement compromis proxys. Révoquer l’approbation de proxy révoque chaque certificat de proxy afin qu’il ne peut pas s’authentifier avec succès pour n’importe quel objet vers le serveur ADFS
- Le FS-P met fin à toutes les connexions et crée une connexion HTTP pour le service ADFS sur le réseau interne. Cela fournit une mémoire tampon de niveau session entre des appareils externes et le service ADFS. Le périphérique externe se connecte jamais directement sur le service ADFS.
- Le FS-P effectue la validation de la demande HTTP qui filtre spécifiquement les en-têtes HTTP qui ne sont pas requises par le service ADFS.

## <a name="recommended-security-configurations"></a>Configurations de sécurité recommandée
Assurez-vous que tous les services ADFS et serveurs WAP recevront les mises à jour plus récentes que la recommandation de sécurité plus importante pour votre infrastructure ADFS est pour vous assurer que vous avez un moyen en place pour conserver vos serveurs ADFS et WAP actuel avec toutes les mises à jour de sécurité, ainsi que les mises à jour facultatives spécifiés comme important pour ADFS sur cette page.

La méthode recommandée pour les clients d’Azure AD surveiller et maintenir à jour leur infrastructure est via Azure ActiveDirectory se connecter d’intégrité pour ADFS, une fonctionnalité d’Azure AD Premium.  Azure AD Health connecter inclut des moniteurs et des alertes qui déclenchent si une machine ADFS ou WAP une n’est pas les mises à jour importantes spécifiquement pour ADFS et WAP.

Informations sur l’installation de l’intégrité connecter Azure AD pour ADFS, vous pouvez trouver [ici](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configurations de sécurité supplémentaires
Les fonctionnalités supplémentaires suivantes peuvent être configurées si vous le souhaitez pour fournir des protections supplémentaires à celles proposées dans le déploiement par défaut.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Protection le verrouillage extranet «soft» pour les comptes
La fonctionnalité de verrouillage extranet dans Windows Server2012R2, un administrateur ADFS permet de définir un nombre maximal de demandes d’authentification ayant échoué (ExtranetLockoutThreshold) et un «la fenêtre d’observation laps de temps (ExtranetObservationWindow). Lorsque ce nombre maximal (ExtranetLockoutThreshold) de demandes d’authentification est atteinte, ADFS cesse d’essayer d’authentifier les informations d’identification de compte fourni par rapport à ADFS pour la période de temps défini (ExtranetObservationWindow). Cette action sécurise ce compte à partir d’un verrouillage de compte ActiveDirectory, en d’autres termes, mais ce compte à partir de perdre l’accès aux ressources d’entreprise qui s’appuient sur ADFS pour l’authentification de l’utilisateur. Ces paramètres s’appliquent à tous les domaines que le service ADFS peut authentifier.

Vous pouvez utiliser la commande Windows PowerShell suivante pour définir le verrouillage extranet ADFS (exemple): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Pour référence, la documentation publique de cette fonctionnalité est [ici](https://technet.microsoft.com/en-us/library/dn486806.aspx ). 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Différencier les stratégies d’accès pour l’intranet et l’accès extranet
ADFS a la possibilité de différencier les stratégies d’accès pour les demandes provenant des demandes de vs de réseau local, d’entreprise qui sont fournis à partir d’internet via le proxy.  Cela est possible par l’application ou globalement.  Pour des applications de valeur élevée ou des applications avec des informations sensibles ou personnelles, envisagez d’exiger l’authentification à plusieurs facteurs.  Cela est possible via le composant logiciel enfichable Gestion ADFS.  

### <a name="require-multi-factor-authentication-mfa"></a>Requièrent Multi-Factor authentication (MFA)
ADFS peut être configuré pour exiger une authentification forte (par exemple, multi-Factor authentication) spécifiquement pour les requêtes entrantes via le proxy pour des applications individuelles et pour l’accès conditionnel à deux Azure AD / Office 365 et sur les ressources locales.  Méthodes prises en charge de l’authentification Multifacteur incluent des fournisseurs tiers et MicrosoftAzure MFA.  L’utilisateur est invité à fournir les informations supplémentaires (par exemple, un texte SMS contenant un code de temps), et ADFS fonctionne avec la plug-in pour autoriser l’accès spécifique au fournisseur.  

Fournisseurs d’authentification Multifacteur externes pris en charge incluant ceux répertoriés dans [cela](https://technet.microsoft.com/en-us/library/dn758113.aspx) page, mais aussi HDI Global.

### <a name="hardware-security-module-hsm"></a>Module de sécurité matériel (HSM)
Dans sa configuration par défaut, les utilisations de clés ADFS pour signer les jetons jamais laissent les serveurs de fédération sur l’intranet.  Ils ne sont jamais présents sur le réseau de périmètre ou dans les ordinateurs de proxy.  Si vous le souhaitez pour fournir une protection supplémentaire, ces clés peuvent être protégés dans un module de sécurité matériel attaché à ADFS.  Microsoft ne produit pas d’un produit HSM, cependant, il existe plusieurs sur le marché qui prennent en charge ADFS.  Pour implémenter cette recommandation, suivez les instructions du fournisseur pour créer le X509certificats de signature et de chiffrement, puis utilisez les services ADFS installation applets de commande powershell, spécifiant vos certificats personnalisés comme suit:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

où:


- `CertificateThumbprint` Votre certificat SSL
- `SigningCertificateThumbprint` Est votre certificat de signature (avec clé HSM protégé)
- `DecryptionCertificateThumbprint` Votre certificat de chiffrement (avec clé HSM protégé)



