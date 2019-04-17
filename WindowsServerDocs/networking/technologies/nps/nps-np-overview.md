---
title: Stratégies de réseau
description: Cette rubrique fournit une vue d’ensemble de stratégies de réseau pour le serveur NPS dans Windows Server2016 et inclut des liens vers des informations supplémentaires sur le serveur NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7e1604f4839bd955e5ea10d9eafea5ef0c978977
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-policies"></a>Stratégies de réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour une vue d’ensemble de stratégies de réseau dans NPS.

>[!NOTE]
>Outre cette rubrique, la documentation de stratégie réseau suivante est disponible.
> - [Autorisation d’accès](nps-np-access.md)
> - [Configurer des stratégies de réseau](nps-np-configure.md)

Stratégies de réseau sont des ensembles de conditions, des contraintes et des paramètres qui vous permettent de désigner les utilisateurs autorisés à se connecter au réseau et les circonstances dans lesquelles ils peuvent ou ne peut pas se connecter.

Lors du traitement des demandes de connexion en tant qu’un serveur d’authentification Dial-In utilisateur RADIUS (Remote Service), NPS effectue l’authentification et autorisation pour la demande de connexion. Pendant le processus d’authentification NPS vérifie l’identité de l’utilisateur ou un ordinateur qui se connecte au réseau. Pendant le processus d’autorisation, NPS détermine si l’utilisateur ou l’ordinateur est autorisé à accéder au réseau.

Pour rendre ces analyses, NPS utilise des stratégies de réseau qui sont configurés dans la console NPS. NPS examine également les propriétés de numérotation du compte d’utilisateur dans ActiveDirectory&reg; \(ADDS\) Services de domaine pour exécuter une autorisation.

## <a name="network-policies---an-ordered-set-of-rules"></a>Stratégies de réseau - un ensemble ordonné de règles

Stratégies de réseau peuvent être affichés sous forme de règles. Chaque règle dispose d’un ensemble de conditions et de paramètres. NPS compare les conditions de la règle aux propriétés de demandes de connexion. En cas de correspondance entre la règle et la demande de connexion, les paramètres définis dans la règle sont appliqués à la connexion.

Lorsque plusieurs stratégies réseau sont configurées dans NPS, ils sont un ensemble ordonné de règles. NPS vérifie chaque demande de connexion par rapport à la première règle dans la liste, puis le deuxième, etc., jusqu'à ce qu’une correspondance est trouvée.

Chaque stratégie réseau a un **état de la stratégie** paramètre qui vous permet d’activer ou désactiver la stratégie. Lorsque vous désactivez une stratégie réseau, NPS n’évalue pas la stratégie lors de l’autorisation des demandes de connexion.

>[!NOTE]
>Si vous voulez que NPS applique une stratégie réseau lors de l’autorisation pour les demandes de connexion, vous devez configurer le **état de la stratégie** paramètre en sélectionnant la stratégie activé la case à cocher.

## <a name="network-policy-properties"></a>Propriétés de stratégie de réseau

Il existe quatre catégories de propriétés pour chaque stratégie réseau:

### <a name="overview"></a>Vue d’ensemble

 Ces propriétés permettent de spécifier si la stratégie est activée, si la stratégie accorde ou refuse l’accès, et si une méthode de connexion réseau spécifique, ou le type de serveur d’accès réseau (NAS), est requis pour les demandes de connexion. Propriétés de la vue d’ensemble vous permettent également de spécifier si les propriétés de numérotation des comptes d’utilisateur dans ADDS sont ignorées. Si vous sélectionnez cette option, seuls les paramètres de la stratégie réseau sont utilisés par le serveur NPS pour déterminer si la connexion est autorisée.


### <a name="conditions"></a>Conditions

 Ces propriétés permettent de spécifier les conditions que la demande de connexion doit avoir afin de correspondre à la stratégie de réseau; si les conditions configurées dans la stratégie correspond à la demande de connexion, NPS applique les paramètres figurant dans la stratégie de réseau pour la connexion. Par exemple, si vous spécifiez l’adresse IPv4 NAS comme une condition de la stratégie réseau et NPS reçoit une demande de connexion à partir d’un NAS qui a l’adresse IP spécifiée, la condition de la stratégie correspond à la demande de connexion. 


### <a name="constraints"></a>Contraintes

 Les contraintes sont des paramètres supplémentaires de la stratégie de réseau qui sont nécessaires pour correspondre à la demande de connexion. Si la demande de connexion ne correspond pas à une contrainte, NPS la rejette automatiquement. Contrairement à la réponse NPS à des conditions qui ne correspondent pas dans la stratégie de réseau, si une contrainte n’est pas mis en correspondance, NPS refuse la demande de connexion sans l’évaluation des stratégies de réseau supplémentaires.

### <a name="settings"></a>Paramètres

 Ces propriétés permettent de spécifier les paramètres NPS s’applique à la demande de connexion si toutes les conditions de la stratégie réseau pour la stratégie sont mis en correspondance.

Lorsque vous ajoutez une nouvelle stratégie de réseau à l’aide de la console NPS, vous devez utiliser l’Assistant Nouvelle stratégie de réseau. Après avoir créé une stratégie de réseau à l’aide de l’Assistant, vous pouvez personnaliser la stratégie en double-cliquant sur la stratégie dans la console NPS pour obtenir les propriétés de la stratégie.

Pour les exemples de syntaxe des critères spéciaux pour spécifier les attributs de stratégie du réseau, consultez [utiliser des Expressions régulières dans NPS](nps-crp-reg-expressions.md).

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
