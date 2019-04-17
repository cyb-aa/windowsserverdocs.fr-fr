---
title: "Scénarios d’ouverture de session de Windows"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 66b7c568-67b7-4ac9-a479-a5a3b8a66142
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d6a1d52bee3c2d14ea83ccb794ecea1872868df5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="windows-logon-scenarios"></a>Scénarios d’ouverture de session de Windows

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique de référence destinée aux professionnels de l’informatique résume courantes ouverture de session Windows et les scénarios de connexion.

Les systèmes d’exploitation Windows requièrent tous les utilisateurs, ouvrez une session sur l’ordinateur avec un compte valide pour l’accès local et les ressources réseau. Ordinateurs Windows sécuriser les ressources en implémentant le processus d’ouverture de session, dans lequel les utilisateurs sont authentifiés. Une fois un utilisateur est authentifié, les technologies de contrôle d’autorisation et d’accès implémentent la seconde phase de protection des ressources: déterminer si l’utilisateur authentifié est autorisé à accéder à une ressource.

Le contenu de cette rubrique s’appliquent aux versions de Windows répertoriées dans le **s’applique aux** figurant au début de cette rubrique.

En outre, les applications et services peuvent nécessiter des utilisateurs de se connecter accéder aux ressources qui sont offertes par l’application ou le service. Le processus de connexion est similaire au processus d’ouverture de session, dans la mesure où un compte valide et les informations d’identification correctes sont requises, mais les informations d’ouverture de session sont stockées dans la base de données du Gestionnaire de comptes de sécurité (SAM) sur l’ordinateur local et dans Active Directory, le cas échéant. Connectez-vous compte informations d’identification et sont gérées par l’application ou le service et peuvent éventuellement être stockées localement dans le stockage sécurisé des informations d’identification.

Pour comprendre le fonctionnement de l’authentification, voir [Concepts de l’authentification Windows](windows-authentication-concepts.md).

Cette rubrique décrit les scénarios suivants:

-   [Ouverture de session interactive](#BKMK_InteractiveLogon)

-   [Ouverture de session réseau](#BKMK_NetworkLogon)

-   [Connexion par carte à puce](#BKMK_SmartCardLogon)

-   [Ouverture de session biométrique](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>Ouverture de session interactive
Le processus d’ouverture de session commence lorsqu’un utilisateur entre les informations d’identification dans la boîte de dialogue de saisie des informations d’identification, ou lorsque l’utilisateur insère une carte à puce dans le lecteur de carte à puce ou lorsque l’utilisateur interagit avec un périphérique biométrique. Les utilisateurs peuvent effectuer une ouverture de session interactive à l’aide d’un compte d’utilisateur local ou un compte de domaine pour vous connecter à un ordinateur.

Le diagramme suivant illustre les éléments de l’ouverture de session interactive et les processus d’ouverture de session.

![Diagramme montrant les éléments de l’ouverture de session interactive et les processus d’ouverture de session](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Architecture d’authentification Client Windows**

### <a name="BKMK_LocaDomainLogon"></a>Ouverture de session locale et de domaine
Informations d’identification de l’utilisateur présente une ouverture de session de domaine contiennent tous les éléments nécessaires pour une ouverture de session locale, telles que le nom de compte et mot de passe ou certificat et les informations de domaine Active Directory. Le processus confirme l’identification de l’utilisateur à la base de données de sécurité sur ordinateur local de l’utilisateur ou à un domaine Active Directory. Ce processus d’ouverture de session obligatoire ne peut pas être désactivé pour les utilisateurs dans un domaine.

Les utilisateurs peuvent effectuer une ouverture de session interactive à un ordinateur de deux manières:

-   Localement, lorsque l’utilisateur a un accès physique direct à l’ordinateur, ou lorsque l’ordinateur fait partie d’un réseau d’ordinateurs.

    Une ouverture de session locale accorde à un utilisateur l’autorisation d’accéder aux ressources de Windows sur l’ordinateur local. Une ouverture de session locale requiert que l’utilisateur a un compte d’utilisateur dans le Gestionnaire de SAM (Security Accounts) sur l’ordinateur local. SAM protège et gère les informations d’utilisateur et groupe sous la forme de comptes de sécurité stockées dans le Registre de l’ordinateur local. L’ordinateur peut avoir accès au réseau, mais il n’est pas requis. Informations de l’appartenance au compte et groupe utilisateur local sont utilisées pour gérer l’accès aux ressources locales.

    Ouverture de session réseau accorde à un utilisateur l’autorisation d’accéder aux ressources de Windows sur l’ordinateur local en plus de toutes les ressources sur les ordinateurs en réseau tel que défini par le jeton d’accès de l’information d’identification. Une ouverture de session locale et une ouverture de session réseau nécessitent que l’utilisateur a un compte d’utilisateur dans le Gestionnaire de SAM (Security Accounts) sur l’ordinateur local. Informations d’appartenance groupe et de compte utilisateur local sont utilisées pour gérer l’accès aux ressources locales, et le jeton d’accès pour l’utilisateur définit quelles ressources sont accessibles sur les ordinateurs en réseau.

    Une ouverture de session locale et une ouverture de session réseau ne sont pas suffisantes pour accorder l’autorisation de l’utilisateur et l’ordinateur d’accéder et d’utiliser les ressources du domaine.

-   À distance, à des Services Terminal Server ou Remote Desktop Services (RDS), auquel cas l’ouverture de session est plus qualifié comme distants interactive.

Après une ouverture de session interactive, Windows exécute des applications pour le compte de l’utilisateur, et l’utilisateur peut interagir avec ces applications.

Une ouverture de session locale accorde à une utilisateur l’autorisation d’accéder aux ressources sur l’ordinateur local ou de ressources sur les ordinateurs en réseau. Si l’ordinateur est joint à un domaine, la fonctionnalité Winlogon tente de se connecter à ce domaine.

Une ouverture de session de domaine accorde à une utilisateur l’autorisation d’accès local et les ressources du domaine. Une ouverture de session de domaine nécessite que l’utilisateur dispose d’un compte d’utilisateur dans Active Directory. L’ordinateur doit posséder un compte dans le domaine Active Directory et être physiquement connecté au réseau. Utilisateurs doivent également disposer des droits d’utilisateur pour vous connecter à un ordinateur local ou un domaine. Informations de compte utilisateur de domaine et les informations d’appartenance de groupe sont utilisés pour gérer l’accès au domaine et les ressources locales.

### <a name="BKMK_RemoteLogon"></a>Ouverture de session à distance
Dans Windows, l’accès à un autre ordinateur par le biais d’ouverture de session à distance s’appuie sur le protocole RDP (Remote Desktop). Étant donné que l’utilisateur doit déjà avoir correctement ouvert une session l’ordinateur client avant de tenter une connexion à distance, les processus d’ouverture de session interactive est terminée avec succès.

RDP gère les informations d’identification entrées par l’utilisateur à l’aide du Client Bureau à distance. Ces informations d’identification sont destinées à l’ordinateur cible, et l’utilisateur doit posséder un compte sur l’ordinateur cible. En outre, l’ordinateur cible doit être configuré pour accepter une connexion à distance. Les informations d’identification d’ordinateur cible sont envoyées pour tenter d’effectuer le processus d’authentification. Si l’authentification est réussie, l’utilisateur est connecté sur local et les ressources réseau qui sont accessibles en utilisant les informations d’identification fournies.

## <a name="BKMK_NetworkLogon"></a>Ouverture de session réseau
Une ouverture de session réseau peut uniquement être utilisé après que l’authentification utilisateur, service ou ordinateur a eu lieu. Lors de la connexion réseau, le processus n’utilise pas les boîtes de dialogue de saisie des informations d’identification pour collecter des données. Au lieu de cela, précédemment établie des informations d’identification ou une autre méthode pour collecter des informations d’identification est utilisée. Ce processus confirme l’identité de l’utilisateur à n’importe quel service de réseau que l’utilisateur tente d’accéder. Ce processus est généralement invisible pour l’utilisateur, sauf si les autres informations d’identification doivent être fournies.

Pour fournir ce type d’authentification, le système de sécurité inclut ces mécanismes d’authentification:

-   Protocole Kerberos version 5

-   Certificats de clé publique

-   Secure Sockets Layer/Transport Layer Security (SSL/TLS)

-   Digest

-   NTLM, pour la compatibilité avec les systèmes basés sur MicrosoftWindowsNT4.0

Pour plus d’informations sur les éléments et les processus, voir le diagramme ci-dessus d’ouverture de session interactive.

## <a name="BKMK_SmartCardLogon"></a>Connexion par carte à puce
Les cartes à puce peut servir à connecter uniquement aux comptes de domaine, les comptes non locaux. L’authentification par carte à puce exige l’utilisation du protocole d’authentification Kerberos. Introduite dans Windows 2000 Server, dans les systèmes d’exploitation Windows une extension de clé publique à la demande de l’authentification initiale du protocole est implémentée de Kerberos. Contrairement à un chiffrement à clé secrète partagé, le chiffrement à clé publique est asymétrique, autrement dit, deux clés différentes sont nécessaires: une pour chiffrer un autre pour déchiffrer. Les clés qui sont requis pour effectuer les deux opérations constituent une paire de clés publique/privée.

Pour lancer une session d’ouverture de session par défaut, un utilisateur doit prouver son identité en fournissant des informations connues uniquement à l’utilisateur et l’infrastructure du protocole Kerberos. Les informations confidentielles sont une clé partagée chiffrement dérivée de mot de passe de l’utilisateur. Une clé secrète partagée est symétrique, ce qui signifie que la même clé est utilisée pour le chiffrement et déchiffrement.

Le diagramme suivant illustre les éléments et les processus requis pour la connexion par carte à puce.

![Diagramme montrant les éléments et les processus requis pour la connexion par carte à puce](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Architecture de fournisseur d’informations d’identification de carte à puce**

Lorsqu’une carte à puce est utilisée au lieu d’un mot de passe, une paire de clés publique/privée sur carte à puce de l’utilisateur est remplacée par la clé secrète partagée, ce qui est dérivée de mot de passe de l’utilisateur. La clé privée est stockée uniquement sur la carte à puce. La clé publique peut être accessibles à toute personne avec laquelle le propriétaire souhaite échanger des informations confidentielles.

Pour plus d’informations sur le processus d’ouverture de session de carte à puce dans Windows, voir [carte à puce connectez-vous fonctionnement dans Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="BKMK_BioLogon"></a>Ouverture de session biométrique
Un appareil est utilisé pour capturer et générer une caractéristique numérique d’un objet, par exemple, une empreinte digitale. Cette représentation numérique est ensuite comparée à un exemple du même artifact, et lorsque les deux sont comparés avec succès, l’authentification peut se produire. Ordinateurs qui exécutent les systèmes d’exploitation désignées dans la **s’applique aux** figurant au début de cette rubrique peut être configuré pour accepter cet écran d’ouverture de session. Toutefois, si l’ouverture de session biométrique est configuré uniquement pour l’ouverture de session locale, l’utilisateur doit présenter les informations d’identification de domaine lors de l’accès à un domaine Active Directory.

## <a name="additional-resources"></a>Ressources supplémentaires
Pour plus d’informations sur la façon dont Windows gère les informations d’identification soumises au cours du processus d’ouverture de session, consultez [gestion des informations d’identification dans l’authentification Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Vue d’ensemble technique d’ouverture de session Windows et l’authentification](https://technet.microsoft.com/library/dn169029.aspx)


