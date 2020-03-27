---
title: Noms de domaine
description: Cette rubrique fournit une vue d’ensemble de l’utilisation de noms de domaine dans le traitement des demandes de connexion du serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dba00395b32980d3139cf88e25571c8001cac24e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316245"
---
# <a name="realm-names"></a>Noms de domaine

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016


Vous pouvez utiliser cette rubrique pour obtenir une vue d’ensemble de l’utilisation de noms de domaine dans le traitement des demandes de connexion au serveur de stratégie réseau.

L’attribut RADIUS de nom d’utilisateur est une chaîne de caractères qui contient généralement un emplacement de compte d’utilisateur et un nom de compte d’utilisateur. L’emplacement du compte d’utilisateur est également appelé nom de domaine ou nom de domaine, et est synonyme du concept de domaine, y compris les domaines DNS, les domaines de Active Directory® et les domaines Windows NT 4,0. Par exemple, si un compte d’utilisateur se trouve dans la base de données des comptes d’utilisateur pour un domaine nommé example.com, example.com est le nom de domaine.

Dans un autre exemple, si l’attribut RADIUS User-Name contient le nom d’utilisateur user1@example.com, User1 est le nom du compte d’utilisateur et example.com est le nom de domaine. Les noms de domaine peuvent être présentés dans le nom d’utilisateur sous la forme d’un préfixe ou d’un suffixe :

- **Example\user1**. Dans cet exemple, l' **exemple** de nom de domaine est un préfixe ; Il s’agit également du nom d’un Active Directory&reg; les services de domaine \(AD DS domaine\).

- <strong>user1@example.com</strong>. Dans cet exemple, le nom de domaine **example.com** est un suffixe. Il s’agit soit d’un nom de domaine DNS, soit du nom d’un domaine AD DS.

Vous pouvez utiliser des noms de domaine configurés dans les stratégies de demande de connexion lors de la conception et du déploiement de votre infrastructure RADIUS pour vous assurer que les demandes de connexion sont acheminées à partir de clients RADIUS, également appelés serveurs d’accès réseau, vers des serveurs RADIUS pouvant Authentifiez et autorisez la demande de connexion.

Lorsque NPS est configuré en tant que serveur RADIUS avec la stratégie de demande de connexion par défaut, NPS traite les demandes de connexion pour le domaine dont le serveur NPS est membre et pour les domaines approuvés.

Pour configurer NPS pour agir en tant que proxy RADIUS et transférer les demandes de connexion à des domaines non approuvés, vous devez créer une nouvelle stratégie de demande de connexion. Dans la nouvelle stratégie de demande de connexion, vous devez configurer l’attribut nom d’utilisateur avec le nom de domaine qui sera contenu dans l’attribut User-Name des demandes de connexion que vous souhaitez transférer. Vous devez également configurer la stratégie de demande de connexion avec un groupe de serveurs RADIUS distants. La stratégie de demande de connexion permet à NPS de calculer les demandes de connexion à transférer au groupe de serveurs RADIUS distants en fonction de la partie domaine de l’attribut User-Name.

## <a name="acquiring-the-realm-name"></a>Acquisition du nom de domaine

La partie nom de domaine du nom d’utilisateur est fournie lorsque l’utilisateur tape des informations d’identification basées sur un mot de passe lors d’une tentative de connexion ou lorsqu’un profil de gestionnaire de connexions (CM) sur l’ordinateur de l’utilisateur est configuré pour fournir automatiquement le nom de domaine.

Vous pouvez indiquer que les utilisateurs de votre réseau fournissent leur nom de domaine lors de la saisie de leurs informations d’identification lors des tentatives de connexion réseau.

Par exemple, vous pouvez demander aux utilisateurs de taper leur nom d’utilisateur, y compris le nom du compte d’utilisateur et le nom du domaine, dans **nom d’utilisateur** dans la boîte de dialogue **connecter** lors de l’établissement d’une connexion d’accès à distance ou d’un réseau privé virtuel (VPN).

En outre, si vous créez un package de numérotation personnalisé avec le kit d’administration de Connection Manager (CMAK), vous pouvez aider les utilisateurs en ajoutant automatiquement le nom de domaine au nom du compte d’utilisateur dans les profils CM installés sur les ordinateurs des utilisateurs. Par exemple, vous pouvez spécifier un nom de domaine et une syntaxe de nom d’utilisateur dans le profil CM afin que l’utilisateur puisse uniquement spécifier le nom du compte d’utilisateur lors de la saisie des informations d’identification. Dans ce cas, l’utilisateur n’a pas besoin de connaître ou de mémoriser le domaine dans lequel se trouve son compte d’utilisateur.

Pendant le processus d’authentification, une fois que les utilisateurs ont entré leurs informations d’identification basées sur un mot de passe, le nom d’utilisateur est transmis du client d’accès au serveur d’accès réseau. Le serveur d’accès réseau construit une demande de connexion et ajoute le nom de domaine dans l’attribut RADIUS de nom d’utilisateur dans le message de demande d’accès envoyé au serveur ou proxy RADIUS.

Si le serveur RADIUS est un serveur NPS, le message de demande d’accès est évalué par rapport à l’ensemble des stratégies de demande de connexion configurées. Les conditions de la stratégie de demande de connexion peuvent inclure la spécification du contenu de l’attribut User-Name.

Vous pouvez configurer un ensemble de stratégies de demande de connexion spécifiques au nom de domaine dans l’attribut User-Name des messages entrants. Cela vous permet de créer des règles de routage qui transfèrent les messages RADIUS avec un nom de domaine spécifique à un ensemble spécifique de serveurs RADIUS lorsque NPS est utilisé en tant que proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Règles de manipulation d’attribut

Avant que le message RADIUS soit traité localement (lorsque NPS est utilisé en tant que serveur RADIUS) ou transféré vers un autre serveur RADIUS (lorsque NPS est utilisé en tant que proxy RADIUS), l’attribut User-Name dans le message peut être modifié par les règles de manipulation d’attribut. Vous pouvez configurer des règles de manipulation d’attribut pour l’attribut User-Name en sélectionnant **nom d’utilisateur** sous l’onglet **conditions** dans les propriétés d’une stratégie de demande de connexion. Les règles de manipulation d’attribut NPS utilisent une syntaxe d’expression régulière.

Vous pouvez configurer des règles de manipulation d’attributs pour l’attribut User-Name afin de modifier les éléments suivants :

- Supprimez le nom de domaine du nom d’utilisateur \(également appelé\)de suppression de domaine. Par exemple, le nom d’utilisateur user1@example.com est défini sur User1.

- Modifiez le nom du domaine, mais pas sa syntaxe. Par exemple, le nom d’utilisateur user1@example.com est remplacé par user1@wcoast.example.com.

- Modifiez la syntaxe du nom de domaine. Par exemple, le nom d’utilisateur example\user1 est remplacé par user1@example.com.

Une fois l’attribut User-Name modifié conformément aux règles de manipulation d’attribut que vous configurez, des paramètres supplémentaires de la première stratégie de demande de connexion correspondante sont utilisés pour déterminer si :

- Le serveur NPS traite le message de demande d’accès localement (quand NPS est utilisé en tant que serveur RADIUS).

- Le serveur NPS transfère le message à un autre serveur RADIUS (lorsque NPS est utilisé en tant que proxy RADIUS).

## <a name="configuring-the-nps-supplied-domain-name"></a>Configuration du nom de domaine fourni par le serveur NPS

Lorsque le nom d’utilisateur ne contient pas de nom de domaine, NPS en fournit un. Par défaut, le nom de domaine fourni par le serveur NPS est le domaine dont le serveur NPS est membre. Vous pouvez spécifier le nom de domaine fourni par le serveur NPS via le paramètre de Registre suivant :

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>Une modification incorrecte du Registre peut sérieusement endommager votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

Certains serveurs d’accès réseau non-Microsoft suppriment ou modifient le nom de domaine tel que spécifié par l’utilisateur. Par conséquent, la demande d’accès réseau est authentifiée par rapport au domaine par défaut, qui peut ne pas être le domaine pour le compte de l’utilisateur. Pour résoudre ce problème, configurez vos serveurs RADIUS pour remplacer le nom d’utilisateur dans le format approprié par le nom de domaine exact.
