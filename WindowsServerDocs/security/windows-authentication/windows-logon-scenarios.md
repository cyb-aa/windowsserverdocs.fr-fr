---
title: Scénarios d’ouverture de session Windows
description: Sécurité de Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828180"
---
# <a name="windows-logon-scenarios"></a>Scénarios d’ouverture de session Windows

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de référence pour les professionnels de l’informatique résume l’ouverture de session Windows courants et scénarios de connexion.

Les systèmes d’exploitation Windows nécessitent tous les utilisateurs à ouvrir une session sur l’ordinateur avec un compte valide pour l’accès local et les ressources réseau. Les ordinateurs basés sur Windows sécuriser les ressources en implémentant le processus d’ouverture de session, dans lequel les utilisateurs sont authentifiés. Après avoir authentifié un utilisateur, les technologies de contrôle d’autorisation et d’accès implémentent la deuxième phase de protection des ressources : déterminer si l’utilisateur authentifié est autorisé à accéder à une ressource.

Le contenu de cette rubrique s’appliquent aux versions de Windows désignés dans le **s’applique à** figurant au début de cette rubrique.

En outre, les applications et services peuvent nécessiter des utilisateurs à se connecter pour accéder à ces ressources sont proposés par l’application ou le service. Le processus de connexion est similaire au processus d’ouverture de session, dans la mesure où un compte valide et les informations d’identification correctes sont requises, mais les informations d’ouverture de session sont stockées dans la base de données du Gestionnaire de comptes de sécurité (SAM) sur l’ordinateur local et dans Active Directory, le cas échéant. Compte et les informations d’identification informations de connexion sont gérées par l’application ou le service et peuvent éventuellement être stockées localement dans le stockage sécurisé des informations d’identification.

Pour comprendre le fonctionnement de l’authentification, consultez [Concepts d’authentification Windows](windows-authentication-concepts.md).

Cette rubrique décrit les scénarios suivants :

-   [Ouverture de session interactive](#BKMK_InteractiveLogon)

-   [Ouverture de session réseau](#BKMK_NetworkLogon)

-   [Ouverture de session de carte à puce](#BKMK_SmartCardLogon)

-   [Ouverture de session biométrique](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>Ouverture de session interactive
Le processus d’ouverture de session commence lorsqu’un utilisateur entre des informations d’identification dans la boîte de dialogue de saisie des informations d’identification, ou lorsque l’utilisateur insère une carte à puce dans le lecteur de carte à puce, ou lorsque l’utilisateur interagit avec un périphérique biométrique. Les utilisateurs peuvent effectuer une ouverture de session interactive à l’aide d’un compte d’utilisateur local ou un compte de domaine pour vous connecter à un ordinateur.

Le diagramme suivant illustre les éléments de l’ouverture de session interactive et les processus d’ouverture de session.

![Diagramme montrant les éléments de l’ouverture de session interactive et le processus d’ouverture de session](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Architecture de l’authentification du Client Windows**

### <a name="BKMK_LocaDomainLogon"></a>Ouverture de session locale et de domaine
Informations d’identification que l’utilisateur présente une ouverture de session de domaine contiennent tous les éléments nécessaires pour une ouverture de session locale, telles que le nom du compte et mot de passe ou certificat et les informations de domaine Active Directory. Le processus confirme l’identification de l’utilisateur à la base de données de sécurité sur l’ordinateur local de l’utilisateur ou à un domaine Active Directory. Ce processus d’ouverture de session obligatoire ne peut pas être désactivé pour les utilisateurs dans un domaine.

Les utilisateurs peuvent effectuer une ouverture de session interactive sur un ordinateur de deux manières :

-   Localement, lorsque l’utilisateur a un accès physique direct à l’ordinateur, ou lorsque l’ordinateur fait partie d’un réseau d’ordinateurs.

    Une ouverture de session locale accorde une autorisation de l’utilisateur d’accéder aux ressources de Windows sur l’ordinateur local. Une ouverture de session locale nécessite que l’utilisateur a un compte d’utilisateur dans le Gestionnaire de comptes de sécurité (SAM) sur l’ordinateur local. Le module SAM protège et gère les informations d’utilisateur et groupe sous la forme de comptes de sécurité stockées dans le Registre de l’ordinateur local. L’ordinateur peut avoir accès au réseau, mais il n’est pas obligatoire. Informations d’appartenance au groupe et de compte utilisateur local sont utilisées pour gérer l’accès aux ressources locales.

    Une ouverture de session réseau accorde une autorisation de l’utilisateur d’accéder aux ressources de Windows sur l’ordinateur local en plus de toutes les ressources sur les ordinateurs en réseau tel que défini par le jeton d’accès de l’information d’identification. Une ouverture de session locale et une ouverture de session réseau nécessitent que l’utilisateur a un compte d’utilisateur dans le Gestionnaire de comptes de sécurité (SAM) sur l’ordinateur local. Informations d’appartenance au groupe et de compte utilisateur local sont utilisées pour gérer l’accès aux ressources locales, et le jeton d’accès pour l’utilisateur définit quelles ressources sont accessibles sur les ordinateurs en réseau.

    Une ouverture de session locale et une ouverture de session réseau ne sont pas suffisantes pour accorder l’autorisation utilisateur et ordinateur pour accéder à et utiliser des ressources du domaine.

-   À distance, par le biais des Services Terminal Server ou Remote Desktop Services (RDS), auquel cas l’ouverture de session est plus qualifié comme à distance interactive.

Après une ouverture de session interactive, Windows exécute des applications pour le compte de l’utilisateur et l’utilisateur peut interagir avec ces applications.

Une ouverture de session locale accorde une autorisation de l’utilisateur à accéder aux ressources sur l’ordinateur local ou des ressources sur les ordinateurs en réseau. Si l’ordinateur est joint à un domaine, la fonctionnalité de Winlogon tente de se connecter à ce domaine.

Une ouverture de session de domaine accorde une autorisation de l’utilisateur d’accès local et les ressources du domaine. Une ouverture de session de domaine nécessite que l’utilisateur dispose d’un compte d’utilisateur dans Active Directory. L’ordinateur doit posséder un compte dans le domaine Active Directory et être physiquement connecté au réseau. Les utilisateurs doivent disposer également les droits d’utilisateur pour vous connecter à un ordinateur local ou un domaine. Informations de compte utilisateur de domaine et les informations d’appartenance de groupe sont utilisés pour gérer l’accès au domaine et des ressources locales.

### <a name="BKMK_RemoteLogon"></a>Ouverture de session à distance
Dans Windows, l’accès à un autre ordinateur par le biais d’ouverture de session à distance s’appuie sur le protocole RDP (Remote Desktop). Étant donné que l’utilisateur doit déjà avoir correctement connecté à l’ordinateur client avant de tenter une connexion à distance, les processus d’ouverture de session interactive est terminée avec succès.

RDP gère les informations d’identification entrées par l’utilisateur à l’aide du Client Bureau à distance. Ces informations d’identification sont destinées à l’ordinateur cible, et l’utilisateur doit disposer d’un compte sur l’ordinateur cible. En outre, l’ordinateur cible doit être configuré pour accepter une connexion à distance. Les informations d’identification d’ordinateur cible sont envoyées à tente de lancer le processus d’authentification. Si l’authentification réussit, l’utilisateur est connecté à local et ressources réseau qui sont accessibles en utilisant les informations d’identification fournies.

## <a name="BKMK_NetworkLogon"></a>Ouverture de session réseau
Une ouverture de session réseau utilisable uniquement une fois que l’authentification utilisateur, service ou ordinateur a eu lieu. Au cours de l’ouverture de session réseau, le processus n’utilise pas les boîtes de dialogue de saisie des informations d’identification pour collecter des données. Au lieu de cela, établie précédemment les informations d’identification ou une autre méthode pour collecter des informations d’identification est utilisée. Ce processus confirme l’identité de l’utilisateur à n’importe quel service de réseau qui tente d’accéder à l’utilisateur. Ce processus est généralement invisible pour l’utilisateur, sauf si les autres informations d’identification doivent être fournis.

Pour fournir ce type d’authentification, le système de sécurité inclut ces mécanismes d’authentification :

-   Protocole Kerberos version 5

-   Certificats de clé publique

-   Secure Sockets Layer/Transport Layer Security (SSL/TLS)

-   Digest

-   NTLM, pour la compatibilité avec les systèmes basés sur Microsoft Windows NT 4.0

Pour plus d’informations sur les éléments et les processus, consultez le diagramme d’ouverture de session interactive ci-dessus.

## <a name="BKMK_SmartCardLogon"></a>Ouverture de session de carte à puce
Cartes à puce peut servir à connecter uniquement aux comptes de domaine, les comptes non locaux. L’authentification de carte à puce requiert l’utilisation du protocole d’authentification Kerberos. Introduite dans Windows 2000 Server, dans les systèmes d’exploitation Windows une extension de la clé publique à la demande d’authentification initiale du protocole est implémentée de Kerberos. Contrairement à un chiffrement à clé secrète partagé, le chiffrement à clé publique est asymétrique, autrement dit, deux clés différentes sont nécessaires : un à chiffrer, un autre à déchiffrer. Les clés qui sont requis pour effectuer les deux opérations constituent une paire de clés publique/privée.

Pour lancer une session d’ouverture de session typique, un utilisateur doit prouver son identité en fournissant des informations ne connues que de l’utilisateur et l’infrastructure du protocole Kerberos sous-jacente. Les informations confidentielles sont une clé de chiffrement partagée dérivée de mot de passe de l’utilisateur. Une clé secrète partagée est symétrique, ce qui signifie que la même clé est utilisée pour le chiffrement et déchiffrement.

Le diagramme suivant illustre les éléments et les processus requis pour la connexion de la carte à puce.

![Diagramme montrant les éléments et les processus requis pour la connexion de la carte à puce](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Architecture de fournisseur d’informations d’identification de carte à puce**

Lorsqu’une carte à puce est utilisée au lieu d’un mot de passe, une paire de clés publique/privée stockée sur la carte à puce est remplacée par la clé secrète partagée, qui est dérivée de mot de passe de l’utilisateur. La clé privée est stockée uniquement sur la carte à puce. La clé publique peut être accessible à toute personne avec qui le propriétaire souhaite échanger des informations confidentielles.

Pour plus d’informations sur le processus d’ouverture de session de carte à puce dans Windows, consultez [comment fonctionne l’authentification de carte à puce dans Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="BKMK_BioLogon"></a>Ouverture de session biométrique
Un appareil est utilisé pour capturer et créer une caractéristique numérique d’un artefact, comme une empreinte digitale. Cette représentation numérique est ensuite comparée à un échantillon de l’artefact de même, et lorsque les deux sont comparés avec succès, l’authentification peut se produire. Ordinateurs qui exécutent les systèmes d’exploitation désignées dans la **s’applique à** figurant au début de cette rubrique peut être configuré pour accepter cet écran d’ouverture de session. Toutefois, si l’ouverture de session biométrique est configurée uniquement pour l’ouverture de session locale, l’utilisateur doit présenter les informations d’identification de domaine lors de l’accès à un domaine Active Directory.

## <a name="additional-resources"></a>Ressources supplémentaires
Pour plus d’informations sur la façon dont Windows gère les informations d’identification soumises pendant le processus d’ouverture de session, consultez [gestion des informations d’identification dans l’authentification Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Vue d’ensemble technique d’ouverture de session Windows et authentification](https://technet.microsoft.com/library/dn169029.aspx)


