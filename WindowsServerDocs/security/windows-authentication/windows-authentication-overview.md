---
title: "Vue d’ensemble de l’authentification Windows"
description: "Sécurité de Windows Server"
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
ms.openlocfilehash: c2ec55ed6b09628d1d80a24be766259980e84d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-overview"></a>Vue d’ensemble de l’authentification Windows

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique de navigation pour les professionnels de l’informatique répertorie des ressources de documentation pour les technologies d’ouverture de session et l’authentification Windows qui comprennent l’évaluation du produit, les guides de prise en main, des procédures, les guides de conception et de déploiement, des références techniques et références des commandes.

## <a name="feature-description"></a>Description de la fonctionnalité
L’authentification est un processus de vérification de l’identité d’un objet, le service ou la personne. Lorsque vous authentifiez un objet, l’objectif est de vérifier que l’objet est authentique. Lorsque vous authentifiez un service ou une personne, l’objectif est de vérifier que les informations d’identification présentées sont authentiques.

Dans un contexte de mise en réseau, l’authentification est le fait de prouver l’identité pour une application de réseau ou une ressource. En règle générale, identité est prouvée par une opération de chiffrement qui utilise soit une clé que seul l’utilisateur connaît - comme avec le chiffrement à clé publique - ou une clé partagée. Le côté serveur de l’échange d’authentification compare les données signées avec une clé de chiffrement connue pour valider la tentative d’authentification.

Stockage de clés de chiffrement dans un emplacement centralisé sécurisé rend le processus d’authentification évolutif et facile à gérer. Les Services de domaine Active Directory est l’architecture recommandée et la technologie par défaut pour le stockage des informations d’identité \ (y compris les clés de chiffrement qui sont credentials\ de l’utilisateur). Active Directory est requis pour les implémentations Kerberos et les par défaut NTLM.

Techniques d’authentification vont à partir d’une simple ouverture de session, qui identifie les utilisateurs en fonction de quelque chose qui ne sait que l’utilisateur - comme un mot de passe, à des mécanismes de sécurité plus puissants qui utilisent des éléments dont dispose l’utilisateur - comme jetons, les certificats de clé publique et la biométrie. Dans un environnement d’entreprise, services ou les utilisateurs peuvent accéder à plusieurs applications ou des ressources sur plusieurs types de serveurs dans un même emplacement ou dans plusieurs emplacements. Pour ces raisons, l’authentification doit prendre en charge les environnements pour d’autres plateformes et autres systèmes d’exploitation Windows.

Le système d’exploitation Windows implémente un ensemble de protocoles d’authentification, notamment Kerberos, NTLM, par défaut \(TLS\/SSL\) Transport Layer sécurité\/Secure Sockets Layer et l’authentification Digest, dans le cadre d’une architecture extensible. En outre, certains protocoles sont combinés en packages d’authentification par exemple Negotiate et le fournisseur SSP d’informations d’identification. Ces protocoles et packages permettent l’authentification des utilisateurs, des ordinateurs et des services; le processus d’authentification permet à son tour, les services et les utilisateurs autorisés d’accéder aux ressources de manière sécurisée.

Pour plus d’informations sur l’authentification Windows, y compris

-   [Concepts de l’authentification Windows](windows-authentication-concepts.md)

-   [Scénarios d’ouverture de session de Windows](windows-logon-scenarios.md)

-   [Architecture d’authentification Windows](windows-authentication-architecture.md)

-   [Architecture d’Interface sécurité prise en charge du fournisseur](security-support-provider-interface-architecture.md)

-   [Processus des informations d’identification dans l’authentification Windows](credentials-processes-in-windows-authentication.md)

-   [Paramètres de stratégie de groupe utilisés dans l’authentification Windows](group-policy-settings-used-in-windows-authentication.md)

consultez le [présentation technique de l’authentification Windows](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Applications pratiques
Authentification Windows est utilisée pour vérifier que les informations proviennent d’une source approuvée, à partir d’un objet personne ou ordinateur, tel qu’un autre ordinateur. Windows fournit de nombreuses méthodes différentes pour atteindre cet objectif, comme décrit ci-dessous.

|À...|Fonctionnalité|Description|
|----|------|--------|
|S’authentifier dans un domaine Active Directory|Kerberos|Microsoft Windows&nbsp;systèmes d’exploitation Server implémentent le protocole d’authentification Kerberos version 5 et des extensions pour l’authentification de clé publique. Le client d’authentification Kerberos est implémenté en tant qu’un fournisseur de support de sécurité \(SSP\) et sont accessibles via l’Interface SSPI \(SSPI\). L’authentification utilisateur initiale est intégrée à l’architecture unique Winlogon sur connexion. Le \(KDC\) centre de Distribution de clés Kerberos est intégré à d’autres services de sécurité de Windows Server en cours d’exécution sur le contrôleur de domaine. Le KDC utilise la base de données du service annuaire du domaine Active Directory en tant que sa base de données du compte de sécurité. Active Directory est requis pour les implémentations Kerberos par défaut.<br /><br />Pour des ressources supplémentaires, voir [vue d’ensemble de l’authentification Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Authentification sécurisée sur le web|TLS\/SSL tel qu’implémenté dans le fournisseur SSP Schannel|Le Transport Layer Security \(TLS\) protocole versions 1.0, 1.1 et 1.2, protocole Secure Sockets Layer \(SSL\) version du protocole versions 2.0 et 3.0, Datagram Transport Layer Security 1.0 et le protocole Private Communications Transport \(PCT\), version 1.0 sont basés sur le chiffrement à clé publique. La suite de protocoles d’authentification de canal sécurisé \(Schannel\) fournisseur fournit ces protocoles. Tous les protocoles Schannel utilisent un modèle client et serveur.<br /><br />Pour des ressources supplémentaires, voir [TLS / SSL & #40; Fournisseur SSP Schannel & #41; Vue d’ensemble](../tls/tls-ssl-schannel-ssp-overview.md).|
|S’authentifier auprès d’un service web ou une application|Authentification Windows intégrée<br /><br />Authentification Digest|Pour des ressources supplémentaires, voir [l’authentification Windows intégrée] (https://technet.microsoft.com/library/cc758557(v=WS.10.aspx and [Digest Authentication](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx), and [Advanced Digest Authentication](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|S’authentifier auprès des applications héritées|NTLM|NTLM est un protocole d’authentification de style challenge\-réponse. En plus de l’authentification, le protocole NTLM offre éventuellement la sécurité de session--spécifiquement l’intégrité des messages et la confidentialité par le biais de signature et de scellement fonctions dans NTLM.<br /><br />Pour des ressources supplémentaires, voir [vue d’ensemble de NTLM](../kerberos/ntlm-overview.md).|
|Authentification multifacteur exploiter|Prise en charge de la carte à puce<br /><br />Prise en charge biométrique|Les cartes à puce sont un moyen tamper\ aux falsifications et portable pour fournir des solutions de sécurité pour des tâches telles que l’authentification du client, connexion aux domaines, signature de code et sécurisation e\ de messagerie.<br /><br />La biométrie repose sur mesurer une caractéristique physique inaltérable d’une personne pour identifier cette personne. Empreintes digitales sont une des caractéristiques biométriques plus fréquemment utilisées, des millions de périphériques biométriques d’empreintes digitales qui sont incorporées dans les ordinateurs personnels et les périphériques.<br /><br />Pour des ressources supplémentaires, voir [référence technique de carte à puce](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Fournir une gestion locale, stockage et réutilisation des informations d’identification|Gestion des informations d’identification<br /><br />Autorité de sécurité locale<br /><br />Mots de passe|Gestion des informations d’identification dans Windows garantit que les informations d’identification sont stockées en toute sécurité. Informations d’identification sont collectées sur le bureau sécurisé \ (pour access\ local ou de domaine), par le biais des applications ou sites Web afin que les informations d’identification correctes sont présentées à chaque fois qu’une ressource est accessible.<br /><br />
|Étendre la protection de l’authentification moderne à des systèmes hérités|Protection étendue pour l’authentification|Cette fonctionnalité améliore la protection et la gestion des informations d’identification lors de l’authentification des connexions réseau à l’aide de l’authentification Windows intégrée \(IWA\).|

## <a name="software-requirements"></a>Configuration logicielle requise
L’authentification Windows est conçue pour être compatible avec les versions précédentes du système d’exploitation Windows. Toutefois, les améliorations apportées à chaque version ne sont pas nécessairement applicables aux versions précédentes. Reportez-vous à la documentation sur les fonctionnalités spécifiques pour plus d’informations.

## <a name="server-manager-information"></a>Informations du Gestionnaire de serveur
Plusieurs fonctionnalités d’authentification peuvent être configurées à l’aide de la stratégie de groupe, qui peuvent être installés à l’aide du Gestionnaire de serveur. La fonctionnalité de Windows Biometric Framework est installée à l’aide du Gestionnaire de serveur. Autres rôles de serveur qui dépendent des méthodes d’authentification, telles que le serveur Web \(IIS\) et les Services de domaine Active Directory peuvent également être installés à l’aide du Gestionnaire de serveur.

## <a name="related-resources"></a>Ressources connexes

|Technologies d’authentification|Ressources|
|----------------|-------|
|Authentification Windows|[Présentation technique de l’authentification Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Inclut des rubriques traitant des différences entre les versions, les concepts d’authentification général, les scénarios d’ouverture de session, les architectures pour les versions prises en charge et des paramètres applicables.|
|Kerberos|[Vue d’ensemble de l’authentification Kerberos](../kerberos/kerberos-authentication-overview.md)<br /><br />[Vue d’ensemble de la délégation contrainte du protocole Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Référence technique de l’authentification Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<br /><br />[Guide de survie Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet Wiki\)|
|TLS\/SSL et DTLS \ (provider\ de prise en charge de sécurité Schannel)|[TLS / SSL & #40; Fournisseur SSP Schannel & #41; Vue d’ensemble](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Référence technique du fournisseur Schannel sécurité prise en charge](../tls/schannel-security-support-provider-technical-reference.md)|
|Authentification Digest|[Résumé des informations techniques de référence d’authentification](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[Vue d’ensemble NTLM](../kerberos/ntlm-overview.md)<br />Contient des liens vers des ressources actuelles et anciennes|
|PKU2U|[Présentation de PKU2U dans Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Carte à puce|[Référence technique de carte à puce](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|Informations d’identification|[Gestion et Protection des informations d’identification](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contient des liens vers des ressources actuelles et anciennes<br /><br />[Vue d’ensemble de mots de passe](../kerberos/passwords-overview.md)<br />Contient des liens vers des ressources actuelles et anciennes|


