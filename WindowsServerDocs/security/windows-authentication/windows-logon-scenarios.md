---
title: Scénarios d’ouverture de session Windows
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 07bfb538e1b43fc0c734b3c59b906c027ef985c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403300"
---
# <a name="windows-logon-scenarios"></a>Scénarios d’ouverture de session Windows

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique de référence pour les professionnels de l’informatique résume les scénarios courants de connexion et de connexion Windows.

Les systèmes d’exploitation Windows requièrent que tous les utilisateurs se connectent à l’ordinateur avec un compte valide pour accéder aux ressources locales et réseau. Les ordinateurs Windows sécurisent les ressources en implémentant le processus d’ouverture de session, dans lequel les utilisateurs sont authentifiés. Une fois qu’un utilisateur est authentifié, les technologies d’autorisation et de contrôle d’accès implémentent la deuxième phase de protection des ressources : déterminer si l’utilisateur authentifié est autorisé à accéder à une ressource.

Le contenu de cette rubrique s’applique aux versions de Windows indiquées dans la liste **s’applique à** au début de cette rubrique.

En outre, les applications et services peuvent exiger que les utilisateurs se connectent pour accéder aux ressources offertes par l’application ou le service. Le processus de connexion est similaire au processus d’ouverture de session, en ce qu’un compte valide et les informations d’identification correctes sont requis, mais les informations de connexion sont stockées dans la base de données du gestionnaire de comptes de sécurité (SAM) sur l’ordinateur local et dans le Active Directory, le cas échéant. Le compte de connexion et les informations d’identification sont gérés par l’application ou le service et peuvent éventuellement être stockés localement dans le module de verrouillage des informations d’identification.

Pour comprendre le fonctionnement de l’authentification, consultez [concepts d’authentification Windows](windows-authentication-concepts.md).

Cette rubrique décrit les scénarios suivants :

-   [Ouverture de session interactive](#BKMK_InteractiveLogon)

-   [Ouverture de session réseau](#BKMK_NetworkLogon)

-   [Ouverture de session par carte à puce](#BKMK_SmartCardLogon)

-   [Connexion biométrique](#BKMK_BioLogon)

## <a name="BKMK_InteractiveLogon"></a>Ouverture de session interactive
Le processus d’ouverture de session commence lorsqu’un utilisateur entre des informations d’identification dans la boîte de dialogue entrée d’identification, ou lorsque l’utilisateur insère une carte à puce dans le lecteur de carte à puce ou lorsque l’utilisateur interagit avec un périphérique biométrique. Les utilisateurs peuvent effectuer une ouverture de session interactive à l’aide d’un compte d’utilisateur local ou d’un compte de domaine pour se connecter à un ordinateur.

Le diagramme suivant montre les éléments d’ouverture de session interactifs et le processus d’ouverture de session.

![Diagramme montrant les éléments d’ouverture de session interactive et le processus d’ouverture de session](../media/windows-logon-scenarios/AuthN_LSA_Architecture_Client.gif)

**Architecture d’authentification du client Windows**

### <a name="BKMK_LocaDomainLogon"></a>Ouverture de session locale et de domaine
Les informations d’identification que l’utilisateur présente pour une ouverture de session de domaine contiennent tous les éléments nécessaires pour une ouverture de session locale, tels que le nom de compte et le mot de passe ou le certificat, ainsi que les informations de domaine de Active Directory. Le processus confirme l’identification de l’utilisateur dans la base de données de sécurité sur l’ordinateur local de l’utilisateur ou dans un domaine Active Directory. Ce processus d’ouverture de session obligatoire ne peut pas être désactivé pour les utilisateurs d’un domaine.

Les utilisateurs peuvent effectuer une ouverture de session interactive sur un ordinateur de deux manières :

-   Localement, lorsque l’utilisateur dispose d’un accès physique direct à l’ordinateur ou lorsque l’ordinateur fait partie d’un réseau d’ordinateurs.

    Une ouverture de session locale accorde à un utilisateur l’autorisation d’accéder aux ressources Windows sur l’ordinateur local. Une ouverture de session locale requiert que l’utilisateur dispose d’un compte d’utilisateur dans le gestionnaire de comptes de sécurité (SAM) sur l’ordinateur local. Le SAM protège et gère les informations d’utilisateur et de groupe sous la forme de comptes de sécurité stockés dans le registre de l’ordinateur local. L’ordinateur peut avoir un accès réseau, mais il n’est pas obligatoire. Les informations d’appartenance au groupe et au compte d’utilisateur local sont utilisées pour gérer l’accès aux ressources locales.

    Une ouverture de session réseau accorde à un utilisateur l’autorisation d’accéder aux ressources Windows sur l’ordinateur local en plus des ressources sur les ordinateurs en réseau telles que définies par le jeton d’accès des informations d’identification. Pour une ouverture de session locale et une ouverture de session réseau, l’utilisateur doit disposer d’un compte d’utilisateur dans le gestionnaire de comptes de sécurité (SAM) sur l’ordinateur local. Les informations d’appartenance au groupe et au compte d’utilisateur local sont utilisées pour gérer l’accès aux ressources locales, et le jeton d’accès pour l’utilisateur définit les ressources qui sont accessibles sur les ordinateurs en réseau.

    Une ouverture de session locale et une ouverture de session réseau ne sont pas suffisantes pour accorder à l’utilisateur et à l’ordinateur l’autorisation d’accéder à et d’utiliser les ressources de domaine.

-   À distance, par le biais des services Terminal Server ou Services Bureau à distance (RDS), auquel cas l’ouverture de session est plus qualifiée d’interactivité à distance.

Après une ouverture de session interactive, Windows exécute des applications pour le compte de l’utilisateur, et l’utilisateur peut interagir avec ces applications.

Une ouverture de session locale accorde à un utilisateur l’autorisation d’accéder aux ressources sur l’ordinateur local ou aux ressources sur les ordinateurs en réseau. Si l’ordinateur est joint à un domaine, la fonctionnalité Winlogon tente de se connecter à ce domaine.

Une ouverture de session de domaine accorde à un utilisateur l’autorisation d’accéder aux ressources locales et de domaine. Une ouverture de session de domaine nécessite que l’utilisateur dispose d’un compte d’utilisateur dans Active Directory. L’ordinateur doit disposer d’un compte dans le domaine Active Directory et être connecté physiquement au réseau. Les utilisateurs doivent également disposer des droits d’utilisateur pour se connecter à un ordinateur local ou à un domaine. Les informations de compte d’utilisateur de domaine et les informations d’appartenance au groupe sont utilisées pour gérer l’accès aux ressources locales et de domaine.

### <a name="BKMK_RemoteLogon"></a>Ouverture de session à distance
Dans Windows, l’accès à un autre ordinateur par le biais de l’ouverture de session à distance s’appuie sur le protocole RDP (Remote Desktop Protocol) (RDP). Étant donné que l’utilisateur doit déjà avoir ouvert une session sur l’ordinateur client avant de tenter une connexion à distance, les processus d’ouverture de session interactive se sont terminés avec succès.

RDP gère les informations d’identification entrées par l’utilisateur à l’aide du client Bureau à distance. Ces informations d’identification sont destinées à l’ordinateur cible, et l’utilisateur doit disposer d’un compte sur cet ordinateur cible. En outre, l’ordinateur cible doit être configuré pour accepter une connexion à distance. Les informations d’identification de l’ordinateur cible sont envoyées pour tenter d’effectuer le processus d’authentification. Si l’authentification réussit, l’utilisateur est connecté aux ressources locales et réseau qui sont accessibles à l’aide des informations d’identification fournies.

## <a name="BKMK_NetworkLogon"></a>Ouverture de session réseau
Une ouverture de session réseau ne peut être utilisée qu’une fois l’authentification de l’utilisateur, du service ou de l’ordinateur effectuée. Au cours de l’ouverture de session réseau, le processus n’utilise pas les boîtes de dialogue d’entrée des informations d’identification pour collecter des données. Au lieu de cela, les informations d’identification précédemment établies ou une autre méthode pour collecter les informations d’identification sont utilisées. Ce processus confirme l’identité de l’utilisateur à n’importe quel service réseau auquel l’utilisateur tente d’accéder. Ce processus est généralement invisible pour l’utilisateur, sauf si d’autres informations d’identification doivent être fournies.

Pour fournir ce type d’authentification, le système de sécurité comprend les mécanismes d’authentification suivants :

-   Protocole Kerberos version 5

-   Certificats de clé publique

-   Protocole SSL/Transport Layer Security (SSL/TLS)

-   Digest

-   NTLM, pour la compatibilité avec les systèmes basés sur Microsoft Windows NT 4,0

Pour plus d’informations sur les éléments et les processus, consultez le diagramme interactive Logon ci-dessus.

## <a name="BKMK_SmartCardLogon"></a>Ouverture de session par carte à puce
Les cartes à puce peuvent être utilisées pour se connecter uniquement aux comptes de domaine, et non aux comptes locaux. L’authentification par carte à puce requiert l’utilisation du protocole d’authentification Kerberos. Introduite dans Windows 2000 Server, dans les systèmes d’exploitation Windows, une extension de clé publique de la demande d’authentification initiale du protocole Kerberos est implémentée. Contrairement au chiffrement à clé secrète partagée, le chiffrement à clé publique est asymétrique, autrement dit, deux clés différentes sont nécessaires : une à chiffrer, une autre à déchiffrer. Ensemble, les clés requises pour effectuer les deux opérations constituent une paire de clés privée/publique.

Pour initier une session de connexion standard, un utilisateur doit prouver son identité en fournissant des informations connues uniquement de l’utilisateur et de l’infrastructure de protocole Kerberos sous-jacente. Les informations secrètes sont une clé partagée de chiffrement dérivée du mot de passe de l’utilisateur. Une clé secrète partagée est symétrique, ce qui signifie que la même clé est utilisée à la fois pour le chiffrement et le déchiffrement.

Le diagramme suivant montre les éléments et les processus requis pour l’ouverture de session par carte à puce.

![Diagramme montrant les éléments et les processus requis pour l’ouverture de session par carte à puce](../media/windows-logon-scenarios/SmartCardCredArchitecture.gif)

**Architecture du fournisseur d’informations d’identification de carte à puce**

Lorsqu’une carte à puce est utilisée à la place d’un mot de passe, une paire de clés publique/privée stockée sur la carte à puce de l’utilisateur est remplacée par la clé secrète partagée, qui est dérivée du mot de passe de l’utilisateur. La clé privée est stockée uniquement sur la carte à puce. La clé publique peut être mise à la disposition de toute personne avec laquelle le propriétaire souhaite échanger des informations confidentielles.

Pour plus d’informations sur le processus d’ouverture de session par carte à puce dans Windows, consultez [fonctionnement de la connexion par carte à puce dans Windows](https://technet.microsoft.com/library/ff404285.aspx).

## <a name="BKMK_BioLogon"></a>Connexion biométrique
Un appareil est utilisé pour capturer et créer une caractéristique numérique d’un artefact, tel qu’une empreinte digitale. Cette représentation numérique est ensuite comparée à un exemple du même artefact, et lorsque les deux sont comparées avec succès, l’authentification peut se produire. Les ordinateurs qui exécutent l’un des systèmes d’exploitation désignés dans la liste **s’applique à** au début de cette rubrique peuvent être configurés pour accepter cette forme de connexion. Toutefois, si l’ouverture de session biométrique est uniquement configurée pour une ouverture de session locale, l’utilisateur doit présenter des informations d’identification de domaine lors de l’accès à un domaine Active Directory.

## <a name="additional-resources"></a>Ressources supplémentaires
Pour plus d’informations sur la façon dont Windows gère les informations d’identification soumises pendant le processus d’ouverture de session, consultez [gestion des informations d’identification dans l’authentification Windows](https://technet.microsoft.com/library/dn169014.aspx).

[Présentation technique de l’authentification et de l’ouverture de session Windows](https://technet.microsoft.com/library/dn169029.aspx)


