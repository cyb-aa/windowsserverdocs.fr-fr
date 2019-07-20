---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: Meilleures pratiques pour la sécurisation des AD FS et du proxy d’application Web
description: Ce document présente les meilleures pratiques pour la planification et le déploiement sécurisés de Services ADFS (AD FS) et du proxy d’application Web.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6939373db678f1ca6be62711f1771b8f7019c312
ms.sourcegitcommit: c9ab5fbde1782a3a2bac2dbd45f3f178f7ae3c4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68354641"
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Meilleures pratiques pour la sécurisation des Services ADFS


Ce document présente les meilleures pratiques pour la planification et le déploiement sécurisés de Services ADFS (AD FS) et du proxy d’application Web.  Il contient des informations sur les comportements par défaut de ces composants et des recommandations pour les configurations de sécurité supplémentaires pour une organisation avec des cas d’utilisation et des exigences de sécurité spécifiques.

Ce document s’applique à AD FS et WAP dans Windows Server 2012 R2 et Windows Server 2016 (version préliminaire).  Ces recommandations peuvent être utilisées si l’infrastructure est déployée sur un réseau local ou dans un environnement hébergé dans le Cloud tel que Microsoft Azure.

## <a name="standard-deployment-topology"></a>Topologie de déploiement standard
Pour le déploiement dans des environnements locaux, nous vous recommandons une topologie de déploiement standard composée d’un ou de plusieurs serveurs AD FS sur le réseau d’entreprise interne, avec un ou plusieurs serveurs de proxy d’application Web (WAP) dans un réseau DMZ ou extranet.  À chaque couche, AD FS et WAP, un équilibreur de charge matériel ou logiciel est placé devant la batterie de serveurs et gère le routage du trafic.  Les pare-feu sont placés comme requis devant l’adresse IP externe de l’équilibreur de charge devant chaque batterie (FS et proxy).

![Topologie de AD FS standard](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Ports requis
Le diagramme ci-dessous représente les ports de pare-feu qui doivent être activés entre et parmi les composants de la AD FS et du déploiement WAP.  Si le déploiement n’inclut pas Azure AD/Office 365, les exigences de synchronisation peuvent être ignorées.

>Notez que le port 49443 est uniquement requis si l’authentification par certificat utilisateur est utilisée, ce qui est facultatif pour Azure AD et Office 365.

![Topologie de AD FS standard](media/Best-Practices-Securing-AD-FS/adfssec2.png)

>[!NOTE]
> Le port 808 (Windows Server 2012 R2) ou le port 1501 (Windows Server 2016 +) est le port Net. TCP AD FS utilisé par le point de terminaison WCF local pour transférer des données de configuration vers le processus de service et PowerShell. Ce port peut être consulté en exécutant la AdfsProperties Sélectionnez NetTcpPort. Il s’agit d’un port local qui n’a pas besoin d’être ouvert dans le pare-feu, mais qui sera affiché dans une analyse de port. 

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect et serveurs de Fédération/WAP
Ce tableau décrit les ports et les protocoles requis pour la communication entre le serveur Azure AD Connect et les serveurs de Fédération/WAP.  

Protocol |Ports |Description
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Utilisé pour télécharger les listes de révocation de certificats pour vérifier les certificats SSL.
HTTPS|443 (TCP/UDP)|Utilisé pour la synchronisation avec Azure AD.
WinRM|5985| Écouteur WinRM

### <a name="wap-and-federation-servers"></a>WAP et serveurs de Fédération
Ce tableau décrit les ports et les protocoles requis pour la communication entre les serveurs de Fédération et les serveurs WAP.

Protocol |Ports |Description
--------- | --------- |---------
HTTPS|443 (TCP/UDP)|Utilisé pour l’authentification.

### <a name="wap-and-users"></a>WAP et utilisateurs
Ce tableau décrit les ports et les protocoles requis pour la communication entre les utilisateurs et les serveurs WAP.

Protocol |Ports |Description
--------- | --------- |--------- |
HTTPS|443 (TCP/UDP)|Utilisé pour l’authentification de l’appareil.
TCP|49443 (TCP)|Utilisé pour l’authentification par certificat.

Pour plus d’informations sur les ports requis et les protocoles requis pour les déploiements hybrides, consultez le document [ici](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-ports/).

Pour plus d’informations sur les ports et les protocoles requis pour un déploiement Azure AD et Office 365, voir le document [ici](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Points de terminaison activés

Lorsque AD FS et WAP sont installés, un ensemble de points de terminaison de AD FS par défaut sont activés sur le service de Fédération et sur le proxy.  Ces valeurs par défaut ont été choisies en fonction des scénarios les plus couramment utilisés et utilisés, et il n’est pas nécessaire de les modifier.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>Facultatif Ensemble minimal de points de terminaison activés pour Azure AD/Office 365
Les organisations qui déploient AD FS et WAP uniquement pour les scénarios Azure AD et Office 365 peuvent limiter encore davantage le nombre de points de terminaison de AD FS activés sur le proxy pour atteindre une surface d’attaque plus minimale.
Voici la liste des points de terminaison qui doivent être activés sur le proxy dans les scénarios suivants:

|Point de terminaison|Objectif
|-----|-----
|/ADFS/ls|Flux d’authentification basés sur un navigateur et versions actuelles de Microsoft Office utilisent ce point de terminaison pour l’authentification Azure AD et Office 365
|/adfs/services/trust/2005/usernamemixed|Utilisé pour Exchange Online avec les clients Office antérieurs à Office 2013 mai 2015 Update.  Les clients ultérieurs utilisent le point de terminaison \adfs\ls passif.
|/adfs/services/trust/13/usernamemixed|Utilisé pour Exchange Online avec les clients Office antérieurs à Office 2013 mai 2015 Update.  Les clients ultérieurs utilisent le point de terminaison \adfs\ls passif.
|/adfs/oauth2|Celui-ci est utilisé pour toutes les applications modernes (sur local ou dans le Cloud) que vous avez configurées pour l’authentification directe sur AD FS (par exemple, pas via AAD).
|/adfs/services/trust/mex|Utilisé pour Exchange Online avec les clients Office antérieurs à Office 2013 mai 2015 Update.  Les clients ultérieurs utilisent le point de terminaison \adfs\ls passif.
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |Exigence pour les flux passifs; et utilisé par Office 365/Azure AD pour vérifier AD FS certificats


AD FS points de terminaison peuvent être désactivés sur le proxy à l’aide de l’applet de commande PowerShell suivante:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Exemple :
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Protection étendue pour l’authentification
La protection étendue de l’authentification est une fonctionnalité qui atténue les attaques de l’intercepteur (intercepteur) et qui est activée par défaut avec AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Pour vérifier les paramètres, vous pouvez effectuer les opérations suivantes:
Le paramètre peut être vérifié à l’aide de l’applet de cmdlet PowerShell ci-dessous.  
    
   `PS:\>Get-ADFSProperties`

La propriété est `ExtendedProtectionTokenCheck`.  Le paramètre par défaut est autoriser, afin que les avantages de sécurité puissent être atteints sans les problèmes de compatibilité avec les navigateurs qui ne prennent pas en charge la fonctionnalité.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Contrôle de congestion pour protéger le service de Fédération
Le proxy du service de Fédération (qui fait partie du WAP) fournit un contrôle de congestion pour protéger le service de AD FS des flux de requêtes.  Le proxy d’application Web rejette les demandes d’authentification du client externe si le serveur de Fédération est surchargé comme détecté par la latence entre le proxy d’application Web et le serveur de Fédération.  Cette fonctionnalité est configurée par défaut avec un niveau de seuil de latence recommandé.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Pour vérifier les paramètres, vous pouvez effectuer les opérations suivantes:
1.  Sur votre ordinateur proxy d’application Web, démarrez une fenêtre de commande avec élévation de privilèges.
2.  Accédez au répertoire ADFS, à l’adresse%WINDIR%\adfs\config.
3.  Modifiez les valeurs par défaut des paramètres de contrôle de congestion<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />en «».
4.  Enregistrez et fermez le fichier.
5.  Redémarrez le service AD FS en exécutant «net stop adfssrv», puis «net start adfssrv».
Pour référence, vous trouverez des conseils sur cette fonctionnalité [ici](https://msdn.microsoft.com/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Vérifications des demandes HTTP standard au niveau du proxy
Le proxy effectue également les vérifications standard suivantes par rapport à l’ensemble du trafic:

- Le FS-P lui-même s’authentifie auprès de AD FS via un certificat éphémère.  Dans un scénario de compromission présumée des serveurs DMZ, AD FS pouvez révoquer l’approbation de proxy afin qu’il n’approuve plus les demandes entrantes provenant de proxys potentiellement compromis. La révocation de l’approbation de proxy révoque le propre certificat de chaque proxy afin qu’il ne puisse pas s’authentifier correctement à l’usage du serveur de AD FS
- FS-P met fin à toutes les connexions et crée une nouvelle connexion HTTP au service AD FS sur le réseau interne. Cela fournit une mémoire tampon au niveau de la session entre les périphériques externes et le service AD FS. L’appareil externe ne se connecte jamais directement au service AD FS.
- FS-P effectue une validation de requête HTTP qui filtre spécifiquement les en-têtes HTTP qui ne sont pas requis par le service AD FS.

## <a name="recommended-security-configurations"></a>Configurations de sécurité recommandées
Assurez-vous que tous les serveurs AD FS et WAP reçoivent les mises à jour les plus récentes. la recommandation de sécurité la plus importante pour votre infrastructure de AD FS consiste à vous assurer que vous disposez d’un moyen de conserver vos serveurs AD FS et WAP à jour avec toutes les mises à jour de sécurité, ainsi qu’à ceux qui sont facultatifs. mises à jour spécifiées comme importantes pour AD FS dans cette page.

La méthode recommandée pour Azure AD aux clients de surveiller et de maintenir leur infrastructure à jour consiste à utiliser Azure AD Connect Health pour AD FS, une fonctionnalité de Azure AD Premium.  Azure AD Connect Health comprend des analyses et des alertes qui déclenchent l’absence d’une des mises à jour importantes pour AD FS et WAP dans une AD FS ou un ordinateur WAP.

Vous trouverez des informations sur l’installation de Azure AD Connect Health pour AD FS [ici](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configurations de sécurité supplémentaires
Les fonctionnalités supplémentaires suivantes peuvent éventuellement être configurées pour fournir des protections supplémentaires à celles proposées dans le déploiement par défaut.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Protection du verrouillage «Soft» extranet pour les comptes
Avec la fonctionnalité de verrouillage extranet de Windows Server 2012 R2, un administrateur AD FS peut définir un nombre maximal autorisé de demandes d’authentification ayant échoué (ExtranetLockoutThreshold) et une période de temps (ExtranetObservationWindow) de la fenêtre d’observation. Lorsque ce nombre maximal (ExtranetLockoutThreshold) de demandes d’authentification est atteint, AD FS cesse d’essayer d’authentifier les informations d’identification de compte fournies par rapport AD FS pour la période définie (ExtranetObservationWindow). Cette action protège ce compte du verrouillage d’un compte Active Directory, en d’autres termes, il empêche ce compte de perdre l’accès aux ressources d’entreprise qui s’appuient sur AD FS pour l’authentification de l’utilisateur. Ces paramètres s’appliquent à tous les domaines que le service AD FS peut authentifier.

Vous pouvez utiliser la commande Windows PowerShell suivante pour définir le AD FS le verrouillage extranet (exemple): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Pour référence, la documentation publique de cette fonctionnalité est disponible [ici](https://technet.microsoft.com/library/dn486806.aspx ). 

### <a name="disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet"></a>Désactiver les points de terminaison WS-Trust Windows sur le proxy, par exemple, à partir de l’extranet

Les points de terminaison Windows WS-Trust ( */ADFS/Services/Trust/2005/windowstransport* et */ADFS/Services/Trust/13/windowstransport*) sont destinés uniquement à être des points de terminaison intranet qui utilisent la liaison WIA sur HTTPS. Les exposer à l’extranet pourraient permettre à ces points de terminaison de contourner les protections de verrouillage. Ces points de terminaison doivent être désactivés sur le proxy (c’est-à-dire désactivés de l’extranet) pour protéger le verrouillage de compte Active Directory à l’aide des commandes PowerShell suivantes. Il n’y a aucun impact d’utilisateur final connu en désactivant ces points de terminaison sur le proxy.

    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/2005/windowstransport -Proxy $false
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/windowstransport -Proxy $false
    
### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Différencier les stratégies d’accès pour l’accès intranet et extranet
AD FS a la possibilité de différencier les stratégies d’accès pour les requêtes qui proviennent des demandes de réseau local, d’entreprise par rapport à partir d’Internet via le proxy.  Cela peut être effectué par application ou globalement.  Pour les applications à valeur métier ou les applications avec des informations sensibles ou personnellement identifiables, envisagez d’exiger une authentification multifacteur.  Pour ce faire, vous pouvez utiliser le composant logiciel enfichable Gestion des AD FS.  

### <a name="require-multi-factor-authentication-mfa"></a>Exiger l’authentification multifacteur (MFA)
AD FS peut être configuré pour exiger une authentification forte (telle que l’authentification multifacteur) spécifiquement pour les demandes entrantes via le proxy, pour des applications individuelles et pour l’accès conditionnel aux ressources Azure AD/Office 365 et locales.  Les méthodes d’authentification MFA prises en charge incluent à la fois Microsoft Azure MFA et des fournisseurs tiers.  L’utilisateur est invité à fournir des informations supplémentaires (par exemple, un texte SMS contenant un code unique) et AD FS utilise le plug-in spécifique au fournisseur pour autoriser l’accès.  

Les fournisseurs MFA externes pris en charge incluent ceux qui sont listés dans [cette](https://technet.microsoft.com/library/dn758113.aspx) page, ainsi que HDI global.

### <a name="hardware-security-module-hsm"></a>Module de sécurité matériel (HSM)
Dans sa configuration par défaut, les clés AD FS utilisées pour signer les jetons ne laissent jamais les serveurs de Fédération sur l’intranet.  Ils ne sont jamais présents dans la zone DMZ ou sur les ordinateurs proxy.  Si vous souhaitez fournir une protection supplémentaire, ces clés peuvent être protégées dans un module de sécurité matériel attaché à AD FS.  Microsoft ne produit pas de produit HSM, mais plusieurs sur le marché prennent en charge AD FS.  Pour mettre en œuvre cette recommandation, suivez les instructions du fournisseur pour créer les certificats X509 pour la signature et le chiffrement, puis utilisez le AD FS d’installation PowerShell applets, en spécifiant vos certificats personnalisés comme suit:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

où :


- `CertificateThumbprint`est votre certificat SSL
- `SigningCertificateThumbprint`est votre certificat de signature (avec une clé protégée par HSM)
- `DecryptionCertificateThumbprint`est votre certificat de chiffrement (avec une clé protégée par HSM)



