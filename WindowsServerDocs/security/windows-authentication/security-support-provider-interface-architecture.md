---
title: Architecture de l’interface du fournisseur SSP (Security Support Provider)
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: de09e099-5711-48f8-adbd-e7b8093a0336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 89e6696c286cae7c3e89346d2044869082cdd8bc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861732"
---
# <a name="security-support-provider-interface-architecture"></a>Architecture de l’interface du fournisseur SSP (Security Support Provider)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de référence destinée aux professionnels de l’informatique décrit les protocoles d’authentification Windows utilisés dans l’architecture SSPI (Security Support Provider Interface).

L’interface SSPI (Security Support Provider Interface) de Microsoft est la base de l’authentification Windows. Les applications et les services d’infrastructure qui requièrent l’authentification utilisent l’interface SSPI pour la fournir.

SSPI est l’implémentation de l’API de service de sécurité générique (GSSAPI) dans les systèmes d’exploitation Windows Server. Pour plus d’informations sur GSSAPI, consultez RFC 2743 et RFC 2744 dans la base de données RFC de l’IETF.

Les fournisseurs SSP (Security Support Provider) par défaut qui appellent des protocoles d’authentification spécifiques dans Windows sont incorporés dans l’interface SSPI en tant que dll. Ces SSP par défaut sont décrits dans les sections suivantes. Des SSP supplémentaires peuvent être incorporés s’ils peuvent fonctionner avec l’interface SSPI.

Comme indiqué dans l’image suivante, l’interface SSPI dans Windows fournit un mécanisme qui transmet des jetons d’authentification sur le canal de communication existant entre l’ordinateur client et le serveur. Lorsque deux ordinateurs ou appareils doivent être authentifiés pour pouvoir communiquer en toute sécurité, les demandes d’authentification sont acheminées vers l’interface SSPI, qui achève le processus d’authentification, quel que soit le protocole réseau en cours d’utilisation. L’interface SSPI retourne des objets binaires volumineux transparents. Celles-ci sont transmises entre les applications, où elles peuvent être passées à la couche SSPI. Par conséquent, l’interface SSPI permet à une application d’utiliser différents modèles de sécurité disponibles sur un ordinateur ou un réseau sans modifier l’interface du système de sécurité.

![Diagramme montrant l’architecture de l’interface du fournisseur de support de sécurité](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

Les sections suivantes décrivent les SSP par défaut qui interagissent avec l’interface SSPI. Les SSP sont utilisés de différentes façons dans les systèmes d’exploitation Windows pour promouvoir la communication sécurisée dans un environnement réseau non sécurisé.

-   [Fournisseur de support de sécurité Kerberos](#BKMK_KerbSSP)

-   [Fournisseur de support de sécurité NTLM](#BKMK_NTLMSSP)

-   [Fournisseur de support de sécurité Digest](#BKMK_DigestSSP)

-   [Fournisseur de support de sécurité Schannel](#BKMK_SchannelSSP)

-   [Fournisseur de support de sécurité Negotiate](#BKMK_NegoSSP)

-   [Fournisseur de support de sécurité des informations d’identification](#BKMK_CredSSP)

-   [Fournisseur de support de sécurité des extensions Negotiate](#BKMK_NegoExtsSSP)

-   [Fournisseur de support de sécurité PKU2U](#BKMK_PKU2USSP)

Également inclus dans cette rubrique :

[Sélection du fournisseur de support de sécurité](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="kerberos-security-support-provider"></a><a name="BKMK_KerbSSP"></a>Fournisseur de support de sécurité Kerberos
Ce fournisseur de services partagés utilise uniquement le protocole Kerberos version 5 tel qu’il est implémenté par Microsoft. Ce protocole est basé sur le RFC 4120 du groupe de travail réseau et sur les révisions préliminaires. Il s’agit d’un protocole standard qui est utilisé avec un mot de passe ou une carte à puce pour une ouverture de session interactive. Il s’agit également de la méthode d’authentification par défaut pour les services dans Windows.

Étant donné que le protocole Kerberos est le protocole d’authentification par défaut depuis Windows 2000, tous les services de domaine prennent en charge le SSP Kerberos. Ces services incluent :

-   Active Directory des requêtes qui utilisent le protocole LDAP (Lightweight Directory Access Protocol)

-   Gestion de serveur distant ou de station de travail qui utilise le service d’appel de procédure distante

-   Services d’impression

-   Authentification client-serveur

-   Accès aux fichiers à distance qui utilise le protocole SMB (Server Message Block) (également connu sous le nom de Common Internet File System ou CIFS)

-   Gestion et référence du système de fichiers DFS

-   Authentification intranet à Internet Information Services (IIS)

-   Authentification de l’autorité de sécurité pour la sécurité du protocole Internet (IPsec)

-   Demandes de certificat pour Active Directory les services de certificats pour les utilisateurs et les ordinateurs du domaine

Emplacement :%windir%\Windows\System32\kerberos.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la liste **s’applique à** au début de cette rubrique, plus windows Server 2003 et Windows XP.

**Ressources supplémentaires pour le protocole Kerberos et le SSP Kerberos**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: extensions du protocole Kerberos](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: extensions du protocole Kerberos : spécification du protocole de service pour l’utilisateur et de délégation confrontée](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [SSP Kerberos/AP (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Améliorations de Kerberos](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx) pour Windows Vista

-   [Modifications apportées à l’authentification Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) pour Windows 7 

-   [Informations techniques de référence sur l’authentification Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="ntlm-security-support-provider"></a><a name="BKMK_NTLMSSP"></a>Fournisseur de support de sécurité NTLM
Le fournisseur de prise en charge de la sécurité NTLM (NTLM SSP) est un protocole de messagerie binaire utilisé par l’interface SSPI (Security Support Provider Interface) pour permettre l’authentification par stimulation/réponse NTLM et pour négocier les options d’intégrité et de confidentialité. NTLM est utilisé partout où l’authentification SSPI est utilisée, y compris pour l’authentification SMB (Server Message Block) ou CIFS, l’authentification HTTP Negotiate (par exemple, l’authentification Web Internet) et le service d’appel de procédure distante. Le fournisseur SSP NTLM comprend les protocoles d’authentification NTLM et NTLM version 2 (NTLMv2).

Les systèmes d’exploitation Windows pris en charge peuvent utiliser le fournisseur SSP NTLM pour les éléments suivants :

-   Authentification client/serveur

-   Services d’impression

-   Accès aux fichiers à l’aide de CIFS (SMB)

-   Service d’appel de procédure distante ou DCOM sécurisé

Emplacement :%windir%\Windows\System32\ msv1_0. dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la liste **s’applique à** au début de cette rubrique, plus windows Server 2003 et Windows XP.

**Ressources supplémentaires pour le protocole NTLM et le SSP NTLM**

-   [Package d’authentification MSV1_0 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [Modifications apportées à l’authentification NTLM](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) dans Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [Guide d’utilisation de l’audit et de la restriction de l’utilisation de NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="digest-security-support-provider"></a><a name="BKMK_DigestSSP"></a>Fournisseur de support de sécurité Digest
L’authentification Digest est une norme de l’industrie utilisée pour le protocole LDAP (Lightweight Directory Access Protocol) et l’authentification Web. L’authentification Digest transmet les informations d’identification sur le réseau sous la forme d’un hachage MD5 ou d’un message condensé.

Le SSP Digest (wdigest. dll) est utilisé pour les éléments suivants :

-   Accès Internet Explorer et Internet Information Services (IIS)

-   Requêtes LDAP

Emplacement :%windir%\Windows\System32\Digest.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la liste **s’applique à** au début de cette rubrique, plus windows Server 2003 et Windows XP.

**Ressources supplémentaires pour le protocole Digest et le SSP Digest**

-   [Authentification Microsoft Digest (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: extensions de protocole Digest](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="schannel-security-support-provider"></a><a name="BKMK_SchannelSSP"></a>Fournisseur de support de sécurité Schannel
Le canal sécurisé (SChannel) est utilisé pour l’authentification du serveur Web, par exemple lorsqu’un utilisateur tente d’accéder à un serveur Web sécurisé.

Le protocole TLS, le protocole SSL, le protocole PCT (Private Communications Technology) et le protocole DTLS (Datagram Transport Layer) sont basés sur le chiffrement à clé publique. Schannel fournit tous ces protocoles. Tous les protocoles Schannel utilisent un modèle client/serveur. Le SSP Schannel utilise des certificats de clé publique pour authentifier les tiers. Lors de l’authentification des tiers, Schannel SSP sélectionne un protocole dans l’ordre de préférence suivant :

-   TLS (Transport Layer Security) version 1,0

-   TLS (Transport Layer Security) version 1,1

-   TLS (Transport Layer Security) version 1,2

-   SSL (Secure Socket Layer) version 2,0

-   SSL (Secure Socket Layer) version 3,0

-   Technologie de communication privée (PCT)

    **Remarque** PCT est désactivé par défaut.

Le protocole sélectionné est le protocole d’authentification préféré que le client et le serveur peuvent prendre en charge. Par exemple, si un serveur prend en charge tous les protocoles Schannel et que le client prend en charge uniquement SSL 3,0 et SSL 2,0, le processus d’authentification utilise SSL 3,0.

DTLS est utilisé lors d’un appel explicite par l’application. Pour plus d’informations sur DTLS et les autres protocoles utilisés par le fournisseur Schannel, voir informations [techniques de référence sur le fournisseur de support de sécurité Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Emplacement :%windir%\Windows\System32\Schannel.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la liste **s’applique à** au début de cette rubrique, plus windows Server 2003 et Windows XP.

> [!NOTE]
> TLS 1,2 a été introduit dans ce fournisseur dans Windows Server 2008 R2 et Windows 7. DTLS a été introduit dans ce fournisseur dans Windows Server 2012 et Windows 8.

**Ressources supplémentaires pour les protocoles TLS et SSL et le SSP Schannel**

-   [Canal sécurisé (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [Informations techniques de référence sur TLS/SSL](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: profil TLS (Transport Layer Security)](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="negotiate-security-support-provider"></a><a name="BKMK_NegoSSP"></a>Fournisseur de support de sécurité Negotiate
Le mécanisme SPNEGO (simple and Protected GSS-API Negotiation Mechanism) constitue la base du SSP Negotiate, whichcan être utilisé pour négocier un protocole d’authentification spécifique. Quand une application appelle SSPI pour se connecter à un réseau, elle peut spécifier un SSP pour traiter la demande. Si l’application spécifie le SSP Negotiate, elle analyse la demande et choisit le fournisseur approprié pour gérer la demande, en fonction des stratégies de sécurité configurées par le client.

SPNEGO est spécifié dans le document RFC 2478.

Dans les versions prises en charge des systèmes d’exploitation Windows, le fournisseur de support de sécurité Negotiate effectue une sélection entre le protocole Kerberos et NTLM. Negotiate sélectionne le protocole Kerberos par défaut, sauf si ce protocole ne peut pas être utilisé par l’un des systèmes impliqués dans l’authentification, ou si l’application appelante n’a pas fourni d’informations suffisantes pour utiliser le protocole Kerberos.

Emplacement :%windir%\Windows\System32\lsasrv.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la liste **s’applique à** au début de cette rubrique, plus windows Server 2003 et Windows XP.

**Ressources supplémentaires pour le SSP Negotiate**

-   [Microsoft Negotiate (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: extensions SPNEGO (simple and Protected GSS-API Negotiation Mechanism)](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: Negotiate and Nego2 HTTP Authentication Protocol Specification](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="credential-security-support-provider"></a><a name="BKMK_CredSSP"></a>Fournisseur de support de sécurité des informations d’identification
Le fournisseur de services de sécurité des informations d’identification (CredSSP) fournit une expérience utilisateur de l’authentification unique (SSO) lors du démarrage de nouveaux services Terminal Server et de sessions de Services Bureau à distance. CredSSP permet aux applications de déléguer les informations d’identification des utilisateurs à partir de l’ordinateur client (à l’aide du fournisseur de services partagés côté client) au serveur cible (via le fournisseur de services partagés côté serveur), en fonction des stratégies du client. Les stratégies CredSSP sont configurées à l’aide de stratégie de groupe, et la délégation des informations d’identification est désactivée par défaut.

Emplacement :%windir%\Windows\System32\credssp.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la liste **s’applique à** au début de cette rubrique.

**Ressources supplémentaires pour le fournisseur de services partagés des informations d’identification**

-   [\[MS-CSSP\]: spécification du protocole CredSSP (Credential Security Support Provider)](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Fournisseur de services de sécurité des informations d’identification et SSO pour la connexion aux services Terminal Server](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="negotiate-extensions-security-support-provider"></a><a name="BKMK_NegoExtsSSP"></a>Fournisseur de support de sécurité des extensions Negotiate
Negotiate extensions (NegoExts) est un package d’authentification qui négocie l’utilisation des SSP, à l’exception de NTLM ou du protocole Kerberos, pour les applications et les scénarios implémentés par Microsoft et d’autres éditeurs de logiciels.

Cette extension du package Negotiate permet les scénarios suivants :

-   **Disponibilité client riche au sein d’un système fédéré.** Vous pouvez accéder aux documents sur les sites SharePoint et les modifier à l’aide d’une application Microsoft Office complète.

-   **Prise en charge complète des services Microsoft Office par le client.** Les utilisateurs peuvent se connecter à Microsoft Office services et utiliser une application Microsoft Office complète.

-   **Microsoft Exchange Server et Outlook hébergés.** Aucune approbation de domaine n’est établie, car Exchange Server est hébergé sur le Web. Outlook utilise le service Windows Live pour authentifier les utilisateurs.

-   **Haute disponibilité du client entre les ordinateurs clients et les serveurs.** Les composants de mise en réseau et d’authentification du système d’exploitation sont utilisés.

Le package Windows Negotiate traite le SSP NegoExts de la même manière que pour Kerberos et NTLM. NegoExts. dll est chargé dans l’autorité de système locale (LSA) au démarrage. Lorsqu’une demande d’authentification est reçue, en fonction de la source de la demande, NegoExts négocie entre les SSP pris en charge. Il recueille les informations d’identification et les stratégies, les chiffre et envoie ces informations au SSP approprié, où le jeton de sécurité est créé.

Les SSP pris en charge par NegoExts ne sont pas des SSP autonomes, tels que Kerberos et NTLM. Par conséquent, dans le SSP NegoExts, lorsque la méthode d’authentification échoue pour une raison quelconque, un message d’échec d’authentification s’affiche ou est consigné. Aucune méthode d’authentification de renégociation ou de secours n’est possible.

Emplacement :%windir%\Windows\System32\negoexts.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la liste **s’applique à** au début de cette rubrique, à l’exception de windows Server 2008 et Windows Vista.

### <a name="pku2u-security-support-provider"></a><a name="BKMK_PKU2USSP"></a>Fournisseur de support de sécurité PKU2U
Le protocole PKU2U a été introduit et implémenté en tant que fournisseur de services partagés dans Windows 7 et Windows Server 2008 R2. Ce fournisseur de services partagés permet l’authentification d’égal à égal, notamment par le biais de la fonctionnalité de partage de fichiers et de médias appelée groupe résidentiel, qui a été introduite dans Windows 7. Cette fonctionnalité permet le partage entre des ordinateurs qui ne sont pas membres d’un domaine.

Emplacement :%windir%\Windows\System32\pku2u.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la liste **s’applique à** au début de cette rubrique, à l’exception de windows Server 2008 et Windows Vista.

**Ressources supplémentaires pour le protocole PKU2U et le SSP PKU2U**

-   [Présentation de l’intégration des identités en ligne](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="security-support-provider-selection"></a><a name="BKMK_SecuritySupportProviderSelection"></a>Sélection du fournisseur de support de sécurité
L’interface SSPI Windows peut utiliser n’importe quel protocole pris en charge par les fournisseurs de support de sécurité installés. Toutefois, étant donné que tous les systèmes d’exploitation ne prennent pas en charge les mêmes packages SSP comme tout ordinateur exécutant Windows Server, les clients et les serveurs doivent négocier pour utiliser un protocole qu’ils prennent en charge. Windows Server préfère que les applications et les ordinateurs clients utilisent le protocole Kerberos, un protocole puissant basé sur les normes, lorsque cela est possible, mais le système d’exploitation continue d’autoriser les ordinateurs clients et les applications clientes qui ne prennent pas en charge le protocole Kerberos pour s’authentifier.

Pour que l’authentification puisse avoir lieu, les deux ordinateurs communicants doivent s’accorder sur un protocole qu’ils peuvent prendre en charge. Pour que tous les protocoles soient utilisables par le biais de l’interface SSPI, chaque ordinateur doit disposer du fournisseur de services partagés approprié. Par exemple, pour qu’un ordinateur client et un serveur utilisent le protocole d’authentification Kerberos, ils doivent tous deux prendre en charge Kerberos V5. Windows Server utilise la fonction **EnumerateSecurityPackages** pour identifier les SSP pris en charge sur un ordinateur et les fonctionnalités de ces ssp.

La sélection d’un protocole d’authentification peut être gérée de l’une des deux manières suivantes :

1.  [Protocole d’authentification unique](#BKMK_SingleAuth)

2.  [Option Negotiate](#BKMK_Negotiate)

### <a name="single-authentication-protocol"></a><a name="BKMK_SingleAuth"></a>Protocole d’authentification unique
Lorsqu’un seul protocole acceptable est spécifié sur le serveur, l’ordinateur client doit prendre en charge le protocole spécifié ou la communication échoue. Lorsqu’un seul protocole acceptable est spécifié, l’échange d’authentification s’effectue comme suit :

1.  L’ordinateur client demande l’accès à un service.

2.  Le serveur répond à la demande et spécifie le protocole qui sera utilisé.

3.  L’ordinateur client examine le contenu de la réponse et vérifie s’il prend en charge le protocole spécifié. Si l’ordinateur client prend en charge le protocole spécifié, l’authentification se poursuit. Si l’ordinateur client ne prend pas en charge le protocole, l’authentification échoue, que l’ordinateur client soit autorisé ou non à accéder à la ressource.

### <a name="negotiate-option"></a><a name="BKMK_Negotiate"></a>Option Negotiate
L’option Negotiate peut être utilisée pour permettre au client et au serveur d’essayer de trouver un protocole acceptable. Cela est basé sur le mécanisme SPNEGO (simple and Protected GSS-API Negotiation Mechanism). Lorsque l’authentification commence par l’option de négociation pour un protocole d’authentification, l’échange SPNEGO s’effectue comme suit :

1.  L’ordinateur client demande l’accès à un service.

2.  Le serveur répond avec une liste de protocoles d’authentification qu’il peut prendre en charge et une stimulation ou une réponse d’authentification, en fonction du protocole qui est son premier choix. Par exemple, le serveur peut répertorier le protocole Kerberos et NTLM, et envoyer une réponse d’authentification Kerberos.

3.  L’ordinateur client examine le contenu de la réponse et effectue des vérifications pour déterminer s’il prend en charge les protocoles spécifiés.

    -   Si l’ordinateur client prend en charge le protocole préféré, l’authentification se poursuit.

    -   Si l’ordinateur client ne prend pas en charge le protocole préféré, mais qu’il prend en charge l’un des autres protocoles listés par le serveur, l’ordinateur client permet au serveur de savoir quel protocole d’authentification il prend en charge, et l’authentification se poursuit.

    -   Si l’ordinateur client ne prend pas en charge l’un des protocoles listés, l’échange d’authentification échoue.

## <a name="see-also"></a>Voir aussi
[Architecture d’authentification Windows](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


