---
title: Noms de domaine
description: Cette rubrique fournit une vue d’ensemble de l’utilisation de noms de domaine dans la demande de connexion du serveur de stratégie réseau de traitement dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 65a272873a60d74efcf417a16fdc84670f5878da
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447003"
---
# <a name="realm-names"></a>Noms de domaine

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016


Vous pouvez utiliser cette rubrique pour une vue d’ensemble de l’utilisation de noms de domaine dans le traitement de demande de connexion serveur NPS.

L’attribut de nom d’utilisateur RADIUS est une chaîne de caractères qui contient généralement un emplacement de compte d’utilisateur et un nom de compte d’utilisateur. L’emplacement du compte utilisateur est également appelé domaine ou nom de domaine et est synonyme avec le concept de domaine, y compris les domaines DNS, les domaines d’Active et les domaines Windows NT 4.0. Par exemple, si un compte d’utilisateur se trouve dans la base de données de comptes utilisateur pour un domaine nommé example.com, exemple.com est le nom de domaine.

Dans un autre exemple, si l’attribut de nom d’utilisateur RADIUS contient le nom d’utilisateur user1@example.com, user1 est le nom de compte d’utilisateur et exemple.com est le nom de domaine. Noms de domaine peuvent être présentés dans le nom d’utilisateur sous la forme d’un préfixe ou un suffixe :

- **Example\user1**. Dans cet exemple, le nom de domaine **exemple** est un préfixe ; et il est également le nom d’un annuaire Active Directory&reg; les Services de domaine \(AD DS\) domaine.

- <strong>user1@example.com</strong>. Dans cet exemple, le nom de domaine **example.com** est un suffixe ; et il est un nom de domaine DNS ou le nom d’un domaine AD DS.

Vous pouvez utiliser des noms de domaine configurés dans les stratégies de demande de connexion lors de la conception et le déploiement de votre infrastructure RADIUS pour vous assurer que les demandes de connexion sont routés à partir de clients RADIUS, également appelés serveurs d’accès réseau aux serveurs RADIUS qui peuvent authentifier et autoriser la demande de connexion.

Lorsque NPS est configuré comme un serveur RADIUS avec la stratégie de demande de connexion par défaut, il traite les demandes de connexion pour le domaine dans lequel le serveur NPS est membre et pour les domaines approuvés.

Pour configurer NPS en tant que proxy RADIUS et les demandes de connexion directe à des domaines non approuvés, vous devez créer une nouvelle stratégie de demande de connexion. Dans la nouvelle stratégie de demande de connexion, vous devez configurer l’attribut de nom d’utilisateur avec le nom de domaine qui est contenus dans l’attribut de nom d’utilisateur de demandes de connexion que vous souhaitez transférer. Vous devez également configurer la stratégie de demande de connexion avec un groupe de serveurs RADIUS distants. La stratégie de demande de connexion permet au serveur NPS calculer les demandes de connexion pour transférer vers le groupe de serveurs RADIUS distant en fonction de la partie domaine de l’attribut de nom d’utilisateur.

## <a name="acquiring-the-realm-name"></a>L’acquisition du nom de domaine

La partie du nom de domaine du nom d’utilisateur est fournie lorsque les types en fonction du mot de passe informations d’identification utilisateur lors d’une connexion Essayez ou lorsqu’un profil Connection Manager (CM) sur l’ordinateur est configuré pour fournir le nom de domaine automatiquement.

Vous pouvez désigner les utilisateurs de votre réseau à fournir leur nom de domaine lors de la saisie de leurs informations d’identification lors de tentatives de connexion de réseau.

Par exemple, vous pouvez demander aux utilisateurs de taper leur nom d’utilisateur, y compris le nom de compte d’utilisateur et le nom de domaine, dans **nom d’utilisateur** dans le **Connect** boîte de dialogue lors de l’établissement d’un réseau privé virtuel ou d’accès à distance (VPN) connexion.

En outre, si vous créez un package de numérotation personnalisé avec le Kit d’Administration de Connection Manager (CMAK), vous pouvez aider les utilisateurs en ajoutant le nom de domaine automatiquement au nom du compte d’utilisateur dans les profils CM qui sont installés sur les ordinateurs des utilisateurs. Par exemple, vous pouvez spécifier une syntaxe de nom de nom et l’utilisateur de domaine dans le profil CM afin que l’utilisateur doit uniquement spécifier le nom de compte d’utilisateur lors de la saisie des informations d’identification. Dans ce cas, l’utilisateur n’a pas besoin à connaître ou Mémoriser le domaine où se trouve son compte d’utilisateur.

Pendant le processus d’authentification, une fois que les utilisateurs tapent leurs informations d’identification par mot de passe, le nom d’utilisateur est passé à partir du client d’accès au serveur d’accès réseau. Le serveur d’accès réseau construit une demande de connexion et inclut le nom de domaine au sein de l’attribut de nom d’utilisateur RADIUS dans le message de demande d’accès qui est envoyé au serveur ou proxy RADIUS.

Si le serveur RADIUS est un serveur NPS, le message de demande d’accès est évalué par rapport au jeu de stratégies de demande de connexion configurée. Conditions sur la stratégie de demande de connexion peuvent inclure la spécification du contenu de l’attribut de nom d’utilisateur.

Vous pouvez configurer un ensemble de stratégies de demande de connexion qui sont spécifiques au nom de domaine au sein de l’attribut de nom d’utilisateur des messages entrants. Cela vous permet de créer des règles de routage qui transfèrent les messages RADIUS avec un nom de domaine spécifique à un ensemble spécifique de serveurs RADIUS lorsque NPS est utilisé en tant que proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Règles de manipulation d’attribut

Avant que le message RADIUS est traité localement (lorsque le serveur NPS est utilisé comme un serveur RADIUS) ou transféré vers un autre serveur RADIUS (lorsque le serveur NPS est utilisé en tant que proxy RADIUS), l’attribut de nom d’utilisateur dans le message peut être modifié par les règles de manipulation d’attribut. Vous pouvez configurer des règles de manipulation d’attribut pour l’attribut de nom d’utilisateur en sélectionnant **nom d’utilisateur** sur le **Conditions** onglet dans les propriétés d’une stratégie de demande de connexion. Les règles de manipulation d’attribut NPS utilisent la syntaxe d’expression régulière.

Vous pouvez configurer des règles de manipulation d’attribut pour l’attribut de nom d’utilisateur à modifier les éléments suivants :

- Supprimer le nom de domaine à partir du nom d’utilisateur \(également appelé domaine jointes\). Par exemple, le nom d’utilisateur user1@example.com est modifié à l’utilisateur user1.

- Modifier le nom de domaine, mais pas sa syntaxe. Par exemple, le nom d’utilisateur user1@example.com est remplacée par user1@wcoast.example.com.

- Modifier la syntaxe du nom de domaine. Par exemple, l’example\user1 de nom d’utilisateur est modifié en user1@example.com.

Une fois que l’attribut de nom d’utilisateur est modifiée selon les règles de manipulation d’attribut que vous configurez, les paramètres supplémentaires de la première stratégie de demande de connexion correspondante sont utilisés pour déterminer si :

- Le serveur NPS traite le message de demande d’accès localement (lorsque le serveur NPS est utilisé comme un serveur RADIUS).

- Le serveur NPS transfère le message vers un autre serveur RADIUS (lorsque le serveur NPS est utilisé en tant que proxy RADIUS).

## <a name="configuring-the-nps-supplied-domain-name"></a>Configuration du nom de domaine fourni par NPS

Lorsque le nom d’utilisateur ne contient pas un nom de domaine, NPS fournit un. Par défaut, le nom de domaine fourni par NPS est le domaine dont le serveur NPS est un membre. Vous pouvez spécifier le nom de domaine NPS fourni via le paramètre de Registre suivant :

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

Certains serveurs d’accès réseau non-Microsoft supprimer ou modifier le nom de domaine tel que spécifié par l’utilisateur. Par conséquent, la demande d’accès réseau est authentifiée par rapport au domaine par défaut, ce qui peut ne pas être le domaine pour le compte d’utilisateur. Pour résoudre ce problème, configurez vos serveurs RADIUS pour modifier le nom d’utilisateur dans le format correct avec le nom de domaine.
