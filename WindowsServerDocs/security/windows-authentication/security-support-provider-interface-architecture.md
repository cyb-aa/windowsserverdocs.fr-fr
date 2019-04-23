---
title: Architecture de l’interface du fournisseur SSP (Security Support Provider)
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de09e099-5711-48f8-adbd-e7b8093a0336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 8b0a74089c5d8a88a380f8a56e3b9e03b84081c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827020"
---
# <a name="security-support-provider-interface-architecture"></a>Architecture de l’interface du fournisseur SSP (Security Support Provider)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de référence pour les professionnels de l’informatique décrit les protocoles d’authentification Windows qui sont utilisées au sein de l’architecture de prise en charge Interface SSPI (Security Provider).

Le Microsoft Support Interface SSPI (Security Provider) est la base pour l’authentification Windows. Applications et des services d’infrastructure qui requièrent une authentification utilisent SSPI lui pour fournir.

SSPI est l’implémentation de la sécurité Service API GSSAPI (Generic) dans les systèmes d’exploitation Windows Server. Pour plus d’informations sur GSSAPI, consultez RFC 2743 et 2744 RFC dans la base de données IETF RFC.

Les fournisseurs de prise en charge de sécurité (SSP) par défaut qui appellent des protocoles d’authentification spécifiques dans Windows sont incorporées dans l’interface SSPI en tant que DLL. Ces fournisseurs de services partagés par défaut sont décrites dans les sections suivantes. Fournisseurs supplémentaires peuvent être incorporées si elles peuvent fonctionner avec l’interface SSPI.

Comme indiqué dans l’image suivante, l’interface SSPI dans Windows fournit un mécanisme qui transporte des jetons d’authentification sur le canal de communication existant entre l’ordinateur client et le serveur. Lorsque deux ordinateurs ou périphériques doivent être authentifiés afin qu’ils puissent communiquer en toute sécurité, les demandes d’authentification sont acheminées vers l’interface SSPI, qui se termine le processus d’authentification, quel que soit le protocole réseau en cours d’utilisation. L’interface SSPI retourne des objets volumineux binaires transparents. Celles-ci sont transmises entre les applications, à quel point il peuvent être passés à la couche SSPI. Par conséquent, l’interface SSPI permet à une application d’utiliser différents modèles de sécurité disponibles sur un ordinateur ou un réseau sans modifier l’interface au système de sécurité.

![Diagramme montrant l’Architecture de Interface du fournisseur de prise en charge de sécurité](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

Les sections suivantes décrivent le SSP par défaut qui interagissent avec l’interface SSPI. Les fournisseurs de services partagés sont utilisés de différentes façons dans les systèmes d’exploitation Windows pour promouvoir une communication sécurisée dans un environnement réseau non sécurisé.

-   [Fournisseur SSP Kerberos](#BKMK_KerbSSP)

-   [Fournisseur de prise en charge de sécurité NTLM](#BKMK_NTLMSSP)

-   [Digest Security Support Provider](#BKMK_DigestSSP)

-   [Fournisseur SSP Schannel](#BKMK_SchannelSSP)

-   [Négocier Security Support Provider](#BKMK_NegoSSP)

-   [Credential Security Support Provider](#BKMK_CredSSP)

-   [Négocier les Extensions Security Support Provider](#BKMK_NegoExtsSSP)

-   [PKU2U Security Support Provider](#BKMK_PKU2USSP)

Également inclus dans cette rubrique :

[Sélection de fournisseur de prise en charge de sécurité](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Fournisseur SSP Kerberos
Ce portail libre-service utilise uniquement le protocole Kerberos version 5 comme implémentées par Microsoft. Ce protocole est basé sur du réseau Working Group RFC 4120 et révisions de brouillon. Il est un protocole standard qui est utilisé avec un mot de passe ou une carte à puce pour une ouverture de session interactive. Il est également la méthode d’authentification recommandée pour les services dans Windows.

Étant donné que le protocole Kerberos est le protocole d’authentification par défaut depuis Windows 2000, tous les services de domaine prend en charge la SSP. Kerberos Ces services incluent :

-   Active Directory les requêtes qui utilisent Lightweight Directory Access Protocol (LDAP)

-   Gestion de serveur ou station de travail à distance qui utilise le service d’appel de procédure distante

-   Services d’impression

-   Authentification de client-serveur

-   Accès au fichier distant qui utilise le protocole Server Message Block (SMB) (également appelé Common Internet File System ou CIFS)

-   Gestion de système de fichiers distribués et de redirection

-   Authentification intranet à Internet Information Services (IIS)

-   Authentification d’autorité de sécurité pour Internet Protocol security (IPsec)

-   Demandes de certificat pour les Services de certificats Active Directory pour les ordinateurs et utilisateurs du domaine

Location: %windir%\Windows\System32\kerberos.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique à** figurant au début de cette rubrique, ainsi que de Windows Server 2003, de Windows XP.

**Ressources supplémentaires pour le protocole Kerberos et le SSP Kerberos**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: Extensions du protocole Kerberos](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: extensions du protocole Kerberos : Service pour l’utilisateur et la spécification du protocole de la délégation contrainte](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Kerberos SSP/AP (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Améliorations liées à Kerberos](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx) pour Windows Vista

-   [Modifications apportées à l’authentification Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) pour Windows 7 

-   [Référence technique de l’authentification Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>Fournisseur de prise en charge de sécurité NTLM
NTLM Security Support Provider (SSP NTLM) est un fichier binaire utilisé par la prise en charge Interface SSPI (Security Provider) pour autoriser l’authentification stimulation / réponse NTLM et négocier les options de l’intégrité et la confidentialité de protocole de messagerie. NTLM est utilisé partout où l’authentification SSPI est utilisée, y compris pour Server Message Block ou CIFS l’authentification, l’authentification Negotiate HTTP (par exemple, l’authentification Web Internet) et le service d’appel de procédure distante. NTLM SSP inclut le NTLM et NTLM version 2 (NTLMv2) protocoles d’authentification.

Les systèmes d’exploitation Windows pris en charge peuvent utiliser le SSP NTLM pour les éléments suivants :

-   Authentification client/serveur

-   Services d’impression

-   Accès aux fichiers à l’aide de CIFS (SMB)

-   Sécuriser le service d’appel de procédure distante ou un service DCOM

Emplacement : %windir%\Windows\System32\msv1_0.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique à** figurant au début de cette rubrique, ainsi que de Windows Server 2003, de Windows XP.

**Ressources supplémentaires pour le protocole NTLM et NTLM SSP**

-   [Package d’authentification Msv1_0 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [Modifications apportées à l’authentification NTLM](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) dans Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [L’audit et en limitant le guide d’utilisation NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>Digest Security Support Provider
L’authentification Digest est un standard qui est utilisé pour Lightweight Directory Access Protocol (LDAP) et l’authentification web. L’authentification Digest transmet les informations d’identification sur le réseau en tant qu’un résumé de message ou de hachage MD5.

Digest SSP (Wdigest.dll) est utilisé pour les éléments suivants :

-   Accès Internet Explorer et Internet Information Services (IIS)

-   Requêtes LDAP

Emplacement : %windir%\Windows\System32\Digest.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique à** figurant au début de cette rubrique, ainsi que de Windows Server 2003, de Windows XP.

**Ressources supplémentaires pour le protocole Digest et le fournisseur SSP Digest**

-   [Authentification Microsoft Digest (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-RPDB\]: Extensions du protocole Digest](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Fournisseur SSP Schannel
Le Kit de développement Secure Channel (Schannel) est utilisé pour l’authentification de serveur web, par exemple lorsqu’un utilisateur tente d’accéder à un serveur web sécurisé.

Le protocole TLS, le protocole SSL, le protocole de la technologie PCT (Private Communications) et le protocole de couche DTLS (Datagram Transport) sont basées sur le chiffrement à clé publique. Schannel fournit tous ces protocoles. Tous les protocoles Schannel utilisent un modèle client/serveur. Le SSP Schannel utilise des certificats de clé publique pour authentifier les tiers. Lors de l’authentification des tiers, le SSP Schannel sélectionne un protocole dans l’ordre de préférence suivant :

-   Transport Layer Security (TLS) version 1.0

-   Transport Layer Security (TLS) version 1.1

-   Transport Layer Security (TLS) version 1.2

-   Secure Socket Layer (SSL) version 2.0

-   Secure Socket Layer (SSL) version 3.0

-   Private Communications Technology (PCT)

    **Remarque** PCT est désactivé par défaut.

Le protocole sélectionné est le protocole d’authentification préféré que le client et le serveur peuvent prendre en charge. Par exemple, si un serveur prend en charge tous les protocoles Schannel et que le client prend en charge SSL 3.0 et SSL 2.0, le processus d’authentification utilise SSL 3.0.

Protocole DTLS est utilisée lorsqu’elle est explicitement appelée par l’application. Pour plus d’informations sur le protocole DTLS et les autres protocoles qui sont utilisés par le fournisseur Schannel, consultez [référence technique du fournisseur de prise en charge de sécurité Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Emplacement : %windir%\Windows\System32\Schannel.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique à** figurant au début de cette rubrique, ainsi que de Windows Server 2003, de Windows XP.

> [!NOTE]
> TLS 1.2 a été introduite dans ce fournisseur dans Windows Server 2008 R2 et Windows 7. Protocole DTLS a été introduite dans ce fournisseur dans Windows Server 2012 et Windows 8.

**Ressources supplémentaires pour les protocoles TLS et SSL et le SSP Schannel**

-   [Canal sécurisé (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [Référence technique de TLS/SSL](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: Profil de transport Layer Security (TLS)](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>Négocier Security Support Provider
Le Simple SPNEGO and Protected GSS-API Negotiation mécanisme () constitue la base du SSP Negotiate, whichcan utilisée pour négocier un protocole d’authentification spécifique. Lorsqu’une application appelle SSPI pour vous connecter à un réseau, il peut spécifier un SSP pour traiter la demande. Si l’application spécifie le fournisseur SSP Negotiate, il analyse la demande et choisit le fournisseur approprié pour gérer la demande, en fonction des stratégies de sécurité configurée par le client.

SPNEGO est spécifié dans la RFC 2478.

Dans les versions prises en charge des systèmes d’exploitation Windows, la sécurité Negotiate prennent en charge sélectionne de fournisseur entre le protocole Kerberos et NTLM. Sélectionne de négociation du protocole Kerberos par défaut, sauf si ce protocole ne peut pas être utilisé par un des systèmes impliqués dans l’authentification ou l’application appelante n’a pas fourni suffisamment d’informations pour utiliser le protocole Kerberos.

Location: %windir%\Windows\System32\lsasrv.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique à** figurant au début de cette rubrique, ainsi que de Windows Server 2003, de Windows XP.

**Ressources supplémentaires pour le fournisseur SSP négocier**

-   [Microsoft négocier (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: Extensions de GSS-API simple et protégés négociation mécanisme SPNEGO)](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: Négocier et spécification du protocole d’authentification HTTP de Nego2](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>Credential Security Support Provider
Le fournisseur de services de sécurité d’informations d’identification (CredSSP) fournit une unique (SSO) utilisateur expérience d’authentification lors du démarrage de nouvelles sessions de Services Terminal Server et des Services Bureau à distance. CredSSP permet aux applications de déléguer les informations d’identification des utilisateurs à partir de l’ordinateur client (en utilisant le fournisseur SSP côté client) sur le serveur cible (via le SSP côté serveur), selon les stratégies du client. CredSSP stratégies sont configurées à l’aide de stratégie de groupe, et la délégation des informations d’identification est désactivée par défaut.

Location: %windir%\Windows\System32\credssp.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique à** figurant au début de cette rubrique.

**Ressources supplémentaires pour le fournisseur SSP informations d’identification**

-   [\[MS-CSSP\]: Spécification du protocole (CredSSP) de fournisseur d’informations d’identification sécurité prise en charge](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Informations d’identification de fournisseur de services de sécurité et de l’authentification unique pour Terminal Services d’ouverture de session](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>Négocier les Extensions Security Support Provider
Négocier des Extensions (NegoExts) est un package d’authentification qui négocie l’utilisation de fournisseurs de services partagés, autre que le protocole Kerberos ou NTLM pour les applications et scénarios implémentée par les logiciels Microsoft et autres entreprises.

Cette extension pour le package Negotiate permet les scénarios suivants :

-   **Disponibilité de client riche au sein d’un système fédéré.** Les documents sont accessibles sur les sites SharePoint, et ils peuvent être modifiés à l’aide d’une application complète de Microsoft Office.

-   **Client riche prise en charge pour les services de Microsoft Office.** Les utilisateurs peuvent se connecter aux services de Microsoft Office et utiliser une application complète de Microsoft Office.

-   **Microsoft Exchange Server hébergé et Outlook.** Il n’existe aucune relation d’approbation domaine, car le serveur Exchange est hébergé sur le web. Outlook utilise le service Windows Live pour authentifier les utilisateurs.

-   **Disponibilité de client riche entre les ordinateurs clients et serveurs.** Les composants de mise en réseau et l’authentification du système d’exploitation sont utilisés.

Le package Windows Negotiate traite le SSP NegoExts de la même manière, comme il le fait pour Kerberos et NTLM. NegoExts.dll est chargé dans l’autorité système locale (LSA) au démarrage. Lorsqu’une demande d’authentification est reçue, selon la source de la demande, NegoExts négocie entre le SSP pris en charge. Il rassemble les informations d’identification et les stratégies, les chiffre et envoie ces informations pour le fournisseur SSP approprié, où le jeton de sécurité est créé.

Le SSP pris en charge par NegoExts n’est pas autonomes SSP tels que Kerberos et NTLM. Par conséquent, dans le SSP NegoExts, en cas d’échec de la méthode d’authentification pour une raison quelconque, un message d’échec d’authentification est affiché ou connecté. Aucune méthode d’authentification de secours ou renégociation n’est possibles.

Emplacement : %windir%\Windows\System32\negoexts.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique à** figurant au début de cette rubrique, à l’exclusion de Windows Server 2008 et Windows Vista.

### <a name="BKMK_PKU2USSP"></a>PKU2U Security Support Provider
Le protocole de PKU2U a été introduit et est implémenté comme un fournisseur de services partagés dans Windows 7 et Windows Server 2008 R2. Fournisseur de services partagés permet une authentification peer-to-peer, en particulier via le support et une fonctionnalité appelée groupe résidentiel, ce qui a été introduit dans Windows 7 de partage de fichiers. La fonctionnalité permet le partage entre les ordinateurs qui ne sont pas membres d’un domaine.

Location: %windir%\Windows\System32\pku2u.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique à** figurant au début de cette rubrique, à l’exclusion de Windows Server 2008 et Windows Vista.

**Ressources supplémentaires pour le protocole de PKU2U et le fournisseur SSP PKU2U**

-   [Présentation de l’intégration d’identité en ligne](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>Sélection de fournisseur de prise en charge de sécurité
L’interface SSPI Windows peuvent utiliser les protocoles qui sont pris en charge via les fournisseurs de prise en charge de sécurité installés. Toutefois, pas tous les systèmes d’exploitation prenant en charge les mêmes packages SSP en tant que chaque ordinateur exécutant Windows Server, les clients et serveurs doivent négocier pour utiliser un protocole qui prennent tous deux en charge. Windows Server préfère les ordinateurs clients et les applications d’utiliser le protocole Kerberos, un protocole basé sur ses normes fort, lorsque possible, mais le système d’exploitation continue pour permettre aux ordinateurs client et client applications ne prenant pas en charge du protocole Kerberos protocole à authentifier.

Avant que l’authentification puisse avoir communication place les deux ordinateurs doivent s’accorder sur un protocole que les deux peuvent prendre en charge. Pour n’importe quel protocole être utilisable par le biais de l’interface SSPI, chaque ordinateur doit disposer du fournisseur approprié. Par exemple, pour un ordinateur client et le serveur à utiliser le protocole d’authentification Kerberos, ils doivent tous deux prennent en charge Kerberos v5. Windows Server utilise la fonction **EnumerateSecurityPackages** pour identifier les fournisseurs de services partagés sont pris en charge sur un ordinateur et ce que sont les fonctionnalités de ces fournisseurs de services partagés.

La sélection d’un protocole d’authentification peut être gérée dans une des deux manières suivantes :

1.  [Protocole d’authentification unique](#BKMK_SingleAuth)

2.  [Option de négocier](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>Protocole d’authentification unique
Lorsqu’un seul protocole acceptable est spécifié sur le serveur, l’ordinateur client doit prendre en charge le protocole spécifié ou l’échec de communication. Lorsqu’un seul protocole acceptable est spécifié, l’échange d’authentification a lieu comme suit :

1.  L’ordinateur client demande l’accès à un service.

2.  Le serveur répond à la demande et spécifie le protocole qui sera utilisé.

3.  L’ordinateur client examine le contenu de la réponse et vérifications pour déterminer si elle prend en charge le protocole spécifié. Si l’ordinateur client prend en charge le protocole spécifié, l’authentification se poursuit. Si l’ordinateur client ne prend pas en charge le protocole, l’authentification échoue, indépendamment de si l’ordinateur client est autorisé à accéder à la ressource.

### <a name="BKMK_Negotiate"></a>Option de négocier
L’option de négociation peut être utilisée pour permettre au client et le serveur tente de trouver un protocole approprié. Cela est basé sur la Simple SPNEGO and Protected GSS-API Negotiation mécanisme (). Lorsque l’authentification commence avec l’option de négocier un protocole d’authentification, l’échange SPNEGO a lieu comme suit :

1.  L’ordinateur client demande l’accès à un service.

2.  Le serveur répond avec une liste de protocoles d’authentification qui prend en charge et une stimulation d’authentification ou la réponse, selon le protocole qui est son premier choix. Par exemple, le serveur peut répertorier le protocole Kerberos et NTLM et envoie une réponse d’authentification Kerberos.

3.  L’ordinateur client examine le contenu de la réponse et vérifications pour déterminer si elle prend en charge les protocoles spécifiés.

    -   Si l’ordinateur client prend en charge le protocole par défaut, l’authentification s’effectue.

    -   Si l’ordinateur client ne prend pas en charge le protocole par défaut, mais il prend en charge un des autres protocoles répertoriés par le serveur, l’ordinateur client informe le serveur qui poursuit de protocole d’authentification pris en charge et l’authentification.

    -   Si l’ordinateur client ne prend pas en charge des protocoles de la liste, l’échange d’authentification échoue.

## <a name="see-also"></a>Voir aussi
[Architecture d’authentification Windows](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


