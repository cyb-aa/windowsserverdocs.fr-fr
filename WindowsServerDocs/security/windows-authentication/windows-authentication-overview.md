---
title: Vue d’ensemble de l’authentification Windows
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485a0774-0785-457f-a964-0e9403c12bb1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2021ccf1e0015cc910f43966f9400e6bd56a230c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870970"
---
# <a name="windows-authentication-overview"></a>Vue d’ensemble de l’authentification Windows

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de navigation pour les professionnels de l’informatique répertorie des ressources de documentation pour l’authentification Windows et les technologies d’ouverture de session qui comprennent l’évaluation du produit, les guides de prise en main, les procédures, les guides de conception et de déploiement, les références techniques et les références des commandes.

## <a name="feature-description"></a>Description des fonctionnalités
L’authentification est un processus de vérification de l’identité d’un objet, d’un service ou d’une personne. Lorsque vous authentifiez un objet, l’objectif est de vérifier que celui-ci est authentique. Lorsque vous authentifiez un service ou une personne, l’objectif est de vérifier que les informations d’identification présentées sont authentiques.

Dans un contexte de réseau, l’authentification est le fait de prouver une identité à une application ou une ressource réseau. En règle générale, l’identité est prouvée par une opération de chiffrement qui utilise soit une clé que seul l’utilisateur connaît - comme avec chiffrement à clé publique - ou une clé partagée. Le côté serveur de l’échange d’authentification compare les données signées avec une clé de chiffrement connue pour valider la tentative d’authentification.

Le stockage des clés de chiffrement dans un emplacement centralisé sécurisé rend le processus d’authentification évolutif et facile à gérer. Les Services de domaine Active Directory est l’architecture recommandée et la technologie par défaut pour le stockage des informations d’identité \(, y compris les clés de chiffrement qui sont des informations d’identification de l’utilisateur\). Active Directory est requis pour les implémentations NTLM et Kerberos par défaut.

Techniques d’authentification vont de la simple ouverture de session, qui identifie les utilisateurs en fonction de quelque chose que seul l’utilisateur connaît, telles qu’un mot de passe, les mécanismes de sécurité plus puissants qui utilisent des éléments dont dispose l’utilisateur - comme les jetons, les certificats de clé publique, et biométrie. Dans un environnement d’entreprise, les services ou les utilisateurs peuvent accéder à plusieurs applications ou ressources sur plusieurs types de serveurs dans un même emplacement ou dans plusieurs emplacements. C’est pourquoi l’authentification doit prendre en charge des environnements pour d’autres plateformes et pour d’autres systèmes d’exploitation Windows.

Le système d’exploitation Windows implémente un ensemble par défaut de protocoles d’authentification, notamment Kerberos, NTLM, Transport Layer Security\/protocole SSL (Secure Sockets Layer) \(TLS\/SSL\)et l’authentification Digest, dans le cadre d’un architecture extensible. En outre, certains protocoles sont combinés en packages d’authentification, par exemple Negotiate et CredSSP (Credential Security Support Provider). Ces protocoles et packages permettent l’authentification des utilisateurs, des ordinateurs et des services ; le processus d’authentification permet à son tour aux utilisateurs et services autorisés d’accéder aux ressources de façon sécurisée.

Pour plus d’informations sur l’authentification Windows, notamment :

-   [Concepts de l’authentification Windows](windows-authentication-concepts.md)

-   [Scénarios de connexion Windows](windows-logon-scenarios.md)

-   [Architecture d’authentification Windows](windows-authentication-architecture.md)

-   [Architecture d’Interface fournisseur sécurité prise en charge](security-support-provider-interface-architecture.md)

-   [Processus d’informations d’identification de l’authentification Windows](credentials-processes-in-windows-authentication.md)

-   [Paramètres de stratégie de groupe utilisés dans l’authentification Windows](group-policy-settings-used-in-windows-authentication.md)

consultez le [vue d’ensemble technique de Windows Authentication](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Cas pratiques
L’authentification Windows est utilisée pour vérifier que les informations proviennent d’une source approuvée, qu’il s’agisse d’une personne ou d’un objet ordinateur, tel qu’un autre ordinateur. Windows offre de nombreuses méthodes différentes pour y parvenir, comme indiqué ci-dessous.

|À...|Fonctionnalité|Description|
|----|------|--------|
|S’authentifier dans un domaine Active Directory|Kerberos|Le Windows Microsoft&nbsp;systèmes d’exploitation Server implémentent le protocole d’authentification Kerberos version 5 et des extensions pour l’authentification par clé publique. Le client d’authentification Kerberos est implémenté comme un fournisseur de prise en charge de sécurité \(SSP\) et sont accessibles via l’Interface SSPI \(SSPI\). Authentification utilisateur initiale est intégrée avec l’authentification unique Winlogon\-sur l’architecture. Le centre de Distribution de clés Kerberos \(KDC\) est intégré avec d’autres services de sécurité de Windows Server en cours d’exécution sur le contrôleur de domaine. Le KDC utilise la base de données du service annuaire du domaine Active Directory en tant que sa base de données de compte de sécurité. Active Directory est requis pour les implémentations Kerberos par défaut.<br /><br />Pour obtenir des ressources supplémentaires, voir [Vue d’ensemble de l’authentification Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Authentification sécurisée sur le Web|TLS\/SSL tel qu’implémenté dans le fournisseur SSP Schannel|Le Transport Layer Security \(TLS\) les versions 1.0, 1.1 et 1.2, le protocole SSL (Secure Sockets Layer) du protocole \(SSL\) protocole, la version de protocole de versions 2.0 et 3.0, Datagram Transport Layer Security 1.0 et privé Transport des communications \(PCT\) protocole, version 1.0, sont basées sur le chiffrement à clé publique. Le canal sécurisé \(Schannel\) suite de protocoles d’authentification de fournisseur fournit ces protocoles. Tous les protocoles Schannel utilisent un modèle client et server.<br /><br />Pour des ressources supplémentaires, consultez [TLS / SSL &#40;SSP Schannel&#41; vue d’ensemble](../tls/tls-ssl-schannel-ssp-overview.md).|
|S’authentifier sur un service Web ou une application|Authentification Windows intégrée<br /><br />Authentification Digest|Pour obtenir des ressources supplémentaires, voir [Authentification Windows intégrée](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx), [Authentification Digest](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx) et [Authentification Digest avancée](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|S’authentifier sur des applications héritées|NTLM|NTLM est un défi\-protocole d’authentification de style de réponse. Outre l’authentification, le protocole NTLM offre éventuellement pour la sécurité de session, en particulier l’intégrité des messages et la confidentialité via la signature et de scellement dans NTLM, les fonctions.<br /><br />Pour obtenir des ressources supplémentaires, voir [Vue d’ensemble de NTLM](../kerberos/ntlm-overview.md).|
|Tirer parti de l’authentification multifacteur|Prise en charge des cartes à puce<br /><br />Prise en charge de la biométrie|Cartes à puce sont une falsification\-portable et résistant permettent de fournir des solutions de sécurité pour des tâches telles que l’authentification du client, connexion aux domaines, signature de code et sécurisant\-mail.<br /><br />Elle consiste à mesurer une caractéristique physique inaltérable d’une personne pour identifier celle-ci de façon unique. Les empreintes digitales font partie des caractéristiques biométriques les plus utilisées, notamment avec les millions de lecteurs d’empreintes digitales intégrés aux ordinateurs personnels et aux périphériques.<br /><br />Pour des ressources supplémentaires, consultez [référence technique de carte à puce](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Assurer la gestion locale, le stockage et la réutilisation des informations d’identification|Gestion des informations d’identification<br /><br />Autorité de sécurité locale<br /><br />Mots de passe|La gestion des informations d’identification dans Windows garantit le stockage sécurisé des informations d’identification. Informations d’identification sont collectées sur le bureau sécurisé \(pour l’accès local ou de domaine\), via des applications ou sites Web afin que les informations d’identification correctes sont présentées lors de chaque accès à une ressource.<br /><br />
|Étendre la protection de l’authentification moderne à des systèmes hérités|Protection étendue de l'authentification|Cette fonctionnalité améliore la protection et la gestion des informations d’identification lors de l’authentification des connexions réseau à l’aide de l’authentification Windows intégrée \(IWA\).|

## <a name="software-requirements"></a>Configuration logicielle requise
L’authentification Windows est conçue pour être compatible avec les précédentes versions du système d’exploitation Windows. Cependant, les améliorations apportées à chaque version ne sont pas nécessairement applicables aux versions précédentes. Pour plus d’informations, reportez-vous à la documentation relative à des fonctionnalités spécifiques.

## <a name="server-manager-information"></a>Informations sur le Gestionnaire de serveur
De nombreuses fonctionnalités d’authentification peuvent être configurées à l’aide d’une stratégie de groupe, laquelle peut être installée avec le Gestionnaire de serveur. Les fonctionnalités Windows Biometric Framework sont installées à l’aide du Gestionnaire de serveur. Autres rôles de serveur qui dépendent des méthodes d’authentification, tels que serveur Web \(IIS\) et Services de domaine Active Directory, peut également être installé à l’aide du Gestionnaire de serveur.

## <a name="related-resources"></a>Ressources connexes

|Technologies d’authentification|Ressources|
|----------------|-------|
|Authentification Windows|[Présentation technique de l’authentification Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Inclut des rubriques traitant des différences entre les versions, les concepts généraux de l’authentification, les scénarios d’ouverture de session, d’architecture pour les versions prises en charge et les paramètres applicables.|
|Kerberos|[Vue d’ensemble de l’authentification Kerberos](../kerberos/kerberos-authentication-overview.md)<br /><br />[Présentation de la délégation contrainte du protocole Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Référence technique de l’authentification Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<br /><br />[Guide de survie Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet Wiki\)|
|TLS\/SSL et DTLS \(fournisseur SSP Schannel\)|[TLS / SSL &#40;SSP Schannel&#41; vue d’ensemble](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Référence technique de fournisseur de prise en charge de sécurité de Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Authentification Digest|[Digest de référence technique d’authentification](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[Vue d’ensemble NTLM](../kerberos/ntlm-overview.md)<br />Contient des liens vers des ressources actuelles et passées|
|PKU2U|[Présentation de PKU2U dans Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Carte à puce|[Référence technique de carte à puce](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|Informations d’identification|[Gestion et protection des informations d'identification](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contient des liens vers des ressources actuelles et passées<br /><br />[Vue d’ensemble de mots de passe](../kerberos/passwords-overview.md)<br />Contient des liens vers des ressources actuelles et passées|


