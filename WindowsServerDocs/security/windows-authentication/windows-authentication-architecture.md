---
title: Architecture d’authentification Windows
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07c9d6bb-9b03-407d-89b6-97c7551b256b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 4a9deef6481c1f7dacb56e8166584de1c59d613c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403279"
---
# <a name="windows-authentication-architecture"></a>Architecture d’authentification Windows

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique de présentation pour les professionnels de l’informatique explique le schéma architectural de base pour l’authentification Windows.

L’authentification est le processus par lequel le système valide les informations d’ouverture de session ou de connexion d’un utilisateur. Le nom et le mot de passe d’un utilisateur sont comparés à une liste autorisée, et si le système détecte une correspondance, l’accès est accordé à l’étendue spécifiée dans la liste d’autorisations de cet utilisateur.

Dans le cadre d’une architecture extensible, les systèmes d’exploitation Windows Server implémentent un ensemble par défaut de fournisseurs de support de sécurité d’authentification, notamment Negotiate, le protocole Kerberos, NTLM, Schannel (canal sécurisé) et Digest. Les protocoles utilisés par ces fournisseurs permettent l’authentification des utilisateurs, des ordinateurs et des services, et le processus d’authentification permet aux utilisateurs et aux services autorisés d’accéder aux ressources de façon sécurisée.

Dans Windows Server, les applications authentifient les utilisateurs à l’aide de l’interface SSPI à des appels abstraits pour l’authentification. Ainsi, les développeurs n’ont pas besoin de comprendre la complexité des protocoles d’authentification spécifiques ou de créer des protocoles d’authentification dans leurs applications.

Les systèmes d’exploitation Windows Server incluent un ensemble de composants de sécurité qui composent le modèle de sécurité Windows. Ces composants garantissent que les applications ne peuvent pas accéder aux ressources sans authentification ni autorisation. Les sections suivantes décrivent les éléments de l’architecture d’authentification.

### <a name="local-security-authority"></a>Autorité de sécurité locale
L’autorité de sécurité locale (LSA, Local Security Authority) est un sous-système protégé qui authentifie et connecte les utilisateurs à l’ordinateur local. De plus, LSA gère les informations sur tous les aspects de la sécurité locale sur un ordinateur (ces aspects sont collectivement appelés stratégie de sécurité locale). Il fournit également différents services pour la traduction entre les noms et les identificateurs de sécurité (SID).

Le sous-système de sécurité effectue le suivi des stratégies de sécurité et des comptes qui se trouvent sur un système informatique. Dans le cas d’un contrôleur de domaine, ces stratégies et comptes sont ceux qui sont en vigueur pour le domaine dans lequel se trouve le contrôleur de domaine. Ces stratégies et comptes sont stockés dans Active Directory. Le sous-système LSA fournit des services pour valider l’accès aux objets, vérifier les droits d’utilisateur et générer des messages d’audit.

### <a name="security-support-provider-interface"></a>Interface du fournisseur de support de sécurité
L’interface SSPI (Security Support Provider Interface) est l’API qui obtient les services de sécurité intégrés pour l’authentification, l’intégrité des messages, la confidentialité des messages et la qualité de service de la sécurité pour tout protocole d’application distribuée.

SSPI est l’implémentation de l’API de service de sécurité générique (GSSAPI). SSPI fournit un mécanisme par lequel une application distribuée peut appeler l’un des différents fournisseurs de sécurité pour obtenir une connexion authentifiée sans connaître les détails du protocole de sécurité.

## <a name="see-also"></a>Voir aussi

-   [Architecture de l’interface du fournisseur SSP (Security Support Provider)](security-support-provider-interface-architecture.md)

-   [Processus des informations d’identification dans l’authentification Windows](credentials-processes-in-windows-authentication.md)

-   [Vue d’ensemble technique de l’authentification Windows](https://technet.microsoft.com/library/dn169029.aspx)


