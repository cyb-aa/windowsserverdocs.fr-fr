---
title: Vue d’ensemble de l’authentification Windows
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 485a0774-0785-457f-a964-0e9403c12bb1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2139a6b3a2b22bfe629df7ed073e16eabe31cac2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861712"
---
# <a name="windows-authentication-overview"></a>Vue d’ensemble de l’authentification Windows

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de navigation pour les professionnels de l’informatique répertorie des ressources de documentation pour l’authentification Windows et les technologies d’ouverture de session qui comprennent l’évaluation du produit, les guides de prise en main, les procédures, les guides de conception et de déploiement, les références techniques et les références des commandes.

## <a name="feature-description"></a>Description des fonctionnalités
L’authentification est un processus de vérification de l’identité d’un objet, d’un service ou d’une personne. Lorsque vous authentifiez un objet, l’objectif est de vérifier que celui-ci est authentique. Lorsque vous authentifiez un service ou une personne, l’objectif est de vérifier que les informations d’identification présentées sont authentiques.

Dans un contexte de réseau, l’authentification est le fait de prouver une identité à une application ou une ressource réseau. En général, l’identité est prouvée par une opération de chiffrement qui utilise soit une clé uniquement que l’utilisateur connaît, comme avec le chiffrement à clé publique, soit une clé partagée. Le côté serveur de l’échange d’authentification compare les données signées avec une clé de chiffrement connue pour valider la tentative d’authentification.

Le stockage des clés de chiffrement dans un emplacement centralisé sécurisé rend le processus d’authentification évolutif et facile à gérer. Active Directory Domain Services est la technologie par défaut recommandée pour le stockage des informations d’identité \(notamment les clés de chiffrement qui sont les informations d’identification de l’utilisateur\). Active Directory est requis pour les implémentations NTLM et Kerberos par défaut.

Les techniques d’authentification vont d’une ouverture de session simple, qui identifie les utilisateurs en fonction de ce que seul l’utilisateur connaît comme un mot de passe, à des mécanismes de sécurité plus puissants qui utilisent des jetons similaires à ceux de l’utilisateur, des certificats de clé publique et de la biométrie. Dans un environnement d’entreprise, les services ou les utilisateurs peuvent accéder à plusieurs applications ou ressources sur plusieurs types de serveurs dans un même emplacement ou dans plusieurs emplacements. C’est pourquoi l’authentification doit prendre en charge des environnements pour d’autres plateformes et pour d’autres systèmes d’exploitation Windows.

Le système d’exploitation Windows implémente un ensemble par défaut de protocoles d’authentification, notamment Kerberos, NTLM, Transport Layer Security\/protocole SSL \(TLS\/SSL\)et Digest, dans le cadre d’une architecture extensible. En outre, certains protocoles sont combinés en packages d’authentification, par exemple Negotiate et CredSSP (Credential Security Support Provider). Ces protocoles et packages permettent l’authentification des utilisateurs, des ordinateurs et des services ; le processus d’authentification permet à son tour aux utilisateurs et services autorisés d’accéder aux ressources de façon sécurisée.

Pour plus d’informations sur l’authentification Windows, notamment :

-   [Concepts de l’authentification Windows](windows-authentication-concepts.md)

-   [Scénarios d’ouverture de session Windows](windows-logon-scenarios.md)

-   [Architecture d’authentification Windows](windows-authentication-architecture.md)

-   [Architecture de l’interface du fournisseur SSP (Security Support Provider)](security-support-provider-interface-architecture.md)

-   [Processus des informations d’identification dans l’authentification Windows](credentials-processes-in-windows-authentication.md)

-   [Paramètres de stratégie de groupe utilisés dans l’authentification Windows](group-policy-settings-used-in-windows-authentication.md)

consultez la [vue d’ensemble technique de l’authentification Windows](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Cas pratiques
L’authentification Windows est utilisée pour vérifier que les informations proviennent d’une source approuvée, qu’il s’agisse d’une personne ou d’un objet ordinateur, tel qu’un autre ordinateur. Windows offre de nombreuses méthodes différentes pour y parvenir, comme indiqué ci-dessous.

|À...|Composant|Description|
|----|------|--------|
|S’authentifier dans un domaine Active Directory|Kerberos|Les systèmes d’exploitation Microsoft Windows&nbsp;Server implémentent le protocole d’authentification Kerberos version 5 et les extensions pour l’authentification de la clé publique. Le client d’authentification Kerberos est implémenté en tant que fournisseur de support de sécurité \(SSP\) et est accessible via l’interface du fournisseur de support de sécurité \(\)SSPI. L’authentification utilisateur initiale est intégrée à l’authentification unique Winlogon\-sur l’architecture. Le\) Kerberos centre de distribution de clés \(KDC est intégré à d’autres services de sécurité Windows Server s’exécutant sur le contrôleur de domaine. Le KDC utilise la base de données du service d’annuaire Active Directory du domaine comme base de données du compte de sécurité. Active Directory est requis pour les implémentations Kerberos par défaut.<p>Pour obtenir des ressources supplémentaires, voir [Vue d’ensemble de l’authentification Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Authentification sécurisée sur le Web|TLS\/SSL tel qu’implémenté dans le fournisseur de support de sécurité Schannel|Transport Layer Security \(TLS\) Protocol versions 1,0, 1,1 et 1,2, protocole SSL \(protocole SSL\), versions 2,0 et 3,0, Datagram Transport Layer Security Protocol version 1,0 et le protocole de transport privé \(PCT\) Protocol, version 1,0, sont basés sur le chiffrement à clé publique. Le canal sécurisé \(la suite de protocoles d’authentification du fournisseur Schannel\) fournit ces protocoles. Tous les protocoles Schannel utilisent un modèle client et server.<p>Pour obtenir des ressources supplémentaires, consultez [vue &#40;d’ensemble&#41; du SSP TLS-SSL Schannel](../tls/tls-ssl-schannel-ssp-overview.md).|
|S’authentifier sur un service Web ou une application|Authentification intégrée Windows<p>Authentification Digest|Pour obtenir des ressources supplémentaires, voir [Authentification Windows intégrée](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx), [Authentification Digest](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx) et [Authentification Digest avancée](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|S’authentifier sur des applications héritées|NTLM|NTLM est un protocole d’authentification de type stimulation\-réponse. En plus de l’authentification, le protocole NTLM fournit éventuellement la sécurité de session, en particulier l’intégrité et la confidentialité des messages via des fonctions de signature et de scellement dans NTLM.<p>Pour obtenir des ressources supplémentaires, voir [Vue d’ensemble de NTLM](../kerberos/ntlm-overview.md).|
|Tirer parti de l’authentification multifacteur|Prise en charge des cartes à puce<p>Prise en charge de la biométrie|Les cartes à puce sont une solution de falsification\-et portable pour fournir des solutions de sécurité pour des tâches telles que l’authentification du client, la connexion aux domaines, la signature du code et la sécurisation e\-mail.<p>Elle consiste à mesurer une caractéristique physique inaltérable d’une personne pour identifier celle-ci de façon unique. Les empreintes digitales font partie des caractéristiques biométriques les plus utilisées, notamment avec les millions de lecteurs d’empreintes digitales intégrés aux ordinateurs personnels et aux périphériques.<p>Pour obtenir des ressources supplémentaires, consultez informations techniques de référence sur les [cartes à puce](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Assurer la gestion locale, le stockage et la réutilisation des informations d’identification|Gestion des informations d’identification<p>Autorité de sécurité locale<p>Mots de passe|La gestion des informations d’identification dans Windows garantit le stockage sécurisé des informations d’identification. Les informations d’identification sont collectées sur le Bureau sécurisé \(pour l’accès local ou au domaine\), par le biais d’applications ou de sites Web afin que les informations d’identification correctes soient présentées à chaque accès à une ressource.<p>
|Étendre la protection de l’authentification moderne à des systèmes hérités|Protection étendue pour l’authentification|Cette fonctionnalité améliore la protection et la gestion des informations d’identification lors de l’authentification des connexions réseau à l’aide de l’authentification Windows intégrée \(IWA\).|

## <a name="software-requirements"></a>Configuration logicielle requise
L’authentification Windows est conçue pour être compatible avec les précédentes versions du système d’exploitation Windows. Cependant, les améliorations apportées à chaque version ne sont pas nécessairement applicables aux versions précédentes. Pour plus d’informations, reportez-vous à la documentation relative à des fonctionnalités spécifiques.

## <a name="server-manager-information"></a>Informations sur le Gestionnaire de serveur
De nombreuses fonctionnalités d’authentification peuvent être configurées à l’aide d’une stratégie de groupe, laquelle peut être installée avec le Gestionnaire de serveur. Les fonctionnalités Windows Biometric Framework sont installées à l’aide du Gestionnaire de serveur. D’autres rôles de serveur qui dépendent des méthodes d’authentification, tels que le serveur Web \(IIS\) et Active Directory Domain Services, peuvent également être installés à l’aide de Gestionnaire de serveur.

## <a name="related-resources"></a>Ressources connexes

|Technologies d’authentification|Ressources|
|----------------|-------|
|Authentification Windows|[Vue d’ensemble technique de l’authentification Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Contient des rubriques traitant des différences entre les versions, les concepts généraux d’authentification, les scénarios de connexion, les architectures pour les versions prises en charge et les paramètres applicables.|
|Kerberos|[Présentation de l’authentification Kerberos](../kerberos/kerberos-authentication-overview.md)<p>[Vue d’ensemble de la délégation Kerberos contrainte](../kerberos/kerberos-constrained-delegation-overview.md)<p>Informations [techniques de référence sur l’authentification Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<p>[Guide de survie Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(\) wiki TechNet|
|Fournisseur de support de sécurité TLS\/SSL et DTLS \(Schannel\)|[Vue d’ensemble &#40;du SSP&#41; TLS-SSL Schannel](../tls/tls-ssl-schannel-ssp-overview.md)<p>[Informations techniques de référence sur le fournisseur SSP (Security Support Provider) Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Authentification Digest|Informations [techniques de référence sur l’authentification Digest](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[Vue d’ensemble de NTLM](../kerberos/ntlm-overview.md)<br />Contient des liens vers les ressources actuelles et précédentes|
|PKU2U|[Présentation de PKU2U dans Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Carte à puce|[Informations techniques de référence sur les cartes à puce](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<p>
|Informations d'identification|[Gestion et protection des informations d'identification](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contient des liens vers les ressources actuelles et précédentes<p>[Vue d’ensemble des mots de passe](../kerberos/passwords-overview.md)<br />Contient des liens vers les ressources actuelles et précédentes|


