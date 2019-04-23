---
title: Architecture d’authentification Windows
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fd6e2cbc233a91b62e3c8c9caf1154f91c3863ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855860"
---
# <a name="windows-authentication-architecture"></a>Architecture d’authentification Windows

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de présentation pour les professionnels de l’informatique décrit le schéma de base architectural pour l’authentification Windows.

L’authentification est le processus par lequel le système valide ouverture de session d’un utilisateur ou les informations de connexion. Nom et le mot de passe d’un utilisateur sont comparées à une liste autorisée, et si le système détecte une correspondance, l’accès à l’étendue spécifiée dans la liste des autorisations pour cet utilisateur.

Dans le cadre d’une architecture extensible, les systèmes d’exploitation Windows Server implémentent un jeu de valeur par défaut sécurité prise en charge des fournisseurs d’authentification, notamment Negotiate, le protocole Kerberos, NTLM, Schannel (canal sécurisé) et Digest. Les protocoles utilisés par ces fournisseurs d’activer l’authentification des utilisateurs, ordinateurs et services, et le processus d’authentification permet d’utilisateurs et services autorisés d’accéder aux ressources de manière sécurisée.

Dans Windows Server, les applications s’authentifier les utilisateurs à l’aide de l’interface SSPI pour abstraire les appels pour l’authentification. Par conséquent, les développeurs est inutile de comprendre les complexités des protocoles d’authentification spécifiques ou de générer des protocoles d’authentification dans leurs applications.

Systèmes d’exploitation Windows Server incluent un ensemble de composants de sécurité qui composent le modèle de sécurité Windows. Ces composants vous assurer que les applications ne peut pas accéder aux ressources sans authentification et d’autorisation. Les sections suivantes décrivent les éléments de l’architecture de l’authentification.

### <a name="local-security-authority"></a>Autorité de sécurité locale
L’autorité de sécurité locale (LSA) est un sous-système protégé qui authentifie et connecte les utilisateurs à l’ordinateur local. En outre, LSA conserve les informations sur tous les aspects de la sécurité locale sur un ordinateur (ces aspects sont collectivement regroupés sous la stratégie de sécurité locale). Il fournit également les divers services de traduction entre les noms et les identificateurs de sécurité (SID).

Le sous-système de sécurité effectue le suivi des stratégies de sécurité et les comptes qui se trouvent sur un système informatique. Dans le cas d’un domaine contrôleur, ces stratégies et les comptes sont ceux qui sont en vigueur pour le domaine dans lequel se trouve le contrôleur de domaine. Ces stratégies et les comptes sont stockés dans Active Directory. Le sous-système LSA fournit des services pour la validation de l’accès aux objets, la vérification des droits d’utilisateur et en générant des messages d’audit.

### <a name="security-support-provider-interface"></a>Security Support Provider Interface
La prise en charge Interface SSPI (Security Provider) est l’API qui obtient des services de sécurité intégrée pour l’authentification, de l’intégrité des messages, de confidentialité des messages et de sécurité de qualité de service pour n’importe quel protocole d’application distribuée.

SSPI est l’implémentation de la sécurité Service API GSSAPI (Generic). SSPI fournit un mécanisme par lequel une application distribuée peut appeler un des fournisseurs de sécurité pour obtenir une connexion authentifiée sans avoir connaissance des détails du protocole de sécurité.

## <a name="see-also"></a>Voir aussi

-   [Architecture d’Interface fournisseur sécurité prise en charge](security-support-provider-interface-architecture.md)

-   [Processus d’informations d’identification de l’authentification Windows](credentials-processes-in-windows-authentication.md)

-   [Présentation technique de l’authentification Windows](https://technet.microsoft.com/library/dn169029.aspx)


