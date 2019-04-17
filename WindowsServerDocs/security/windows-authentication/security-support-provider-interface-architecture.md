---
title: "Architecture d’Interface sécurité prise en charge du fournisseur"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="security-support-provider-interface-architecture"></a>Architecture d’Interface sécurité prise en charge du fournisseur

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique de référence destinée aux professionnels de l’informatique décrit les protocoles d’authentification Windows qui sont utilisées dans l’architecture de l’Interface SSPI (Security Support Provider Interface).

Le MicrosoftSSPI Security Support Provider Interface () est la base de l’authentification Windows. Applications et services d’infrastructure qui requièrent une authentification utilisent l’interface SSPI pour la fournir.

L’interface SSPI est l’implémentation de la sécurité Service API GSSAPI (Generic) dans les systèmes d’exploitation Windows Server. Pour plus d’informations sur GSSAPI, voir RFC2743 et 2744 RFC dans la base de données de IETF RFC.

Les fournisseurs de prise en charge de sécurité (SSP) par défaut qui font appel à des protocoles d’authentification spécifique dans Windows sont intégrées dans l’interface SSPI en tant que DLL. Ces fournisseurs de services partagés par défaut sont décrits dans les sections suivantes. Fournisseurs supplémentaires peuvent être incorporées si elles peuvent fonctionner avec l’interface SSPI.

Comme indiqué dans l’image suivante, l’interface SSPI Windows fournit un mécanisme qui exécute des jetons d’authentification sur le canal de communication existant entre l’ordinateur client et le serveur. Lorsque deux ordinateurs ou périphériques doivent être authentifiés afin qu’ils puissent communiquer en toute sécurité, les demandes d’authentification sont routées vers l’interface SSPI, qui achève le processus d’authentification, quel que soit le protocole réseau en cours d’utilisation. L’interface SSPI retourne des objets volumineux binaires transparents. Ceux-ci sont transmis entre les applications, à partir de laquelle ils peuvent être transmises à la couche SSPI. Par conséquent, l’interface SSPI permet à une application d’utiliser différents modèles de sécurité sur un ordinateur ou un réseau sans modification de l’interface pour le système de sécurité.

![Diagramme montrant l’Architecture de Interface du fournisseur de prise en charge de sécurité](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

Les sections suivantes décrivent les fournisseurs SSP par défaut interagisse avec l’interface SSPI. Les fournisseurs de services partagés sont utilisés de différentes façons dans les systèmes d’exploitation Windows à la promotion de communication sécurisé dans un environnement réseau non sécurisé.

-   [Fournisseur SSP Kerberos](#BKMK_KerbSSP)

-   [Fournisseur SSP NTLM](#BKMK_NTLMSSP)

-   [Digest Security Support Provider](#BKMK_DigestSSP)

-   [Fournisseur SSP Schannel](#BKMK_SchannelSSP)

-   [Négocier Security Support Provider](#BKMK_NegoSSP)

-   [Credential Security Support Provider](#BKMK_CredSSP)

-   [Négocier Extensions Security Support Provider](#BKMK_NegoExtsSSP)

-   [PKU2U Security Support Provider](#BKMK_PKU2USSP)

Également inclus dans cette rubrique:

[Sélection de fournisseur de Support de sécurité](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Fournisseur SSP Kerberos
Fournisseur de services partagés utilise uniquement le protocole Kerberos version5 comme étant implémentées par Microsoft. Ce protocole est basé sur du réseau groupe de travail RFC4120 et révisions de brouillon. Il est un protocole standard qui est utilisé avec un mot de passe ou une carte à puce pour une ouverture de session interactive. Il est également la méthode d’authentification préféré pour les services dans Windows.

Étant donné que le protocole Kerberos est le protocole d’authentification par défaut depuis Windows2000, tous les services de domaine prend en charge la SSP. Kerberos Ces services incluent:

-   Requêtes ActiveDirectory qui utilisent le répertoire protocole LDAP (Lightweight Access)

-   Gestion de serveur ou station de travail à distance qui utilise le service d’appel de procédure distante

-   Services d’impression

-   Authentification du client-serveur

-   Accès au fichier distant qui utilise le protocole Server Message Block (SMB) (également appelé Common Internet File System ou CIFS)

-   Gestion du système de fichiers distribués et référence

-   Authentification de l’intranet à Internet Information Services (IIS)

-   Authentification d’autorité de sécurité pour Internet Protocol security (IPsec)

-   Demandes de certificats ActiveDirectory Certificate Services pour les ordinateurs et les utilisateurs du domaine

Emplacement: %windir%\Windows\System32\kerberos.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique aux** figurant au début de cette rubrique, ainsi que de Windows Server2003 et de WindowsXP.

**Ressources supplémentaires pour le protocole Kerberos et le SSP Kerberos**

-   [MicrosoftKerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: Extensions du protocole Kerberos](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: Extensions du protocole Kerberos: Service pour l’utilisateur et la spécification du protocole de la délégation contrainte](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Fournisseur SSP/AP Kerberos (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Améliorations liées à Kerberos](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx) pour WindowsVista

-   [Modifications apportées à l’authentification Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) pour Windows7 

-   [Référence technique de l’authentification Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>Fournisseur SSP NTLM
NTLM Security Support Provider (SSP NTLM) est un fichier binaire protocole utilisé par le fournisseur d’Interface SSPI (Security Support) pour autoriser l’authentification NTLM stimulation-réponse et négocier l’intégrité et la confidentialité des options de messagerie. NTLM est utilisé partout où l’authentification SSPI est utilisée, y compris pour l’authentification Server Message Block ou CIFS, l’authentification HTTP négocier (par exemple, l’authentification Web Internet) et le service d’appel de procédure distante. Le SSP NTLM inclut le NTLM et NTLM version2 (NTLMv2) protocoles d’authentification.

Les systèmes d’exploitation Windows pris en charge peuvent utiliser le SSP NTLM pour les éléments suivants:

-   Authentification du client/serveur

-   Services d’impression

-   Accès aux fichiers à l’aide du protocole CIFS (SMB)

-   Service d’appel de procédure distante sécurisée ou DCOM

Emplacement: %windir%\Windows\System32\msv1_0.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique aux** figurant au début de cette rubrique, ainsi que de Windows Server2003 et de WindowsXP.

**Ressources supplémentaires pour le protocole NTLM et NTLM SSP**

-   [Package d’authentification Msv1_0 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [Modifications apportées à l’authentification NTLM](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) dans Windows7 

-   [MicrosoftNTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [L’audit et la restriction de guide de l’utilisation NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>Digest Security Support Provider
L’authentification Digest est un standard qui est utilisé pour le répertoire protocole LDAP (Lightweight Access) et l’authentification web. L’authentification Digest transmet les informations d’identification sur le réseau en tant qu’un résumé de message ou de hachage MD5.

(Wdigest.dll) SSP Digest est utilisé pour les éléments suivants:

-   Accès Internet Explorer et Internet Information Services (IIS)

-   Requêtes LDAP

Emplacement: %windir%\Windows\System32\Digest.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique aux** figurant au début de cette rubrique, ainsi que de Windows Server2003 et de WindowsXP.

**Ressources supplémentaires pour le protocole Digest et le fournisseur SSP Digest**

-   [Authentification Digest de Microsoft (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: Extensions du protocole de résumé](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Fournisseur SSP Schannel
La Secure Channel (Schannel) est utilisé pour l’authentification de serveur web, par exemple, lorsqu’un utilisateur tente d’accéder à un serveur web sécurisé.

Le protocole TLS, protocole SSL, le protocole de la technologie PCT (Private Communications) et le protocole de couche DTLS (Datagram Transport) sont basées sur le chiffrement à clé publique. Schannel fournit tous ces protocoles. Tous les protocoles Schannel utilisent un modèle client/serveur. Le SSP Schannel utilise des certificats de clé publique pour authentifier les tiers. Lorsque l’authentification des tiers, le SSP Schannel sélectionne un protocole dans l’ordre de préférence suivant:

-   Transport Layer Security (TLS) version1.0

-   Transport Layer Security (TLS) version1.1

-   Transport Layer Security (TLS) version1.2

-   Secure Socket Layer (SSL) version2.0

-   Secure Socket Layer (SSL) version3.0

-   Private Communications Technology (PCT)

    **Remarque** PCT est désactivé par défaut.

Le protocole sélectionné est le protocole d’authentification préféré que le client et le serveur peuvent prendre en charge. Par exemple, si un serveur prend en charge tous les protocoles Schannel et que le client prend en charge uniquement SSL 3.0 et SSL 2.0, le processus d’authentification utilise SSL 3.0.

DTLS est utilisé lorsqu’elle est appelée explicitement par l’application. Pour plus d’informations sur les autres protocoles utilisés par le fournisseur Schannel et DTLS, voir [référence technique du fournisseur de prise en charge de sécurité Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Emplacement: %windir%\Windows\System32\Schannel.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique aux** figurant au début de cette rubrique, ainsi que de Windows Server2003 et de WindowsXP.

> [!NOTE]
> TLS 1.2 a été introduite dans ce fournisseur dans Windows Server2008R2 et Windows7. DTLS a été introduite dans ce fournisseur dans Windows Server2012 et Windows8.

**Ressources supplémentaires pour les protocoles TLS et SSL et le SSP Schannel**

-   [Canal sécurisé (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [Référence technique de TLS/SSL](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: transport Layer Security (TLS) profil](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>Négocier Security Support Provider
Le Protected GSS-API négociation mécanisme SPNEGO (Simple and) constitue la base pour le fournisseur SSP négocier, whichcan être utilisés pour négocier un protocole d’authentification spécifique. Lorsqu’une application appelle dans l’interface SSPI pour vous connecter à un réseau, il peut spécifier un fournisseur de services partagés pour traiter la demande. Si l’application spécifie le fournisseur SSP négocier, il analyse la demande et sélectionne le fournisseur approprié pour gérer la demande, en fonction des stratégies de sécurité configurée par le client.

SPNEGO est spécifié dans la RFC2478.

Dans les versions prises en charge des systèmes d’exploitation Windows, la négociation de sécurité prend en charge sélectionne fournisseur entre le protocole Kerberos et NTLM. Sélectionne de négociation du protocole Kerberos par défaut, sauf si ce protocole ne peut pas être utilisé par un des systèmes impliqués dans l’authentification ou l’application appelante n’a pas fourni les informations suffisantes pour utiliser le protocole Kerberos.

Emplacement: %windir%\Windows\System32\lsasrv.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique aux** figurant au début de cette rubrique, ainsi que de Windows Server2003 et de WindowsXP.

**Ressources supplémentaires pour le fournisseur SSP négocier**

-   [Microsoft négocier (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: Extensions de simple et Protected GSS-API négociation mécanisme (SPNEGO)](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: négocier et la spécification du protocole d’authentification de Nego2 HTTP](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>Credential Security Support Provider
Le fournisseur de services de sécurité d’informations d’identification (CredSSP) fournit une unique (SSO) utilisateur expérience d’authentification lors du démarrage de nouvelles sessions des Services Terminal Server et des Services Bureau à distance. CredSSP permet aux applications de déléguer les informations d’identification des utilisateurs à partir de l’ordinateur client (en utilisant le fournisseur SSP côté client) sur le serveur cible (via le SSP côté serveur), selon les stratégies du client. Stratégies CredSSP sont configurés à l’aide de stratégie de groupe, et la délégation des informations d’identification est désactivée par défaut.

Emplacement: %windir%\Windows\System32\credssp.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique aux** figurant au début de cette rubrique.

**Ressources supplémentaires pour le fournisseur SSP informations d’identification**

-   [\[MS-CSSP\]: spécification du protocole de sécurité prise en charge du fournisseur (CredSSP) des informations d’identification](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Fournisseur de services de sécurité et authentification unique des informations d’identification pour Terminal Services d’ouverture de session](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>Négocier Extensions Security Support Provider
Extensions de négocier (NegoExts) est un package d’authentification qui négocie l’utilisation de fournisseurs de services partagés, autre que NTLM ou le protocole Kerberos, des applications et des scénarios implémentée par Microsoft et d’autres logiciels sociétés.

Cette extension pour le package Negotiate permet les scénarios suivants:

-   **Disponibilité du client enrichi au sein d’un système fédéré.** Les documents sont accessibles sur les sites SharePoint, et elles peuvent être modifiées à l’aide d’une application complète de MicrosoftOffice.

-   **Client riche en charge pour les services de MicrosoftOffice.** Les utilisateurs peuvent se connecter aux services de MicrosoftOffice et utiliser une application complète de MicrosoftOffice.

-   **Serveur hébergé MicrosoftExchange et Outlook.** Il n’existe aucune approbation de domaine établie car le serveur Exchange est hébergé sur le web. Outlook utilise le service Windows Live pour authentifier les utilisateurs.

-   **Disponibilité du client enrichi entre les ordinateurs clients et serveurs.** Les composants de mise en réseau et l’authentification du système d’exploitation sont utilisés.

Le package Windows Negotiate traite le SSP NegoExts de la même manière comme il le fait pour Kerberos et NTLM. NegoExts.dll est chargé dans l’autorité système locale (LSA) au démarrage. Lors de la réception d’une demande d’authentification, selon la source de la demande, NegoExts négocie entre les fournisseurs de services partagés pris en charge. Il collecte les informations d’identification et les stratégies, les chiffre et envoie ces informations pour le fournisseur SSP approprié, dans lequel le jeton de sécurité est créé.

Les fournisseurs de services partagés pris en charge par NegoExts ne sont pas autonome SSP tels que Kerberos et NTLM. Par conséquent, le SSP NegoExts, en cas d’échec de la méthode d’authentification pour une raison quelconque, un message d’échec d’authentification est affiché ou ouvert une session. Aucune méthode d’authentification renégociation ou de secours n’est possibles.

Emplacement: %windir%\Windows\System32\negoexts.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique aux** figurant au début de cette rubrique, à l’exclusion de Windows Server2008 et WindowsVista.

### <a name="BKMK_PKU2USSP"></a>PKU2U Security Support Provider
Le protocole PKU2U a été introduit et implémenté en tant qu’un fournisseur de services partagés dans Windows7 et Windows Server2008R2. Fournisseur de services partagés permet une authentification pair à pair, notamment par le média et la fonctionnalité appelée groupe résidentiel, ce qui a été introduit dans Windows7 de partage de fichiers. La fonctionnalité permet le partage entre des ordinateurs qui ne sont pas membres d’un domaine.

Emplacement: %windir%\Windows\System32\pku2u.dll

Ce fournisseur est inclus par défaut dans les versions désignées dans la **s’applique aux** figurant au début de cette rubrique, à l’exclusion de Windows Server2008 et WindowsVista.

**Ressources supplémentaires pour le protocole PKU2U et le fournisseur SSP PKU2U**

-   [Présentation de l’intégration d’identité en ligne](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>Sélection de fournisseur de Support de sécurité
L’interface SSPI Windows peuvent utiliser les protocoles pris en charge par les fournisseurs de prise en charge de sécurité installés. Toutefois, pas tous les systèmes d’exploitation prenant en charge les mêmes packages SSP comme n’importe quel ordinateur donné exécutant Windows Server, les clients et serveurs doivent négocier d’utiliser un protocole qui les prennent en charge. Windows Server préfère les ordinateurs clients et les applications d’utiliser le protocole Kerberos, un protocole basé sur des normes fort, lorsque possible, mais le système d’exploitation continue de permettent aux applications qui ne prennent pas en charge le protocole Kerberos pour authentifier les clients et les ordinateurs clients.

Avant l’authentification peut communiquer sur place les deux ordinateurs doivent accepter un protocole que les deux peuvent prendre en charge. Pour n’importe quel protocole soit utilisable par le biais de l’interface SSPI, chaque ordinateur doit disposer du fournisseur approprié. Par exemple, pour un ordinateur client et serveur à utiliser le protocole d’authentification Kerberos, ils doivent tous deux prendre en charge Kerberos v5. Windows Server utilise la fonction **EnumerateSecurityPackages** pour identifier les fournisseurs de services partagés sont pris en charge sur un ordinateur et ce que sont les fonctionnalités de ces fournisseurs de services partagés.

La sélection d’un protocole d’authentification peut être gérée dans un des deux manières suivantes:

1.  [Protocole d’authentification unique](#BKMK_SingleAuth)

2.  [Négocier l’option](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>Protocole d’authentification unique
Quand un seul protocole acceptable est spécifié sur le serveur, l’ordinateur client doit prendre en charge le protocole spécifié ou l’échec de communication. Quand un seul protocole acceptable est spécifié, l’échange d’authentification s’effectue comme suit:

1.  L’ordinateur client demande l’accès à un service.

2.  Le serveur répond à la demande et spécifie le protocole qui sera utilisé.

3.  L’ordinateur client examine le contenu de la réponse et les vérifications pour déterminer si elle prend en charge le protocole spécifié. Si l’ordinateur client ne prend en charge le protocole spécifié, l’authentification se poursuit. Si l’ordinateur client ne prend pas en charge le protocole, l’authentification échoue, quel que soit l’indique si l’ordinateur client est autorisé à accéder à la ressource.

### <a name="BKMK_Negotiate"></a>Négocier l’option
L’option négocier peut être utilisée pour autoriser le client et le serveur tenter de trouver un protocole acceptable. Cela est basée sur la Simple and Protected GSS-API mécanisme SPNEGO (Negotiation). Lorsque l’authentification commence avec l’option de négocier un protocole d’authentification, l’échange SPNEGO s’effectue comme suit:

1.  L’ordinateur client demande l’accès à un service.

2.  Le serveur répond avec une liste de protocoles d’authentification qui prend en charge et une stimulation d’authentification ou la réponse, basé sur le protocole est son premier choix. Par exemple, le serveur peut répertorier le protocole Kerberos et NTLM et envoyer une réponse de l’authentification Kerberos.

3.  L’ordinateur client examine le contenu de la réponse et les vérifications pour déterminer si elle prend en charge les protocoles spécifiés.

    -   Si l’ordinateur client prend en charge le protocole par défaut, l’authentification se poursuit.

    -   Si l’ordinateur client ne prend pas en charge le protocole par défaut, mais il ne prend en charge une des autres protocoles répertoriés par le serveur, l’ordinateur client informe le serveur auquel il prend en charge le protocole d’authentification et l’authentification se poursuit.

    -   Si l’ordinateur client ne prend pas en charge des protocoles de la liste, l’échange d’authentification échoue.

## <a name="see-also"></a>Voir aussi
[Architecture d’authentification Windows](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


